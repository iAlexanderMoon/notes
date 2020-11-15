
How long should building ng serve take?

Docker startup or service restart:
Startup is quick.
Compilation:	30 Seconds

Changing a file: Compilation 7.5 Seconds

Tried with the watch flag set to true. Compilation 6.5 seconds. Probably a fluke because it's on by default!

The slow part is loading the application. it issues a refresh and the entire app loads again! 
Most parts are cached and it returns the new ones. BUT the screen goes away and we watch it load all over again.


Maybe hot module reloading is the way to go?


Easy enough to try add the "hmr" switch to dockerdev configuration.

Output:

NOTICE: Hot Module Replacement (HMR) is enabled for the dev server.
The project will still live reload when HMR is enabled, but to take advantage of HMR additional application code is required'
(not included in an Angular CLI project by default).'
See https://webpack.js.org/guides/hot-module-replacement for information on working with HMR for Webpack.


https://codinglatte.com/posts/angular/enabling-hot-module-replacement-angular-6/
Hot Module Replacement (HMR) is a key webpack feature that is not enable by default in Angular. 

npm install --save-dev @angularclass/hmr

Created hmr.ts beside main.ts as per configuration article and enabled hmr for dockerdev.
https://codinglatte.com/posts/angular/enabling-hot-module-replacement-angular-6/


NOTICE: (HMR) does not allow for CSS hot reload when used together with '--extract-css'.

It does seem faster than a full reload. I wonder if we could make more chunks to make it faster.