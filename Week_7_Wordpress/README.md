# Project 7 - WordPress Pentesting

Time spent: **20** hours spent in total including setting up the pentesting workspace

> Objective: Find, analyze, recreate, and document **five vulnerabilities** affecting an old version of WordPress

## Pentesting Report

1. (Required) Vulnerability: Unauthenticated Stored Cross-Site Scripting (XSS) ID: 7945
  - [ ] Summary: 4.2 version of Wordpress is vulnerable to a stored XSS attack. An unauthenticated attacker can inject JavaScript code in the Wordpress comments and JS code is executed when comment is viewed by anyone.
    - Vulnerability types: Stored XSS
    - Tested in version: 4.2
    - Fixed in version: 4.2.1
  - [ ] GIF Walkthrough: <img src="xss-wordpress.gif" width="800">
  - [ ] Steps to recreate:
	* Navigate to comment section of Wordpress website.
	* Fill out the Name and Email fields.
	* In the comment field, you should inject JS code similar to this:
``` <a title='x onmouseover=alert(unescape(/hello%20sher/.source)) style=position:absolute;left:0;top:0;width:5000px;height:5000px  AAAAAAAAAAAA...[64 kb]..AAA'></a>```
	* Make sure your comment length is greater than 64KB so we get SQL truncate our comment and store it in the database.The comment should be greater than 65635(64KB) characters. 
	* View the comments as an admin or a different user and you'll get an alert box. 

  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/changeset?sfp_email=&sfph_mail=&reponame=&new=32311%40branches%2F4.2&old=32300%40branches%2F4.2)

2. (Required) Vulnerability Name or ID: Server-side Request Forgery (id: 8376)
  - [ ] Summary: 
         Press This feature in Wordpress allows internal GET Server Side Request Forgery via CSRF because it does not require csrf token for each request since the url does not validate if the user intends to send a scrape request or not. The filter does not validate for **0.0.0.0:PORT** and allows an attacker to make the victim send GET request to server's private 127.0.0.1:PORT, localhost:PORT and anything that can be accessed via 0.0.0.0 

    - Vulnerability types: CSRF, SSRF
    - Tested in version: 4.2
    - Fixed in version: 4.2.7
  - [ ] GIF Walkthrough: 
        I logged the user and created the same exact Press This form in my Google Cloud Plaform server. If I get an admin click on my link, he'll make an unwanted Press This request to a url(public or private) specified by an attacker. However, in this gif demonstration, admin user will not be able to make a request if clicked on my Google Cloud server external IP because wordpress is installed locally using Vagrant in my machine so Google Server will not be able to send GET request action to my local wordpress website. 
        <img src="csrf1.gif" width="800">
  - [ ] Steps to recreate:
     *  User should be logged in Wordpress
     *  Attacker gets user click on a malicious link such as ```<img src="/=https://userWordpress.com/wp-admin/press-this.php?u=http://0.0.0.0:8080&url-scan-submit=Scan"/>```
     *  User sends an unwanted request to its server requesting an internal server address to be hit  
     * As a result, server sends GET request to 0.0.0.0:8080 and servers pricate 127.0.0.1 responds
     * Attacker can also use a public address
  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/changeset/36435)

3. (Required) Vulnerability Name or ID: XSS Vulnerability
  - [ ] Summary: When adding a new post, Wordpress allows you to input text in Text input and by clicking on the Visual Tab, one can view it. However, Visual tab is vulnerable to XSS, injected through the Text input.
    - Vulnerability types: XSS
    - Tested in version: 4.2
    - Fixed in version: 4.3.1
  - [ ] GIF Walkthrough: 
       <img src="xss1.gif" width="800">
 
  - [ ] Steps to recreate: 
     * Navigate to yourWorpressUrl.com/wp-admin/post-new.php
     * Click on Text tab and input: ```<HTML xmlns: > <audio><audio src=a onerror=alert(0X1)>```
     * Click on Visual Tab and XSS gets triggered.

  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/browser/branches/4.3/src/wp-admin/post-new.php?rev=34199)

4. (Optional) Vulnerability Name or ID: Stored XSS
  - [ ] Summary: Admin's User tables page was vulnerable to XSS attack. Non-admin user could insert malicious XSS as their email and it gets triggered when admin views list of users with their emails.The proof of concept was missing and I had try different XSS combinations but I did not succeed.
    - Vulnerability types: XSS
    - Tested in version: 4.2
    - Fixed in version: 4.2.1
  - [ ] GIF Walkthrough: 
      <img src="storedXss.gif" width="800">
  - [ ] Steps to recreate: 
     * Log in as a normal user and go to your profile
     * Change your email to malicious XSS code. Your email is directly put in ```href="mailto:useremail"``` without sanitizing.
     * Once the admin views the list of users, XSS gets triggered.
  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php)

## Assets

List any additional assets, such as scripts or files

## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)
- [HackerOne](https://www.hackerone.com)
- [WPScan Vulnerability Database](https://wpvulndb.com)

GIFs created with [LiceCap](http://www.cockos.com/licecap/).

## Notes

Describe any challenges encountered while doing the work:

I really enjoyed working on this assignment. One of the challenging parts of this assignment was to able to reproduce some of the security vulnerabilities in the Wordpress website.
This is because the security vulnerabilities were briefly documented however the proof of concept was missing. 

## License

    Copyright [2019] [Sher Sanginov]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
