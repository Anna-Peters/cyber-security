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
  Framework version and changelog can tell about potential vulnnerabilities
  Accessing the directories like /assets can give unpermittedd access
  DOM structuree might contain secret pages
  "Pretty print" to read js files in the source, can be done by pressing {} button

  Favicon can be degugged in terminal with command curl URL | md5sum to define the framework via https://wiki.owasp.org/index.php/OWASP_favicon_database, after that the framework page can  tell if any default admin credentials are still available to exploit the system
  Robot.txt can be checked for any content not allowedd to be seen: http://.../robot.txt
  In sitemap, the pages which should be parsed by crawlers, can be checked for secret pages too .../sitemap.xml

   Try running the below curl command against the web server, where the -v switch enables verbose mode, which will output the headers (there might be something interesting!). curl http://... -v
 OSINT  Google Hacking / Dorking https://en.wikipedia.org/wiki/Google_hacking


  
- HackTheBox Machines
- PortSwigger Labs

## Certifications

- ISC2 Certified in Cybersecurity (in progress)


