DISCOVERING HIDDEN DIRECTORIES

METHODOLOGY
To identify hidden or unlinked directories on the target website (http://testphp.vulnweb.com/), I used Gobuster, a directory and file brute-forcing tool. The enumeration was performed using a common wordlist (common.txt) provided by the SecLists project.

FINDINGS
The scan revealed the following directories and files of interest:

Directory / File	Status Code	Description
/admin/	301	Redirects to admin panel – potentially sensitive
/cgi-bin/	403	Forbidden – commonly contains scripts; possible entry point
/CVS/	200	Exposed version control system directory – misconfigured
/CVS/Entries	200	VCS metadata – could reveal file structure
/CVS/Root	200	CVS root config – may leak repository info
/CVS/Repository	200	May show internal repository names
/crossdomain.xml	200	Flash cross-domain policy file – inspect for misconfigurations
/images/	200	Publicly accessible – may host media or leak metadata
/pictures/	301	Redirect – similar to /images/
/secured/	301	Redirects to restricted resource – inspect for auth bypass
/vendor/	301	May expose package manager files (e.g., PHP Composer)


CONCLUSION
Sensitive and misconfigured directories like /admin, /secured, and /CVS were discovered, which could potentially be leveraged for further exploitation.
403 responses such as /cgi-bin/ suggest access control is in place, but these paths still exist and could be brute-forced or fuzzed further.
Exposure of CVS version control files is a misconfiguration and may leak source code or file structure of the application.
crossdomain.xml should be examined to ensure it doesn’t allow excessive cross-origin access.
This enumeration step is crucial for identifying attack surfaces and misconfigurations during web application assessments.

Code / Script Used
Gobuster Command
gobuster dir \
  -u http://testphp.vulnweb.com/ \
  -w /usr/share/wordlists/dirb/common.txt \
  -t 40 \
  -o gobuster-results.txt



  
