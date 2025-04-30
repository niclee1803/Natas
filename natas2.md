# Natas2
Upon inspecting HTML source, I find that there is an image element as shown below
```html
<img src="files/pixel.png">
```
I access the files directory of the webpage by going to
```text
http://natas2.natas.labs.overthewire.org/files
```

I found a ```users.txt``` file in that directory, which contains the password to natas3


## Key learnings: 
* Web servers sometimes expose directory listings, especially when Indexes are enabled in the server config (e.g., Apache).
* Hidden or unlinked files may be discoverable through analysis of HTML elements (e.g., ```<img>```, ```<script>```, ```<a>```).
* Security through obscurity is not a reliable defense—directory contents should be properly protected or excluded.

## Takeaway for Developers
* Disable directory listing unless absolutely required
* Use ```.htaccess``` or server configuration to restrict file access
* Never store sensitive information in publicly accessible directories
