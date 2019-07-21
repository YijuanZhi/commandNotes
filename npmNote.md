# Web Development Commands Notes --- Yijuan Zhi

## npm
- npm init ~~~~ initialized npm setup files, walk through the json creation process, create the package.json file
- npm install express --save ~~~~ install express and its dependent components, --save write this into our init package json file.
- npm install ejs --save ~~~~ for the ejs support
- npm uninstall [packagename] ~~~~ will uninstall the specified package
- npm list -g --depth=0 ~~~~ will list all the major packages that was installed globally, does not include their dependent packages
- common options: [-P|--save-prod|-D|--save-dev|-O|--save-optional] [-E|--save-exact] [-B|--save-bundle] [--no-save] [--dry-run]
  

## Running
- node [app name] ~~~~ will start the application inside node environment
- nodemon ~~~~ will start the application and restart it when modified


## Others
- curl [url] ~~~~ a tool used for transfer from or to a server