FROM gradle:jdk17-alpine AS build
COPY --chown=gradle:gradle . /home/gradle/src
WORKDIR /home/gradle/src
# by the way, this --no-daemon flag and my other attempts to stop the gradle-daemon
# are futile: gradle will always launch gradle-daemon in a container image env
RUN gradle build --no-daemon

# For some reason I need this silly debugging message here or else
# the above step takes very very long. now it only takes long (220s)
RUN echo Hello I finished with build

FROM openjdk:17-alpine3.14

EXPOSE 8080

RUN mkdir /app
# maybe: should be /home/.gradle/src/build/libs/*.jar
COPY --from=build /home/gradle/src/build/libs/*.jar /app/spring-boot-application.jar

ENTRYPOINT ["java", "-jar","/app/spring-boot-application.jar"]
# ENTRYPOINT ["java", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-Djava.security.egd=file:/dev/./urandom","-jar","/app/spring-boot-application.jar"]

# DEBUG mismatching JRE and JDK
# Compiled by Java SE 17 = 61, Java runtime recognizes Java SE 8 = 52,
# solution: change JRE to match JDK

### IMPORTANT
### sunday update:
# bradleygoldsmith@Bradleys-MacBook-Air api % docker run --platform linux/x86_64/v8 -it -p 3001:3000 backend sh
# Unrecognized VM option 'UseCGroupMemoryLimitForHeap'
# Error: Could not create the Java Virtual Machine.
# Error: A fatal exception has occurred. Program will exit.
# need -jar flag for sure,....,


# when building: must include --platform linux/x86_64/v8 flag if using a mac with M1 or M2 chip
# likewise when running the image from a mac with M1 or M2 chip
# build:
#   docker build --platform linux/x86_64/v8 -t backend .             
# run as shell instance:
#   docker run --platform linux/x86_64/v8 -it -p 3001:3000 backend sh

# the current error I get when running the image:
# > docker run --platform linux/x86_64/v8 -it -p 3001:3000 backend sh
# OpenJDK 64-Bit Server VM warning: Unable to open cgroup memory limit file /sys/fs/cgroup/memory/memory.limit_in_bytes (No such file or directory)
# Error: A JNI error has occurred, please check your installation and try again
# Exception in thread "main" java.lang.sUnsupportedClassVersionError: org/springframework/boot/loader/JarLauncher has been compiled by a more recent version of the Java Runtime (class file version 61.0), this version of the Java Runtime only recognizes class file versions up to 52.0
#         at java.lang.ClassLoader.defineClass1(Native Method)
#         at java.lang.ClassLoader.defineClass(ClassLoader.java:756)
#         at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
#         at java.net.URLClassLoader.defineClass(URLClassLoader.java:473)
#         at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
#         at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
#         at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
#         at java.security.AccessController.doPrivileged(Native Method)
#         at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
#         at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
#         at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:352)
#         at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
#         at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:601)
