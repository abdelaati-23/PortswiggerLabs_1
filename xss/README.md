<!-- omit in toc -->
# XSS

<!-- omit in toc -->
## Table of Contents

- [Reflected XSS into HTML context with nothing encoded](#reflected-xss-into-html-context-with-nothing-encoded)
- [Stored XSS into HTML context with nothing encoded](#stored-xss-into-html-context-with-nothing-encoded)
- [DOM XSS in document.write sink using source location.search](#dom-xss-in-documentwrite-sink-using-source-locationsearch)
- [DOM XSS in document.write sink using source location.search inside a select element](#dom-xss-in-documentwrite-sink-using-source-locationsearch-inside-a-select-element)
- [DOM XSS in innerHTML sink using source location.search](#dom-xss-in-innerhtml-sink-using-source-locationsearch)
- [DOM XSS in jQuery anchor href attribute sink using location.search source](#dom-xss-in-jquery-anchor-href-attribute-sink-using-locationsearch-source)
- [DOM XSS in jQuery selector sink using a hashchange event](#dom-xss-in-jquery-selector-sink-using-a-hashchange-event)
- [DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded](#dom-xss-in-angularjs-expression-with-angle-brackets-and-double-quotes-html-encoded)

## Reflected XSS into HTML context with nothing encoded
Reference: https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

<!-- omit in toc -->
### Solution
1. Copy and paste the following into the search box: ``<script>alert(1)</script>``
2. Click "Search".

## Stored XSS into HTML context with nothing encoded
Reference: https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded

<!-- omit in toc -->
### Quick Solution
Although this is an **extremely** easy lab to complete I also tried a tool called ``dalfox`` that is able to perform Store XSS assessments. The command I used is the following (on version 2.5.5):
```
dalfox sxss -X POST "<target_url>/post/comment" -d "csrf=<crsf_token>&postId=2&comment=thelicato&name=thelicato&email=the@lica.to&website=" -p "comment" --cookie="session=<session_cookie>" --trigger "<target_url>/post?postId=2" --request-method GET -w 1
```
The requests can also be proxied to Burp by adding ``--proxy <proxy_url>``

<!-- omit in toc -->
### Solution
1. Enter the following into the comment box: ``<script>alert(1)</script>``
2. Enter a name, email and website.
3. Click "Post comment".
4. Go back to the blog.

## DOM XSS in document.write sink using source location.search
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

<!-- omit in toc -->
### Solution
1. Enter a random alphanumeric string into the search box.
2. Right-click and inspect the element, and observe that your random string has been placed inside an ``img src`` attribute.
3. Break out of the img attribute by searching for: ``"><svg onload=alert(1)>``

## DOM XSS in document.write sink using source location.search inside a select element
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element

<!-- omit in toc -->
### Quick Solution
The lab description says that there is a vulnerability in the stock checker functionality. The ``document.write`` function is called with data from ``location.search`` that extracts a ``storeId`` parameter. Adding it to the URL can trigger the vulnerability:
```
product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
```

<!-- omit in toc -->
### Solution
1. On the product pages, notice that the dangerous JavaScript extracts a ``storeId`` parameter from the ``location.search`` source. It then uses ``document.write`` to create a new option in the select element for the stock checker functionality.
2. Add a ``storeId`` query parameter to the URL and enter a random alphanumeric string as its value. Request this modified URL.
3. In your browser, notice that your random string is now listed as one of the options in the drop-down list.
4. Right-click and inspect the drop-down list to confirm that the value of your ``storeId`` parameter has been placed inside a select element.
5. Change the URL to include a suitable XSS payload inside the ``storeId`` parameter as follows:
```
product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
```

## DOM XSS in innerHTML sink using source location.search
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink

<!-- omit in toc -->
### Solution
1. Enter the following into the into the search box: ``<img src=1 onerror=alert(1)>``
2. Click "Search".
The value of the ``src`` attribute is invalid and throws an error. This triggers the ``onerror`` event handler, which then calls the ``alert()`` function. As a result, the payload is executed whenever the user's browser attempts to load the page containing your malicious post.

## DOM XSS in jQuery anchor href attribute sink using location.search source
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink

<!-- omit in toc -->
### Solution
1. On the Submit feedback page, change the query parameter `returnPath` to / followed by a random alphanumeric string.
2. Right-click and inspect the element, and observe that your random string has been placed inside an a ``href`` attribute.
3. Change ``returnPath`` to ``javascript:alert(document.cookie)``, then hit enter and click "back".

## DOM XSS in jQuery selector sink using a hashchange event
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event

<!-- omit in toc -->
### Solution
1. Notice the vulnerable code on the home page using Burp or your browser's DevTools.
2. From the lab banner, open the exploit server.
3. In the **Body** section, add the following malicious `iframe`:
```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```
4. Store the exploit, then click **View exploit** to confirm that the ``print()`` function is called.
Go back to the exploit server and click **Deliver to victim** to solve the lab.

## DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
Reference: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression

<!-- omit in toc -->
## Quick Solution
Detect that *AngularJS* is used on the website. Then you can simply test different XSS payloads for AngularJS. This is an interesting source for them: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md

A payload that works is the following:
```
{{constructor.constructor('alert(1)')()}}
```

<!-- omit in toc -->
### Solution
1. Enter a random alphanumeric string into the search box.
2. View the page source and observe that your random string is enclosed in an ``ng-app`` directive.
3. Enter the following AngularJS expression in the search box:
```
{{$on.constructor('alert(1)')()}}
```
4. Click search