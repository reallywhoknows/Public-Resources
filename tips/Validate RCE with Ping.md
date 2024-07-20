# Validating RCE with Ping
Often RCE (Remote Code Execution) will be completely blind, but we can infer some information based on a variety of factors, but one of the easiest ways to confirm that code execution is happening (even if our shells aren't catching!) is to see if we can get some network traffic back to our Attacking machine.

In this example, I will be using one of the vulnerable machines from OffSec's PEN-200 but do not worry there will be no spoilers or details regarding the box.

## We have code execution (I think?)
In this scenario, let's say you've found an exploit and are trying to get a reverse shell. You cannot get feedback (such as installed libraries) in order to specifically target something installed, and you begin doubting whether your RCE even works or not.

In this case, there are two tools I typically use to validate RCE **Ping** and **Curl**. These are typically installed on linux or windows and can be used to validate our RCE let's look.

![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/rce-w-ping1.png?raw=true)

It says it works but I don't know if it's true or not!

Let's start with **Ping**

**Ping** uses a protocol called *icmp* which may not always respond on a host. We may need to verify that we don't automatically block that sort of traffic. We'll start with starting a listener in Kali and specify *icmp* traffic.
```
sudo tcpdump -n -i tun0 icmp
```
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/rce-w-ping2.png?raw=true)

Now let's go ahead and attempt our RCE with the payload `ping kali_ip`. As a special note, make sure if you're pinging on linux to limit the amount of pings using `-c4` or it'll never stop bothering you.
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/rce-w-ping3.png?raw=true)

Another method **curl**

**Curl** is a pretty simple, but extremely useful tool that lets us pool web traffic from our command line. We'll want to host a simple webserver with python which will include logs whenever someone accesses it or downloads something. This can be used to validate that our victim is executing our code.
```
python3 -m http.server 80
```
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/rce-w-ping4.png?raw=true)

Now all we have to is edit our payload utilizing **curl** and see if our webserver gets an visitors. `curl http://kali_ip/`
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/rce-w-ping5.png?raw=true)

Perfect! Now with this, we know for sure code execution is happening, we just need to trouble shoot *why* our payload isn't executing as intended, whether it's a firewall, write permissions or binaries not being available.