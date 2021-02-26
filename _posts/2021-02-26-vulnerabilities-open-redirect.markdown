---
layout: post
title:  "Open Redirect Vulnerabilities"
date:   2021-02-26 20:17:24 -0500
categories: vulnerabilities web
---
Open redirects occur when a developre mistrusts attacker-controlled input to redirect to another site. This input usually takes the form of a URL parameter, HTML meta refresh tags, or the Document Object Model (DOM) location property.

Redirection is a normal behavior of websites. The application directs the browser to send a request to a different URL. Severs will generally return a stattus code of 302, meaning the site was found but has temporarily been moved. Its also common to see responses of 301, 303, 307, or 308.

Open redircect vulnerabilites occur when the destination the browser is being redirected to can be manipulated by the user.

# Parameter Based Attacks

As stated earlier, one type of open redirect vulnerability is when a redirect URL is passed as a URL parameter. An attacker is able to modify the value of this parameter and if the server doesn't validate the value then a user could be redirected to a malicious site.

As an example, suppose google had a URL such as:

https://www.google.com/?redirect_to=https://www.gmail.com

As you can see, the URL parameter <em>redirect_to</em> is being used by the server to determine where to redirect the browser. In this case, the browser will be redirected to gmail.

However, suppose an attacker wanted to redirect the browser to a malicious website where they could gather aditional information about the user. The attacker would be able to accomplish this by modifying the above you to be:

https://www.google.com/?redirect_to=https://www.malicious-site.com

This request would cause the server to respond with a response directing the browser to navigate to this malicious site and may do so without the users knowledge.

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

# Javascript Based Attack

An attacker may also be able to control redirect functionality by changing the window's <em>location</em> property through the DOM. The <em>Document Object Model (DOM)</em> is an API that allows the structure, style, or content of a webpage to modified. The <em>location</em> property defines where a request should be sent and a browser will immediately interpret this javascript and redirect to the value of the property.

Any of the following javascript statements can be run to change the location property on the window:

```javascript
window.location = "https://www.google.com/"
window.location.href = "https://www.google.com/"
window.location.replace("https://www.google.com/")
```

This type of vulnerability can be exploited when an attacker is able to execute Javascript. The can be a Cross-site Scripting attack or simply a website that allows a user to define a URL to redirect to.