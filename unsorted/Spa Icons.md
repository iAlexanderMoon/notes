icons
'android-chrome-'
'android-chrome-'
'android-chrome-maskable-'
'android-chrome-maskable-'
'apple-touch-icon-'
'apple-touch-icon-'
'apple-touch-icon-'
'apple-touch-icon-'
'apple-touch-icon-'
'favicon-'
'favicon-'
'msapplication-icon-'
'mstile-']\n"

Vue Cli Generates a new project file with icon files in:

project/public/img/icons/
	android-chrome-192x192.png
	android-chrome-maskable-192x192.png
	apple-touch-icon-120x120.png
	apple-touch-icon-180x180.png
	apple-touch-icon-76x76.png
	favicon-16x16.png
	msapplication-icon-144x144.png
	safari-pinned-tab.svg
	android-chrome-512x512.png
	android-chrome-maskable-512x512.png
	apple-touch-icon-152x152.png
	apple-touch-icon-60x60.png
	apple-touch-icon.png
	favicon-32x32.png
	mstile-150x150.png

Of course these are all images of the Vue icon.

The following script set of commands can be issued to imagemagik to generate these files given an image to start with.
* install ImageMagik as necessary for your platform https://imagemagick.org/

```
apt install imagemagick
```


For best results:

    A square PNG image, 228 x 228 or greater.
    An Image with an Alpha layer (transparent background)

```sh
convert olymellogo.png -resize '192x192' android-chrome-192x192.png
convert logo.png -resize '192x192' android-chrome-192x192.png


```

https://www.emergeinteractive.com/insights/detail/the-essentials-of-favicons/



https://web.dev/maskable-icon/
standardized "minimum safe zone"
logo, should be within a circular area in the center of the icon with a radius equal to 40% of the icon width. 
The outer 10% edge may be cropped.


40% of radius... well that 80% of the width and we want it centered.
width x 0.8 = 
192 x 0.8 = 153.6 = 154
512 x 0.8 = 409.6 = 410

Imagemagick commands to generate the named icon images:

For best results... logo.png should be square, a minimum of 228x228 with a transparent background.

convert logo.png -resize '192x192' android-chrome-192x192.png
convert logo.png -resize '512x512' android-chrome-512x512.png
convert logo.png -resize '154x154' -gravity center -background transparent -extent 192x192 android-chrome-maskable-192x192.png
convert logo.png -resize '410x410' -gravity center -background transparent -extent 512x512 android-chrome-maskable-512x512.png
convert logo.png -resize '180x180' -background white apple-touch-icon.png
convert logo.png -resize '60x60' -background white apple-touch-icon-60x60.png
convert logo.png -resize '76x76' -background white apple-touch-icon-76x76.png
convert logo.png -resize '120x120' -background white apple-touch-icon-120x120.png
convert logo.png -resize '180x180' -background white apple-touch-icon-180x180.png
convert logo.png -resize '152x152' -background white apple-touch-icon-152x152.png
convert logo.png -resize '16x16' favicon-16x16.png
convert logo.png -resize '32x32' favicon-32x32.png
convert logo.png -resize '144x144' msapplication-icon-144x144.png
convert logo.png -resize '150x150' -background transparent -compose Copy -gravity center -extent 270x270 mstile-150x150.png 
convert logo.png -resize '512x512' safari-pinned-tab.svg

When I looked at the mstile-150x150.png the inner logo wasn't centered... but I never did figure that out and it wasn't that important to me.

