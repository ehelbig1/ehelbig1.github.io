---
layout: post
title:  "Open Redirect Vulnerabilities"
date:   2021-02-26 20:17:24 -0500
category: security
tag: vulnerabilities
---

&nbsp;
&nbsp;

***

# Summary 

Open redirects occur when a developer mistrusts attacker-controlled input to redirect to another site. This input usually takes the form of a URL parameter, HTML meta refresh tags, or the Document Object Model (DOM) location property.

Redirection is a normal behavior of websites. The application directs the browser to send a request to a different URL. Severs will generally return a status code of 302, meaning the site was found but has temporarily been moved. Its also common to see responses of 301, 303, 307, or 308.

Open redirect vulnerabilities occur when the destination the browser is being redirected to can be manipulated by the user.

&nbsp;
&nbsp;

***

# How to Find Open Redirect Vulnerabilities

When looking for Open Redirect vulnerabilities in an application you will keep an eye out for any requests that include a parameter that specifies a redirect URL. Sometimes this will be obvious by naming conventions such as a parameter name <em>redirect_url</em> or something similar. Other times, these parameters may be obfuscated and could be single characters. Take note of the responses that you receive from the server for any indication that you are being redirected and explore if that URL is vulnerable.

As we'll see later, there are several different ways that redirects are handled and as such, there are several areas to explore when searching for Open Redirect vulnerabilities.

&nbsp;
&nbsp;

***

# Parameter Based Attacks

As stated earlier, one type of open redirect vulnerability is when a redirect URL is passed as a URL parameter. An attacker is able to modify the value of this parameter and if the server doesn't validate the value then a user could be redirected to a malicious site.

As an example, suppose google had a URL such as:

https://www.google.com/?redirect_to=https://www.gmail.com

As you can see, the URL parameter <em>redirect_to</em> is being used by the server to determine where to redirect the browser. In this case, the browser will be redirected to gmail.

However, suppose an attacker wanted to redirect the browser to a malicious website where they could gather additional information about the user. The attacker would be able to accomplish this by modifying the above you to be:

https://www.google.com/?redirect_to=https://www.malicious-site.com

This request would cause the server to respond with a response directing the browser to navigate to this malicious site and may do so without the users knowledge.

&nbsp;
&nbsp;

***

# HTML Meta Tag Based Attack

Another type of open redirect vulnerability has to do with meta tags within the HTML document itself. These meta tags can direct a browser to refresh a web page and make a request to a different URL.

Here's an example of an HTML document with such a meta tag:

```html
<html>
    <head>
        <meta http-equiv="refresh" content="0; url=https://www.google.com/">
    </head>
    <body>
    </body>
</html>
```

The <em>content</em> attribute in the above meta tag defines how long the browser should wait before making a request to the defined URL (0 meaning 0 seconds in this case).

This vulnerability can be exploited when an attacker as control over the content attribute in the meta tag or the ability to inject their own meta tag via another vulnerability.

&nbsp;
&nbsp;

***

# Javascript Based Attack

An attacker may also be able to control redirect functionality by changing the window's <em>location</em> property through the DOM. The <em>Document Object Model (DOM)</em> is an API that allows the structure, style, or content of a webpage to modified. The <em>location</em> property defines where a request should be sent and a browser will immediately interpret this javascript and redirect to the value of the property.

Any of the following javascript statements can be run to change the location property on the window:

```javascript
window.location = "https://www.google.com/"
window.location.href = "https://www.google.com/"
window.location.replace("https://www.google.com/")
```

This type of vulnerability can be exploited when an attacker is able to execute Javascript. The can be a Cross-site Scripting attack or simply a website that allows a user to define a URL to redirect to.

&nbsp;
&nbsp;

***

# Remediation

To prevent Open Redirect vulnerabilities, it's best to not allow a user to define a redirect URL. Two ways this can be done are: 
- Have the application can provide direct links for the user to click on. 
- Store a list of possible URL to redirect to and pass an index into that list with the request.

Both these solutions prevent a user from defining an arbitrary redirect URL but sometimes it may not be feasible to completely remove this functionality from an application however.

In these cases, one of the following methods should be used:
- Use relative URLs and validate that the URL exists.
- Use URLs relative the web root and validate that the request value starts with a **/**.
- Use absolute URLs and verify that the URL begins with a valid domain.

&nbsp;
&nbsp;

***

{% include relevant_articles.markdown title=page.title category=page.category tag=page.tag %}

&nbsp;
&nbsp;

***

# References

- [PortSwigger](https://portswigger.net/kb/issues/00500100_open-redirection-reflected)
- [Real-World Bug Hunting: A Field Guide to Web Hacking](https://www.amazon.com/Real-World-Bug-Hunting-Field-Hacking/dp/1593278616)