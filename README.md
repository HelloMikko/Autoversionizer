# AutoVersionizer

AutoVersionizer is a simple tool that saves you time when updating the versions of your app(s). Every time you type `node update-version.js` in Visual Studio Code (VSCode), the tab is saved as normal and the version number in the `package.json` file is automatically updated.

##### [see demo](https://www.awesomescreenshot.com/video/14633323?key=ca2d045019b44d14bd8ea1d81b7c0c1f)


To auto update the version# in your electron package.json, create a file called `update-version.js`

Copy these codes to your `update-version.js` file:
```
const  fs  =  require('fs');

// Read the current package.json file
const  packageJson  =  JSON.parse(fs.readFileSync('./package.json').toString());

// Update the version number
const  version  =  packageJson.version.split('.');
version[version.length  -  1] =  parseInt(version[version.length  -  1], 10) +  1;
packageJson.version =  version.join('.');

// Write the updated package.json file
fs.writeFileSync('./package.json', JSON.stringify(packageJson, null, 2));

console.log(`Updated version number to ${packageJson.version}`);
```

Next, You can add a simple snippet to your HTML at the bottom before the `</body>` tag page, like IE:

<script>
// Fetch the JSON data from your package.json file
fetch("./../package.json").then(response  =>  response.json()).then(data  => { // Get the version number from the data
const  version  =  data.version;

// Get the element where you want to display the version number
const  versionDisplay  =  document.getElementById("version-display");

// Update the text of the element with the version number
// @ts-ignore
versionDisplay.textContent  =  `The latest version: ${version}`;
});
</script>


Note, the API `fetch("./../package.json")` needs to make sure you have the right file path to your `package.json` file in order to read the version info, usually that would be somehwhere on lines 2-4. Mine is normally on line 3, which it should not matter if it was on line 200, as long as the word `"Version:"` is what it sniffs for.

This also goes for `update-version.js`

Last thing to do, open up your app or web page and in your vscode/console, type `node update-version.js` 

Notice you will see in your package.json the "Version": has been updated and the console itself will tell you what # its updated too. In your web app or page, you should automatically see it displayed as well. You can place the snippet `<p id="version-display"></p> anywhere for it to show.

I have mine as `<p  class="text-center text-gray-600"  id="version-display"></p>` since I am running tailwindcss which is why you see the classes.

I hope you will find this fun and informational and useful to display anything from anyfile using a Local API which is very basic. Others rely on other sources such as Github Actions and so on, I prefer it the more easier way since Im already in my vsCode creating more shit for you guys =)

Enjoy!
