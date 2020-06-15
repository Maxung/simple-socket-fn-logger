# simple-socket-fn-logger

## About

A simple socket server that can be used as a logging endpoint for your Oracle Functions! At bare minimum, it's a great tool to get near realtime logging for your Oracle Functions that are deployed in the Oracle Cloud. But it can be more than that! If you want, you can modify `Main.groovy` to persist your log data (maybe to a [free Autonomous DB instance](https://oracle.com/cloud/free))! The syslog format contains a log of data and this logger just outputs the message contents. You have access to a `Map` of data that looks like so:

```json
{
    "syslog.header.appName": "app_id=ocid1.fnapp.oc1.phx...,fn_id=ocid1.fnfunc.oc1.phx...",
    "syslog.header.version": "1",
    "syslog.header.hostName": "runner-00001700e5f9",
    "syslog.header.facility": "1",
    "syslog.header.msgId": "app_id=ocid1.fnapp.oc1.phx...,fn_id=ocid1.fnfunc.oc1.phx...",
    "syslog.header.timestamp": "2020-06-15T14:46:35Z",
    "syslog.message": "Error in function: ReferenceError: foo is not defined",
    "syslog.header.pri": "11",
    "syslog.header.procId": "8",
    "syslog.header.severity": "3"
}
```

Feel free to extend this as needed!

## Usage

You can use a pre-compiled version of this server, or compile it yourself.  

### Pre-compiled

Download the JAR file, place it in a directory and run the JAR (requires Java 11 JDK installed). See [Running The Server](#running-the-server).

### Compiling

To compile, use Gradle:

```shell script
$ ./gradlew shadowJar
```

This will create a runnable JAR in the `build/libs` directory.  See [Running The Server](#running-the-server).

### Running The Server

To run the server:

```shell script
java -jar simple-socket-fn-logger-[version]-all.jar
```

This will start up a socket server on the default port of 30000. If you want to use a different port, pass it in:

```shell script
java -jar simple-socket-fn-logger-[version]-all.jar
```

>[!WARNING]
>Check firewall ports, routers, security lists, etc to make sure the port is open! 

This **can** be run on `localhost`, but your syslog URL must be your public IP and your router/firewall should forward the port as necessary!

### Config Oracle Function Application

You'll need to set the syslog URL to point at your new socket server. You can do this via the CLI:

```shell script
$ fn update app syslog-demo-app --syslog-url tcp://[your public IP]:[socket server port]
```

Or via the console:

![set syslog url via console](https://objectstorage.us-phoenix-1.oraclecloud.com/n/toddrsharp/b/readme-assets/o/2020-06-15_10-58-38.png)

It's worth repeating that this **can** be run on `localhost`, but your syslog URL must be your public IP and your router/firewall should forward the port as necessary!
