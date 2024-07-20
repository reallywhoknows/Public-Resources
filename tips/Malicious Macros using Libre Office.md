# Crafting Malicious Macros with Libre Office.
Just like with Microsoft Office macros, we can also craft malicious macros for Libre Office. I recently came across the need to do this, since office wasn't readily available for me at that time. Of course, I would recommend you use Office if that's best suited, and you know for sure that it will be used, but in this case, I knew Libre Office was being used to open the files. The entire process is incredibly similar to Microsoft Office and other then a couple of minor setting differences. If you've done one, the other won't be too unusual. We'll start with creating a new file in Libre Office. The contents of the file don't really matter, but if you're trying to make this look legit, put some effort into it!

## Creating the Macro
We'll need to go to the following location in order to create our macro.
`Tools -> Macros -> Organize Macros -> Basic`
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/libre-office-macros1.png?raw=true)

Next we'll select our document and choose "New", then give it a name.
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/libre-office-macros2.png?raw=true)

We are given a pretty basic code block to work with, but we can execute commands using the `Shell` function and passing through our command.
( More on the shell Function https://help.libreoffice.org/6.1/he/text/sbasic/shared/03130500.html )
```
REM  *****  BASIC  *****

Sub Main

End Sub
```

In my scenario context, the target machine is running Windows, so I know that my command will need to go through CMD. If you're doing this on a Linux machine, be mindful that the syntax I am providing will naturally not work.

I'll be using the following command to execute a powershell payload.
```
cmd /c powershell
```

The full content of my payload will be a base64 powershell reverse shell.
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/libre-office-macros3.png?raw=true)

## Automated Execution
Those of you who are familiar with creating macros on Windows Office products will likely notice there is nothing in the macro to automatically execute on open. This is in fact correct, in Libre Office there is an additional setting that can be changed.
`Tools -> Customize -> Events`
![alt text](https://github.com/reallywhoknows/Public-Resources/blob/main/tips/Images/libre-office-macros4.png?raw=true)

Once we have this applied, the document is opened and it'll automatically execute our payload. As with any type of phishing attack, we really want to verify that this is working beforehand.


I went ahead and executed it on a Windows VM in order to verify it. While we can't know exactly what the victims' environment may be like, we at minimum can confirm that the payload will work if it isn't restricted.