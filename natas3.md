# Natas3
Upon inspecting HTML source, I found this comment:  
```html
<!-- No more information leaks!! Not even Google will find it this time... -->
```

This could have something to do with the ```robots.txt``` file of a website.

From Google:
A ```robots.txt``` file is a text file that instructs web crawlers (like those used by search engines) which parts of a website they should or should not visit. It's a way for website owners to manage how search engines explore and index their content. While it can help prevent overloading servers with requests, it's not a foolproof way to keep pages from appearing in search results. 

We can access the file by navigating to:
```
http://natas3.natas.labs.overthewire.org/robots.txt
```

The contents of the file are:
```makefile
User-agent: *
Disallow: /s3cr3t/
```



