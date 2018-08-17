# Indy-Connection-Tutorial

This tutorial concerns setting up a connecton to a server using the Indy components.<br>
It was created due to the lack of examples on the web as to how to effectively do this.<br>
It will use an example to connection to an NNTP server.  The IDE used is from CodeTyphon.

It will demonstrate these connections:  
 utNoTLSSupport (no SSL secure connection)  
 utUseExplicitTLS and utUseImplicitTLS (both types of SSL secure connection).

<p>
A. Create a form and find the Indy tabs on the Lazarus IDE.
</p>
<p align="left">
  <img src="/image/Indy.png" width="533" height="85">
</p>
<p>
 
B. Add a panel with 3 radio buttons and caption them accordingly.
</p>
<p align="left">
 <img src="/image/radiobuttons.png" width="411" height="91">
</p>
<p>
 
C. Find the IdNNTP component on the 'Indy Clients Protocols (nz)' tab.
</p>
<p align="left">
  <img src="/image/Indy2.png" width="197" height="98">
</p> 
<p>
 
D. For our demonstration, put 3 IdNNTP components on the form.<br>
&nbsp;&nbsp;&nbsp;&nbsp;(This is going to allow both no SSL and use SSL.  It is easier<br>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;to use 3 IdNNTP components, rather than trying to switch<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 IdNNTP component between each connection type.)
</p>
<p align="left">
   <img src="/image/IdNNTPn.png" width="411" height="134">
</p>
<p>
 
E. Now add a required I/O handler.  An SSL handler is being used<br>
&nbsp;&nbsp;&nbsp;&nbsp;here, but it will work to connect a non SSL connection also.
</p>
<p align="left">
   <img src="/image/Indy3.png" width="197" height="98">
 <br>
   <img src="/image/IdSSLIOHandlerSocketOpenSSL1.png" width="411" height="134">
</p> 

<p>
F. Now add a socks info handler.
</p>
<p align="left">
 <img src="/image/Indy4.png" width="197" height="98"><br>
<img src="/image/IdSSLIOHandlerSocketOpenSSL1.png" width="411" height="134">
</p> 
 
