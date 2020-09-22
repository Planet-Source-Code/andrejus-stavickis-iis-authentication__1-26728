<div align="center">

## IIS authentication


</div>

### Description

this code provides a username/password of username/domain to be used on MS IIS to authenticate users. This code don't ivolve any authentication algorythm - it uses basic or Windows authentication made by IIS server. I didn't saw any such code here. This code is only an example, but you can modify it as you wish.
 
### More Info
 
no input parameters, therefore IIS supplies authentication method and algorythm.

The reason i wrote this code without any auth mechanizm is that in my opinion, developer should focus on systems development, and authentication should be left for operating systems - it much more secure, than authenticating on your own, but involves user registration in local/global users database. But if you don't want this user to login interactively, restrict the user "logon to" and add a IIS workstation/server name - this will eliminate the possibility for user to login remotely and access any resources on your computer. Actually this code is usefull in corporate-wide environment, when local employees, nedds an access to some WEB interface, but using this they will don't need to enter any username/password as the IE will do it automatically in "intranet" zone, and when working remotely, they will be prompted for username/password/domain. You must enable basic and Windows integrated authentication to let IIS authenticate user against Windows user database/Active Directory.

domain\username in case of negotiate authentication

and

username:password in case of basic auth. but you really don't need any passwords, because if your code is being run, thats mean IIS already authenticated a user against domain/computer.

actually no any, except the code will not provide a password in negotiate auth.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Andrejus Stavickis](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/andrejus-stavickis.md)
**Level**          |Advanced
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |ASP \(Active Server Pages\) 
**Category**       |[Internet/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-html__1-34.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/andrejus-stavickis-iis-authentication__1-26728/archive/master.zip)





### Source Code

```
<%
' if you will uncomment these lines, and open an
' ASP file in your browser, you will see ALL
' strings and parameters, IIS server is passing
' to ASP script - as well as ASP session ID
' and other usefull stuff like client/server IP
'
' ccountst.ServerVariables.Count
' for i=1 to ccount
' stri=Request.ServerVariables.Item(i)
' Response.Write(I & "*" & stri & " <HR>****")
' next
 stri=Request.ServerVariables.Item(7)
 auth=Request.ServerVariables.Item(6)
  If LCase(Left(Auth, 9)) = "negotiate" Then
   Pos = InStr(stri, "\")
   If Pos > 1 Then
   LOGON_Domain = Left(stri, Pos - 1)
   LOGON_User = Mid(stri, Pos + 1)
   End If
   Response.Write("User Domain: " & logon_domain & "<br>")
  end if
  If LCase(Left(Auth, 4)) = "ntlm" Then
   Pos = InStr(stri, "\")
   If Pos > 1 Then
   LOGON_Domain = Left(stri, Pos - 1)
   LOGON_User = Mid(stri, Pos + 1)
   End If
   Response.Write("User Domain: " & logon_domain & "<br>")
  end if
  If LCase(Left(Auth, 5)) = "basic" Then
   Pos = InStr(stri, ":")
   If Pos > 1 Then
   LOGON_User = Mid(stri, Pos + 1)
   End If
   If Pos = 0 Then
   LOGON_User = stri
   End If
   logon_password=Request.ServerVariables.Item(5)
   Response.Write("User Domain: [default] (specified in IIS Admin).<br>")
  end if
 Response.Write("User Name: " & logon_User & "<br>")
  If LCase(Left(Auth, 5)) = "basic" Then
   Response.Write("User Password: " & logon_password & "<br>")
  end if
  If LCase(Left(Auth, 9)) = "negotiate" Then
   Response.Write("User Password: [Windows authenticated a user] <br>")
  end if
  If LCase(Left(Auth, 4)) = "ntlm" Then
   Response.Write("User Password: [Windows authenticated a user] <br>")
  end if
%>
```

