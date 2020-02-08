# Indy-Connection-Tutorial

This tutorial concerns setting up a connecton to a server using Indy components.<br>
It will use an example that connects to an NNTP server.  The IDE used is from CodeTyphon.<br>
(The 144.76.182.167 host being used is a free NNTP server.)

It will demonstrate these connections:<br>
 <b>utNoTLSSupport</b> &nbsp;(no SSL secure connection)<br>
 <b>utUseExplicitTLS</b> &nbsp;(A client first establishes an unsecured connection to a server and then<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  issues a specific command to that server to establish an SSL link.)<br>
 <b>utUseImplicitTLS</b> &nbsp;(A mechanism whereby security is automatically turned on as soon as a<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  client makes a connection to a server.).<br>

<hr>

<p>
<i><b>Component Setup</b></i>
</p>

<p>
<b>A. Create a form and find the Indy tabs on the Lazarus (or CodeTyphon) IDE.</b>
</p>
<p align="left">
  <img src="/image/Indy.png" width="533" height="85">
</p>
<br>
<p>
<b>B. Add a panel with 3 radiobuttons and caption them as shown.<br></b>
&nbsp;&nbsp;&nbsp;&nbsp;Rename the top radiobutton to NoTLS1<br>
&nbsp;&nbsp;&nbsp;&nbsp;Rename the middle radiobutton to Explicit119TLS1<br>
&nbsp;&nbsp;&nbsp;&nbsp;Rename the bottom radiobutton to Implicit563TLS1<br>
</p>
<p align="left">
 <img src="/image/radiobuttons.png" width="411" height="91">
</p>
<br>
<p>
<b>C. For our demonstration, add 3 IdNNTP components on the form.</b><br>
&nbsp;&nbsp;&nbsp;&nbsp;(This is going to allow both no SSL and use SSL. It is easier<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;to use 3 IdNNTP components, rather than trying to switch<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 IdNNTP component between each connection type.)
</p>
<p align="left">
 <img src="/image/Indy2.png" width="197" height="98"><br>
 <img src="/image/IdNNTPn.png" width="411" height="134">
</p>
<br>
<p>
<b>D. Now add a required I/O handler.</b><br>
&nbsp;&nbsp;&nbsp;&nbsp;An SSL handler is being used here, but it will work to connect<br>
&nbsp;&nbsp;&nbsp;&nbsp;a nonSSL connection also.  (For non SSL only, a TIdIOHandlerStack<br>
&nbsp;&nbsp;&nbsp;&nbsp;can be used.)<br>
</p>
<p align="left">
   <img src="/image/Indy3.png" width="197" height="98">
 <br>
   <img src="/image/IdSSLIOHandlerSocketOpenSSL1.png" width="411" height="134">
</p> 
<br>
<p>
<b>E. Next, add a socks info handler IdSocksInfo1.</b>
</p>
<p align="left">
 <img src="/image/Indy5.png" width="197" height="98"><br>
 <img src="/image/IdSocksInfo1.png" width="411" height="134">
</p> 
<br>
<p>
<b>F. Lastly, add 2 buttons and a check box.</b><br>
&nbsp;&nbsp;&nbsp;&nbsp;1. Change the top button caption to Connect and name to Connect1<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. Change the bottom button caption to Disconnect and name to Disconnect1<br>
&nbsp;&nbsp;&nbsp;&nbsp;3. Change the checkbox caption to 'Use Tor' and name to UseTor1<br>
</p>
<p align="left">
 <img src="/image/buttons.png" width="411" height="134">
</p> 

<hr>

<i><b>Component Parameters Setup</b></i>
<br><br>
<b>IdNNTP Components</b><br>
<p>
<b>A. IdNNTP1 will be set for a non-SSL connection.</b><br>
 </p>
<p align="left">
 <img src="/image/IdNNTPObInsNoTSL.png" width="240" height="390">
</p>
<br>
<p>
<b>B. IdNNTP2 will be set for an Explicit TLS connection.</b><br>
</p>
<p align="left">
 <img src="/image/IdNNTPObInsExpTSL.png" width="240" height="390">
</p> 
<br>
<p> 
<b>C. IdNNTP3 will be set for an Implicit TLS connection.</b><br> 
</p> 
<p align="left">
 <img src="/image/IdNNTPObInsImpTSL.png" width="240" height="390">
