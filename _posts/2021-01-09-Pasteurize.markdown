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
which because the code is extended thanks to the creator allows us to use a nested array in the payload of `content` can become `content[]`





