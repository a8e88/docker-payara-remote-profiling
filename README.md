# docker-payara-remote-profiling
Note how to set up remote profiling with payara container and jconsole.

## Settp remote ssh
1. Allow tunnel to 4848 and 6001, In fact 4848 is payara admin web console port, you can replace 6001 with any port number you want.

## Run payara container
```sh
docker run \
-p 127.0.0.1:4848:4848 \
-p 127.0.0.1:6001:6001 \
-d payara/server-full
```
### Note
In example I expose port to host only, because I plan to use ssh tunnel to monitoring profile.

## Set JVM option in payara
You need to add jvm option to payara, go to admin web console --> Configurations --> server-config --> JVM settings --> JVM Options
```sh
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.port=6001
-Dcom.sun.management.jmxremote.rmi.port=6001
-Dcom.sun.management.jmxremote.ssl=false
-Djava.rmi.server.hostname=127.0.0.1
```
### Note: 
1. java.rmi.server.hostname is ip address of your server(host that you run type docker run command), In my case I use 127.0.0.1 because I want to use ssh tunnel instead of direct connect to public ip address of my server. 
2. -Dcom.sun.management.jmxremote.port and -Dcom.sun.management.jmxremote.rmi.port should be same value.
3. restart payara domain after modify jvm options

### Connect to remote payara
1. Start JConsole and select remote address
2. use 127.0.0.1:6001 for remote address


