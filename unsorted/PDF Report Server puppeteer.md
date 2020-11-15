PDF Report Server

Converting HTML to PDF on request

jsreport server Docker.
Middleware to intercept initial request and initiate the request on the jsreport server to render pdf and return to the Middleware to return result to .

Could be an infinite loop if the request switch is set.
Need to send token information

Middleware needs to look at the client sending the request for a token. 
If it's a client token from jsreport then the middleware should ignore the request passing it up the request pipeline to MVC to serve the request.
How do we turn this on ONLY for certain requests. Do we register them in the USE Middleware or provide some kind of reflection attribute or something.



https://www.npmjs.com/package/chrome-headless-render-pdf
https://medium.com/compass-true-north/go-service-to-convert-web-pages-to-pdf-using-headless-chrome-5fd9ffbae1af


https://github.com/GoogleChrome/puppeteer

const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://news.ycombinator.com', {waitUntil: 'networkidle2'});
  await page.pdf({path: 'hn.pdf', format: 'A4'});

  await browser.close();
})();


Middleware to pupeter

puppeteer to generate pdf
	javascript official library for controlling headless chrome
	
	puppeteer sets up a running chrome and an webapi endpoint.
	on it's own microservice
	
	dotnet middleware takes sends the request throught the api 
	


Chrome headless:

chrome \
  --headless \                   # Runs Chrome in headless mode.
  --disable-gpu \                # Temporarily needed if running on Windows.
  --remote-debugging-port=9222 \
  https://www.chromestatus.com   # URL to open. Defaults to about:blank.

  
  chrome --headless --disable-gpu --print-to-pdf https://www.chromestatus.com/
  debugging starts the devtools-protocol
  https://chromedevtools.github.io/devtools-protocol/

  Devtools starts a web socket:
  DevTools listening on ws://127.0.0.1:36775/devtools/browser/a292f96c-7332-4ce8-82a9-7411f3bd280a
  
  JSONRPC
  
GO Implementation and answers about doing stuff:

	https://medium.com/compass-true-north/go-service-to-convert-web-pages-to-pdf-using-headless-chrome-5fd9ffbae1af  
  
  GO: Bindings fro CDP https://godoc.org/github.com/mafredri/cdp
  
  
One copy of chrome running constantly:
http://pm2.keymetrics.io/
https://peter.sh/experiments/chromium-command-line-switches/