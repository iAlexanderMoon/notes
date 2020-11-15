For My Template:

User Interface Design Guidelines:

Browse Pages:

	A browse page is a search or list result page. It includes search functionality for requesting data from a backend and filter functionality for modifying what or how the data is displayed. 
	
	When a browse page is visited it should provide sane defaults to the search and filter. 
	
	Search defaults should be overrideable through the URI.

	Search requests should modify the URI to show the values being requested on that page. 
	
	```
	  Session storage is sometimes used to store these details of which only live for the life of the browser tab or window. 
	  If a new tab or window is session data is lossed with the page.
	```

	For log term storage of user defaults, this could be persisted in localstorage, thus saving the data across browser windows/tabs and browser application opening and closing. However, this does not store the data across browser
	
	Implementation Details:
	
	When a search or data fetch is made, the url should be changed to reflect the data retrieved.
	
	
	Typically it provides a search mechanism for requesting data. When data is requested the URL should reflect what data is being searched.
	Query parameters should set search parameters as necessary.


Browse Pages:
	
	When visiting a search page for the first time it is good to set it to sane defaults. 
	
	What are sane defaults?
	Should the 
	
	
	Session Storage
	Local Storage
	
	
https://flaviocopes.com/web-storage-api/

https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage