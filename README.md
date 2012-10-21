UI Screen Shots
===============

This is a set of scripts to demonstrate how to take screen shots for your iOS app for the App Store automatically using UI Automation. It shows how to take screen shots, extract them from the automation results and change the language in the simulator with shell scripts. This saves quite a bit of time since we need to generate screens for the 3.5" display, the 4" display, and both iPhone and iPad if your app is universal--not to mention that you have to do this for *every* localization you support in the store.

You can see the script run against one of [my apps](http://readmoreapp.com) in [this video][readmorevid].

  [readmorevid]: http://nl1551.s3.amazonaws.com/cocoamanifest.net/2012/readmore-screenshots.mov

## Installation

First, you need to get Xcode from the App Store. It's free and comes with (almost) everything you need. Once you have Xcode installed, you need to install the command line tools. Choose "Preferences" from the "Xcode" menu. Choose the "Downloads" and choose the "Components" sub tab. You'll see "Command Line Tools" in the list. Click the install button next to it and wait until it finishes setting up.

Pull down this repository and change to the directory in the terminal.

## Usage

To run the demonstration, type `./run_screenshooter.sh ~/Desktop/screenshots` to tell it where to put the final set of screen shots. After a few minutes, you can open the destination directory and see all the languages, devices types and screen sizes as PNGs.

By default each screenshot is named like so:

    en-iphone5-portrait-screen1.png

The first part is the locale identifier, the second is the device (iphone, iphone5, ipad), the third is the device orientation, and the fourth is an identifier that you choose for each screen shot when you call `captureLocalizedScreenshot()`.

## How It Works

`run_screenshooter.sh` triggers a build of the application for the iOS simulator and puts the resulting bundle in `/tmp` with a custom name so it can find it. Then, the `instruments` command line tool is invoked which installs the app bundle and then executes `automation/run.js` which drives the simulator. `run.js` drives the app and calls `captureLocalizedScreenshot()` to shoot each image after navigating to the right screen.

`captureLocalizedScreenshot()` is a custom method that pulls the user's language choice out of the user defaults, checks for the device and whether it's a 4" display or not, deduces the orientation, and generates the screenshot file name along with the user supplied identifier. Once the name is calculated, it calls `captureScreenWithName()` on the `UIATarget` which saves the image along with the Instruments trace results in `/tmp`.

After each time the automation script ends, `run_screenshooter.sh` copies all the screenshots taken for that Instruments trace run and copies them to the destination directory. Then it continues on to execute the same automation script again with a new language or an a new device type. Check out the `main` function in `run_screenshooter.sh` for how this is all set up.

## For More Info

To learn more about UI Automation, check out my [collection of resources][automation] on [cocoamanifest.net](http://cocoamanifest.net).

  [automation]: http://cocoamanifest.net/features/#ui_automation

## Contributing

Feel free to fork the project and submit a pull request. If you have any good ideas to make this easier to set up for new users, that would be great!

## Contact

Questions? Ask!

Jonathan Penn

- http://cocoamanifest.net
- http://github.com/jonathanpenn
- http://twitter.com/jonathanpenn
- http://alpha.app.net/jonathanpenn
- jonathan@cocoamanifest.net

## License

UI Screen Shots is available under the MIT license. See the LICENSE file for more info.
