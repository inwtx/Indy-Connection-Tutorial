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
<b>A. Create a form and find the Indy tabs on the Lazarus IDE.</b>
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
&nbsp;&nbsp;&nbsp;&nbsp; a nonSSL connection also.  (For non SSL only, a TIdIOHandlerStack<br>
&nbsp;&nbsp;&nbsp;&nbsp; can be used.)<br>
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
&nbsp;change the Method to sslvTLSv1_2).
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
unit Unit1;<br>
<br>
{$mode objfpc}{$H+}<br>
<br>
interface<br>
<br>
uses<br>
&nbsp;&nbsp;&nbsp;&nbsp;    Windows, Forms, Dialogs, StdCtrls, Buttons, ExtCtrls, Classes, SysUtils,<br>
&nbsp;&nbsp;&nbsp;&nbsp;    IdNNTP, IdIOHandlerStack, IdSSLOpenSSL, IdSocks;<br>
<br>
type<br>
{ TForm1 }<br>
<br>
TForm1 = class(TForm)<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Connect1: TButton;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     ConnectionPanel1: TPanel;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Disconnect1: TButton;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Explicit119TLS1: TRadioButton;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdIOHandlerStack1: TIdIOHandlerStack;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdNNTP1: TIdNNTP;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdNNTP2: TIdNNTP;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdNNTP3: TIdNNTP;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdSocksInfo1: TIdSocksInfo;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     IdSSLIOHandlerSocketOpenSSL1: TIdSSLIOHandlerSocketOpenSSL;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Implicit563TLS1: TRadioButton;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Label1: TLabel;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     NoTLS1: TRadioButton;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     UseTOR1: TCheckBox;<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;     procedure Connect1Click(Sender: TObject);<br>
&nbsp;&nbsp;&nbsp;&nbsp;     procedure Disconnect1Click(Sender: TObject);<br>
&nbsp;&nbsp;&nbsp;&nbsp;     procedure IdNNTP1Connected(Sender: TObject);<br>
&nbsp;&nbsp;&nbsp;&nbsp;     procedure IdNNTP1Disconnected(Sender: TObject);<br>
&nbsp;&nbsp;&nbsp;&nbsp;     procedure UseTOR1Click(Sender: TObject);<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;     private<br>
&nbsp;&nbsp;&nbsp;&nbsp;     { private declarations }<br>
&nbsp;&nbsp;&nbsp;&nbsp;     public<br>
&nbsp;&nbsp;&nbsp;&nbsp;     { public declarations }<br>
end;<br>
<br>
var<br>
Form1: TForm1;<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;       IdNNTP_No: TIdNNTP;<br>
<br>
implementation<br>
<br>
{$R *.lfm}<br>
<br>
{ TForm1 }<br>
<br>
//Connect button<br>
procedure TForm1.Connect1Click(Sender: TObject);<br>
begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;     if NoTLS1.Checked = True then // Port 119 No TLS Support<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        IdNNTP_No := IdNNTP1<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        else<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        if Explicit119TLS1.Checked = True then // Port 119 Use Explicit TLS<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;           IdNNTP_No := IdNNTP2<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;           else<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;           if Implicit563TLS1.Checked = True then // Port 563 Use Implicit TLS<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;              IdNNTP_No := IdNNTP3<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;              else begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;              ShowMessage('Connection button not pressed!');<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;              EXIT;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;              end;<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;     try<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       if not IdNNTP_No.Connected then<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       IdNNTP_No.Connect;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     except<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       on E: Exception do begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;             if pos('10061', E.Message) > 0 then<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                ShowMessage('Server down, no internet connection, or TOR may not be running!')<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                else<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                ShowMessage(E.Message);<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;             Disconnect1.Enabled := False;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;             end;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     end;<br>
end;<br>
<br>
//IdNNTP1, IdNNTP2, and IdNNTP3 OnConnected event<br>
procedure TForm1.IdNNTP1Connected(Sender: TObject);<br>
begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Connect1.Enabled := False;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     UseTOR1.Enabled := False;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Disconnect1.Enabled := True;<br>
end;<br>
<br>
//Disconnect button<br>
procedure TForm1.Disconnect1Click(Sender: TObject);<br>
begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;     if IdNNTP_No.Connected then<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     IdNNTP_No.Disconnect;<br>
end;<br>
<br>
//IdNNTP1, IdNNTP2, and IdNNTP3 OnDisconnected event<br>
procedure TForm1.IdNNTP1Disconnected(Sender: TObject);<br>
begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Connect1.Enabled := True;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     UseTOR1.Enabled := True;<br>
&nbsp;&nbsp;&nbsp;&nbsp;     Disconnect1.Enabled := False;<br>
end;<br>
<br>
//UseTOR1 checkbox OnClick event<br>
procedure TForm1.UseTOR1Click(Sender: TObject);<br>
begin<br>
&nbsp;&nbsp;&nbsp;&nbsp;     if UseTOR1.Checked = True then<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        IdSSLIOHandlerSocketOpenSSL1.TransparentProxy := IdSocksInfo1<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        else<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;        IdSSLIOHandlerSocketOpenSSL1.TransparentProxy := nil;<br>
end;<br>
<br>
end.<br>

 
 







