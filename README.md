With review of the [typegram](https://github.com/recoilme/tgram/) project code, it was discovered that multiple endpoints are vulnerable to CSRF. [More info about CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))

By default, "_Access-Control-Allow-Origin_" header set in _*_, what means Ajax can be used to perform malicious requests, like in [this](../delete.html) shitty snippet.
If own instance of typegram has inproper configs vulnerability can be executed.
[Typegram source](https://github.com/recoilme/tgram/blob/master/main.go#L186)

### Security suggestions:
- Replace vulnerable configuration with a more secure one.
- Vulnerable GET requests must be replaced with POST / PUT
- Need to add protection against CSRF attacks [(More Info)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)
- ???

### Vulnerable methods:
#### Votes:
```
https://ru.tgr.am/vote/up/@{username}/1
https://ru.tgr.am/vote/down/@{username}/1
```
#### Follows:
```
 GET https://ru.tgr.am/unfollow/{username}/@{username}
 GET https://ru.tgr.am/follow/{username}/@{username}
```
#### Favs:
```
 GET https://ru.tgr.am/unfav/{post}/@{username}/{post}
 GET https://ru.tgr.am/fav/{post}/@{username}/{post}
```
#### Comments:
[Example](../comment.html)
[POC](https://dokzlo13.github.io/typegram_attacks/comment.html) (require enabled js)
```
 POST https://ru.tgr.am/comments/@{username}/{post}
 body=
```
```
 GET https://ru.tgr.am/commentdel/@{username}/{username}/{post}/{comment}
```
#### New post
[Example](../post.html)
[POC](https://dokzlo13.github.io/typegram_attacks/post.html) (require enabled js)
```
 POST https://ru.tgr.am/editor/0
 title=&ogimage=&body=&tag=
```
#### Post Deletion
```
 GET https://ru.tgr.am/delete/a/{post}
```
#### Settings
This request requires "password" filled with correct password, but also has no CSRF protection
```
 POST https://ru.tgr.am/settings
 image=&bio=&email=&newpassword=&password=
```
#### Telegram export
```
 POST https://ru.tgr.am/export/type2tele
 channel=
 ```