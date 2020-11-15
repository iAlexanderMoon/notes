

https://nexusger.de/posts/2019-07-29-cache-spa-in-iis/
ache Single Page Applications (SPA) in Internet Information Service (IIS)


Cache looks like it's downloaded the new file... but it doesn't seem to be using it!
F5 on the home screen gets rid of the index of error when linking... now maybe that;s an auth thing and we need to try it on first login tomorrow.


<script type="text/javascript" src="runtime.3e9ad080d40319ffaa76.js"></script><script type="text/javascript" src="es2015-polyfills.f5ac0d3ffd2251d6e495.js" nomodule></script><script type="text/javascript" src="polyfills.376ed6d049b86e95fad0.js"></script><script type="text/javascript" src="scripts.bc6022a69becc267e66f.js"></script><script type="text/javascript" 

src="main.ef28c334842a7fcb8574.js"></script></body>
"main.ef28c334842a7fcb8574.js

	Why is this happening on a new private window?
	Request URL:https://app.olymel.com/ids/connect/authorize?client_id=obs_spa&redirect_uri=https%3A%2F%2Fapp.olymel.com%2Fobs%2Fsilent-renew.html&response_type=code&scope=openid%20profile%20api.access&nonce=N0.128381946536492931585680061712&state=15856800617110.116868839144335430.3641862830880346&code_challenge=WtBz8nTDUYjnimyw_EtQ4mH6A13fMHPmBGjHelil0-0&code_challenge_method=S256&prompt=none
	
	
	or: this.authService.obsRights is undefined
	
	main.ac2ca197954badd3be7a.js
	
	
	
  <location path="index.html">
    <system.webServer>
      <httpProtocol>
        <customHeaders>
          <add name="Cache-Control" value="no-cache" />
        </customHeaders>
      </httpProtocol>
    </system.webServer>
  </location>