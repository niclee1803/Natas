# Natas1
On loading the page, I noticed that right-click is disabled using JavaScript, likely to prevent users from easily viewing the page source.

There are many ways to bypass this to access the HTML source of the website
## Method 1: curl
We can use curl to access the raw HTML data of the website directly in the command line.
```bash
curl http://natas1.natas.labs.overthewire.org/ --user "natas1:0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq"
```
## Method 2: Use "View Page Source" via browser dev tools
Press ```CTRL+U``` or go to:  
Browser menu → Developer Tools → View Source   

## Method 3: Disable Javascript
Can be done through browser settings or using an extension (e.g., uBlock Origin) to block scripts.

## Key learnings:
* Client-side controls (like disabling right-click) are not security mechanisms.
* HTML comments are sometimes used to hide sensitive information, which is insecure if served to the client.
* Tools like curl, wget, and browser dev tools are essential for inspecting how websites work under the hood.

## Takeaways for developers
* Don't rely on JavaScript to hide sensitive information. Anything sent to the client can be seen by the client.
* Don't include passwords, API keys, or internal notes in HTML comments or front-end code.





