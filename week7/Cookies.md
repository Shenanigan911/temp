# Cookies and Sessions

## Cookies
Store data
- can be temporary or persistent
- stored locally in the browser
  - clear cookies - delete info that site has created
  - 
  
where do they come from?
- webserver is who tells the browser to store cookies
- cookie information is passed in HEADERS
- 

response.writeHead(200, {
"Content-Type": "image/jpg",
"Authorization" : "Bearer asdjfhaskfh"
})

Sessions
- objects that represent information about a user
  - How the server remembers who you are
  - Live on the servers
  - Cookies are sent up from the browser to the server on every request
  - cookies are stored in each browser in its own cookie store.  Cookie in Chrome will be different than cookie for same site in Firefox.
  - 
  
