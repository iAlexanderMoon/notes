


fine SecurityHeadersAttribute.cs on ids project

var csp = "default-src 'self'; object-src 'none'; frame-ancestors 'none'; sandbox allow-forms allow-same-origin allow-scripts; base-uri 'self';";

change frame-ancestors 'none' 
to frame-ancestors 'self' https://app.olymel.com/obs

for more information see https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors

Identity Server

Quickstart/SecurityHeadersAttribute.cs



---------------------------------------
PWA detect refresh


add an event listeners inside your service worker that listens for the install event. Whenever it fires, send a message via postmessage to your main thread and show a update info to your users.
When users presses ok send another message to the service worker back that triggers a "self.skipWaiting()" and afterwards a location.reload() 

 src-pwa/register-service-worker.js… You got the “updated” method there.
 
 https://forum.quasar-framework.org/topic/2560/solved-pwa-force-refresh-when-new-version-released/15