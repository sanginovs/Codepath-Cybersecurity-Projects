# Project 8 - Pentesting Live Targets

Time spent: **8** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: SQL Injection

* When adding a new territory, the form has two inputs: Name and Position. Position is vulnerable to SQL Injection. I was able to close the query with single quote and SQL server threw an error: Incorrect Integer value for column position at row 1.
* When adding a new country, the form has three inputs: Name, Code, Country_Id. Country_Id is vulnerable to SQL Injection. I used a single quote to figure it out. It threw an error : "Incorrect Integer value for column country_id at row 1",  "Data truncated for column country_id at row 1"

<img src = "sqlinjection.gif" width = "800">

Vulnerability #2: Session Hijacking

a. I logged in Blue using Safari browser with my "pperson" user credentials. Then, I opened Firefox and navigated to Blue's index page. However, it redirected me to a login page. I copied the session id from Safari Browser and changed Firefox session id to it. Then, I was able to access the home page without logging in while using different browser and session id of the logged in user.

<img src="sessionhijacking.gif" width = "800">

## Green

Vulnerability #1: Username Enumeration

Green is vulnerable to username enumeration. When a registered user types in their username but the wrong password, it shows an error message in **bold** saying **Log in was unsuccessful**. However, when an unregistered user puts in their wrong username and password, it shows the same error message not in bold: "Log in was unsuccessful." As a result, users who exist in the database are shown **"Log in was unsuccessful"** in bold.

<img src="usernameEnumeration.gif" width = "800">

Vulnerability #2: Cross-Site Scripting (XSS)

Green is also vulnerable to stored Cross-site scripting attack. There is a "Contact Us" form in the public section of the website. The feedback field in the Contact Us form is vulnerable to stored XSS. I was able to pass in JS code `<script>alert("Hello Sher")</script>`. Once I logged into the website and navigated to Feedback section, the alert box popped up with a message "Hello Sher".

<img src="xss.gif" width="800" >


## Red

Vulnerability #1: Cross-Site Request Forgery

Red is vulnerable to Cross-Site Request Forgery (CSRF) because it does not require CSRF token when submitting a form. I inspected the form which lets you edit States in the website. Then, I created the exact clone of the form in my Google Cloud Platform web server that edits Alabama to Alabamaaaaa. Once the logged in user clicks on my link, the form is submitted automatically and the user does not notice anything:

<img src= "csrf.gif" width="800" >

Vulnerability #2: IDOR

Red is also vulnerable to IDOR vulnerability. First, I logged in and viewed the list of salespeople and noticed it uses direct id same as the one in database. So, it was easy to guess the next user by just changing id parameter to current_user_id +1 `salespeople/show.php?id=10`. There were two users in the Salespeople section who were not available to public:  
* Tery Testerson ('Not Public Until Sept. 1)
* Lazy Lazyman ('Fired For Stealing')

In the public view of the website, unregistered users can simply view all the Salespeople in their region. However, the above two individuals (Tery and Lazy) are not available to public view. However, one can simply change the user id to 10, 11 and view their profile.

<img src="idor.gif" width="800">


## Notes

Describe any challenges encountered while doing the work:

The challenging part of this project was finding vulnerabilities at the beginning since I was not sure which color had which vulnerabilities. Once I found couple of them, it was easier to find the rest.