# AutoVersionizer

AutoVersionizer is a simple tool that saves you time when updating the versions of your app(s). Every time you press `ctrl + s` in Visual Studio Code (VSCode), the tab is saved as normal and the version number in the `package.json` file is automatically updated.

## Installation and Usage

1. To implement the functionality in VSCode, you'll need to install a VSCode extension. Start by using the VSCode API to listen for the `ctrl + s` key press and running the version update command. For example, if you're using Electron to build cross-platform desktop apps, open your main file (e.g. `main.js`, `app.js`, etc.) and add the `package.json` file under the `require electron` statement:

```javascript
const { app, BrowserWindow } =  require('electron');
const  packageJson  =  require('./package.json');
let  mainWindow;
```

2. Use JavaScript to run Grunt, which will retrieve the version information from package.json and run each time you save your work. You can add the following JavaScript anywhere in your app as long as it's in the same file path as the package.json file:

```grunt.registerTask('version', 'Update version number', function () {
  var packageJson = require('./package.json');
  packageJson.version = '1.0.' + (new Date()).getTime();
  fs.writeFileSync('./package.json', JSON.stringify(packageJson, null, 2));
});

grunt.event.on('watch', function (action, filepath) {
  grunt.task.run('version');
});
```
3. In the code above, the activate function registers a command that listens for the onDidSaveTextDocument event. When this event is triggered, the code checks if the saved document is a package.json file, and if so, runs the npm version patch command using the child_process.execSync method.

4. Finally, add the version script to the scripts section of the package.json file:

```"scripts": {
  "version": "npm version patch"
}
```
5. To run the code, open the VSCode command terminal using ctrl + and type npm version patch. This will run in the terminal until you close your project.

6. To view the updated version, open another terminal and run your app, then navigate to the package.json file. Each time you save your project, the version will be updated.

Enjoy!
