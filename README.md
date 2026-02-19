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




  
- HackTheBox Machines
- PortSwigger Labs

## Certifications

- ISC2 Certified in Cybersecurity (in progress)


