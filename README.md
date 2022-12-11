# CVE-2021-41773

CVE-2021-41773
According to The National Vulnerability Database (NVD) CVE-2021-41773, Apache HTTP Server 2.4.49 is vulnerable to Path Traversal and Remote Code execution attacks.

# Path Traversal
The path traversal vulnerability was introduced due to the new code change added for path normalization i.e., for URL paths to remove unwanted or dangerous parts from the pathname,

but it was inadequate to detect different techniques of encoding the path traversal characters “dot-dot-slash (../)”
To prevent path traversal attacks, the normalization function which is responsible to resolve URL-encoded values from the requested URI, resolved Unicode values one at a time. Hence when URL encoding the second dot as %2e , the logic fails to recognize %2e as dot thereby not decoding it, this converts the characters ../ to .%2e/ and bypasses the check.

Along with Path traversal check bypass, for an Apache HTTP server to be vulnerable, the HTTP Server configuration should either contain the directory directive for entire server’s filesystem as Require all granted or the directory directive should be completely missing from the configuration file.
Therefore, bypassing the dot-dot check as .%2e and chaining it with misconfigured directory directive allows an attacker to read arbitrary files such as passwd from the vulnerable server file system.

# Remote Code Execution
While CVE-2021-41773 was initially documented as Path traversal and File disclosure vulnerability additional research concluded that the vulnerability can be further exploited to conduct remote code execution when mod_cgi module is enabled on the Apache HTTP server, this allows an attacker to leverage the path traversal vulnerability and call any binary on the system using HTTP POST requests.

# By default the mod_cgi
module is disabled on Apache HTTP server by commenting the above line in the
configuration file. Hence, when mod_cgi is enabled and “Require all granted” config is applied to the filesystem directory directive then an attacker can remotely execute commands on the Apache server.


# This script Exploits an unauthenticated remote code execution vulnerability which exists in Apache version 2.4.49 (CVE-2021-41773). If files outside of the document root are not protected by ‘require all denied’ and CGI has been explicitly enabled, it can be used to execute arbitrary commands.

# A dockerized application that is vulnerable to the Apache vulnerability (CVE-2021-41773) 

#How To Reproduce

`docker run -dit --name Apache -p 83:80 b0x`

#Log into container and excute the following 

` cp /bin/sh /usr/bin/`

# Example 

    ruby CVE-2021-41773.rb
                                                        
    #       c  c  wWw    wWw  -2021-41773
    #       (OO)  (O)    (O)    wWw              
    #     ,'.--.) (     / )    (O)_             
    #    / //_|_     / /    .' __)            
    #    | ___    /  /     (  _)              
    #    '.    )    `--' /    `.__)             
    #      `-.'     `-..-'                                                                            
    #            Author:Habib  
    #   ruby CVE-2021-41773.rb target_url dir command 
    #   ruby CVE-2021-41773.rb http://localhost bin/sh whoami 
    
    ruby CVE-2121-41773.rb http://localhost:83
    Target: http://localhost:83 is vulnerable
    Command:
    ruby CVE-2121-41773.rb http://localhost:83 bin/sh id Target: http://localhost:83 is vulnerable
    Command: uid=1(daemon) gid=1(daemon) groups=1(daemon)




