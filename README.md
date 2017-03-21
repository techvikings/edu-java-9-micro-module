# Compile

```bash
$ javac --module-source-path src -d out $(find . -name '*.java')

$ java --module-path out --module com.example.helloworld/com.example.helloworld.HelloWorld
Hello World!
```
# Build JAR

```bash
$ cd out

$ jar --create --main-class com.example.helloworld.HelloWorld --file helloworld.jar -C com.example.helloworld .

$ java -jar helloworld.jar
Hello World!

$ mkdir jar

$ mv helloworld.jar jar/helloworld.jar
```

# Create Runtime Image

```bash
$ cd /project

$ jlink --module-path out/jar:$JAVA_HOME/jmods --add-modules com.example.helloworld --strip-debug --output out/image

$ ls -la out/image
total 5
drwxrwx--- 1 1000 1000    0 Mar 20 10:27 .
drwxrwx--- 1 1000 1000    0 Mar 20 10:27 ..
drwxrwx--- 1 1000 1000    0 Mar 20 10:27 bin
drwxrwx--- 1 1000 1000    0 Mar 20 10:27 conf
drwxrwx--- 1 1000 1000    0 Mar 20 10:27 include
drwxrwx--- 1 1000 1000 4096 Mar 20 10:27 lib
-rwxrwx--- 1 1000 1000  168 Mar 20 10:27 release

$ ls -la out/image/bin/
total 24
drwxrwx--- 1 1000 1000     0 Mar 20 10:27 .
drwxrwx--- 1 1000 1000     0 Mar 20 10:27 ..
-rwxrwx--- 1 1000 1000   136 Mar 20 10:27 com.example.helloworld
-rwxrwx--- 1 1000 1000  9256 Mar 20 10:27 java
-rwxrwx--- 1 1000 1000 13408 Mar 20 10:27 keytool

$ out/image/bin/com.example.helloworld
Hello World!

$ du -hs out/image/
37M     out/image/
```


# Optimize Runtime Image
```bash
$ jlink --module-path out/jar:$JAVA_HOME/jmods --add-modules com.example.helloworld --strip-debug --compress=0 --output out/image-compressed0

$ du -hs out/image-compressed0
32M     out/image-compressed0

$ jlink --module-path out/jar:$JAVA_HOME/jmods --add-modules com.example.helloworld --strip-debug --compress=1 --output out/image-compressed1

$ du -hs out/image-compressed1
28M     out/image-compressed1

$ jlink --module-path out/jar:$JAVA_HOME/jmods --add-modules com.example.helloworld --strip-debug --compress=2 --output out/image-compressed2

$ du -hs out/image-compressed2
28M     out/image-compressed2
```