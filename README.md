## Zopim live chat impersonation issue (proof of concept)

### Issue

Zopim live chat API permits for the website to set various attributes to the chat widget and chat session,
including user email and user name.

What this API lacks is any kind of verification of those details, so, unless implemented
as a policy by anyone administering the widget, this can be abused to perform social engineering attacks on
chat operators and/or someone oblivious to this issue ("hey, but those details where set by OUR website, right...?").

This makes embedding Zopim in any private customer area impossible, as trust can not be properly established via chat,
making the chat inherently less helpful than it could be.

### POC

This repository contains a demo code you can run and see what happens when the parameters are set
second time.

You can evaluate the source of `index.html` to see how this works, and try it yourself. The code is tested and it works.

### Impact

At the time of writing (Aug 2016), setting the parameters second time after the chat is already initialized or
preventing initialization, or replacing script, or doing any other kind of frontend mockery around the embed code
results in instant change of the user details in Zopim platform. If the code to set params is executed right after
widget loads, nobody would probably suspect a thing.

### Possible fix

Zopim could issue a secure per-account token. Details being set to the chat widget could then be signed in backend by the
end user using this token, could be as simple as

```
    token = sha1( [user, email@mail.com].concat(token) )
```

which could then be verified on Zopim side. If the token does not match or is not present,
showing a warning to the chat operator could be appropriate thing to do. :)

### Possible not-fixes

Anything not involving a secret and done in frontend would probably be just waste of time.

#### Credits

Zopim widget code - (c) Zopim, all rights and wrongs reserved :)
POC code - public domain