</p> 
<br> 
<p> 
<b>IdSSLIOHandlerSocketOpenSSL1 Component</b><br>
(Only need to expand the SSLOptions area and<br>
&nbsp;change Method to sslvTLSv1_2).
</p>
<p>
 <img src="/image/IdSSLIOHandlerSocketOpenSSL1OI.png" width="240" height="660"> 
</p>
<br> 
<p> 
<b>IdSocksInfo1 Component</b><br>
Set the Host to 127.0.0.1, Port to 9050, and Version to<br>
svSocks5 for use with Tor (Tor must be started externally).<br>
</p>
<p>
 <img src="/image/IdSocksInfo1OI.png" width="240" height="370"> 
</p> 

<hr>

<i><b>Code</b></i><br><br>
```
unit Unit1;

{$mode objfpc}{$H+}

interface

uses
    Windows, Forms, Dialogs, StdCtrls, Buttons, ExtCtrls, Classes, SysUtils,
    IdNNTP, IdIOHandlerStack, IdSSLOpenSSL, IdSocks;

type
{ TForm1 }

TForm1 = class(TForm)
     Connect1: TButton;
     ConnectionPanel1: TPanel;
     Disconnect1: TButton;
     Explicit119TLS1: TRadioButton;
     IdIOHandlerStack1: TIdIOHandlerStack;
     IdNNTP1: TIdNNTP;
     IdNNTP2: TIdNNTP;
     IdNNTP3: TIdNNTP;
     IdSocksInfo1: TIdSocksInfo;
     IdSSLIOHandlerSocketOpenSSL1: TIdSSLIOHandlerSocketOpenSSL;
     Implicit563TLS1: TRadioButton;
     Label1: TLabel;
     NoTLS1: TRadioButton;
     UseTOR1: TCheckBox;

     procedure Connect1Click(Sender: TObject);
     procedure Disconnect1Click(Sender: TObject);
     procedure IdNNTP1Connected(Sender: TObject);
     procedure IdNNTP1Disconnected(Sender: TObject);
     procedure UseTOR1Click(Sender: TObject);

     private
     { private declarations }
     public
     { public declarations }
end;

var
Form1: TForm1;

       IdNNTP_No: TIdNNTP;

implementation

{$R *.lfm}

{ TForm1 }

//Connect button
procedure TForm1.Connect1Click(Sender: TObject);
begin
     if NoTLS1.Checked = True then // Port 119 No TLS Support
        IdNNTP_No := IdNNTP1
        else
        if Explicit119TLS1.Checked = True then // Port 119 Use Explicit TLS
           IdNNTP_No := IdNNTP2
           else
           if Implicit563TLS1.Checked = True then // Port 563 Use Implicit TLS
              IdNNTP_No := IdNNTP3
              else begin
              ShowMessage('Connection button not pressed!');
              EXIT;
              end;

     try
       if not IdNNTP_No.Connected then
          IdNNTP_No.Connect;
     except
       on E: Exception do begin
             if pos('10061', E.Message) > 0 then
                ShowMessage('Server down, no internet connection, or TOR may not be running!')
                else
                ShowMessage(E.Message);

             Disconnect1.Enabled := False;
             end;
     end;
end;

//IdNNTP1, IdNNTP2, and IdNNTP3 OnConnected event
procedure TForm1.IdNNTP1Connected(Sender: TObject);
begin
     Connect1.Enabled := False;
     UseTOR1.Enabled := False;
     Disconnect1.Enabled := True;
end;

//Disconnect button
procedure TForm1.Disconnect1Click(Sender: TObject);
begin
     if IdNNTP_No.Connected then
     IdNNTP_No.Disconnect;
end;

//IdNNTP1, IdNNTP2, and IdNNTP3 OnDisconnected event
procedure TForm1.IdNNTP1Disconnected(Sender: TObject);
begin
     Connect1.Enabled := True;
     UseTOR1.Enabled := True;
     Disconnect1.Enabled := False;
end;

//UseTOR1 checkbox OnClick event
procedure TForm1.UseTOR1Click(Sender: TObject);
begin
     if UseTOR1.Checked = True then
        IdSSLIOHandlerSocketOpenSSL1.TransparentProxy := IdSocksInfo1
        else
        IdSSLIOHandlerSocketOpenSSL1.TransparentProxy := nil;
end;

end.
```
 
 







