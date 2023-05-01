# Helpful Blog 📯

### 🚥 4-2-2023

##### React, HashRouter & Keyclock
🦨 If your application is adding &state= to the url after Keyclock is initalized, then this documetation might help you 🚑.

[Link to doc 👈](https://github.com/suvel/2023_blog/blob/main/react_hashrouter_keyclock.md#react-hashrouter--keyclock "Link to doc")

**TLDR**
*  By default, when the responseMode is set to 'fragment', Keycloak adds the state parameter to the URL fragment. However, when the responseMode is set to 'query', the state parameter is added to the query string instead.
```
keycloak
    .init({ 
	onLoad: 'login-required',
	//add the below line 👇
	responseMode: 'query' 
	})
```
* Alternate solution, use BrowerRouter * without responseMode* (default to 'fragment')

### 🚥 1-5-2023

##### Exploring HTML 
I embarked on my journey into a framework with little knowledge of the basics, but I've come to realize the importance of laying a strong foundation. Now, I'm taking it slow, steadily acquainting myself with the essential concepts I should have known from the start. In this blog, I aim to share the valuable lessons and insights I've gained through my experience of discovering what I didn't know before.


[Link to doc 👈](https://github.com/suvel/2023_blog/blob/main/exploring_HTML.md)
