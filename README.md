#Installation
1. **Bower**: *bower install angular-filesystem-reader --save*

2. **Manual**: Everything resides inside angular_filesystem_reader.js. Use git or download it directly, remember to include the path in your html page and you're good to go.

#Guide
All the logic of the filesystem reader resides inside *angular_filesystem_reader.js*. It consists of an angular service, called **angularFsReader** which resides inside the angular module called **angular-filesystem-module**.

The service contains two 'public' properties, **getFiles** (method) and **liveArray** (valiable).

**liveArray** can be optionally used for databinding in controllers, to access the live/updating array of file objects asynchronously, while **getFiles** executes. Note that **getFiles** needs to be invoked before **liveArray** returns any meaningfull/non-empty array.


```javascript
angularFsReader.getFiles(nativeURL, validExtensions, skippable_folders, specific_folders, qualifiedFileHandler, reportError);
```
###The parameters:

1. **nativeURL** (String):
This has to be the nativeURL (.nativeURL property of the directory entry object) of your intended root directory. 
In android, it could be something like: *file:///storage* or *file:///storage/emulated/0/* (internal memory)

2. **validExtensions** (Array of String):
This has to be the array of strings containing the extensions of the files you're looking for. E.g. *['mp3', 'wav']*

3. **skippable_folders** (Array of Strings) optional or null:
If you want certain folders to be skipped, mention them as the elements of this array. 
Each element should start with the root directory you have indicated in the **nativeURL** parameter. It should be of the form: *your_root_directory/some_directory/skip_this_directory*. E.g. ['storage/sdcard1/Music', 'storage/sdcard1/Books']. Provide **null** if you don't need to skip/ignore any folders.

4. **specific_folders** (Array of Strings) optional or null:
If you want only certain folders to be **exclusively** scanned, mention them as the elements of this array. 
Each element should start with the root directory you have indicated in the **nativeURL** parameter. It should be of the form: *your_root_directory/some_directory/an_exclusive_directory*. E.g. ['storage/sdcard1/Music', 'storage/sdcard1/Books']. Provide **null** if you don't need to specify any exclusive folders.

5. **qualifiedFileHandler** (Function):
This is the success handler callback that you need to specify. Your callback should expect an array of qualifying file entry objects as its parameter.
Note - In case you're willing to use the **Promise** route (see below), you can specify **null** instead of a callback.

6. **reportError** (Function):
This is the error handler callback that you need to specify, which will be invoked in case the root **nativeURL** you specify is not formatted properly or if such a directory does not exist.

###Returns:
A **Promise** that resolves to an array of qualifying file entry objects.
**Note** - You don't need to use this, if you're passing the **qualifiedFileHandler** callback.



##Examples:

**Note** - Remember that the method **getFiles** should be called only after the *deviceready* event has fired.

Using **qualifiedFileHandler** callback:
```javascript
angular.module('myApp', ['angular-fs-reader']).service('DeviceDataService', ['$q' 'angularFsReader', function ($q, angularFsReader) {

        document.addEventListener('deviceready', function () {
            
            angularFsReader.getFiles('file:///storage', ['mp3', 'wav'], ['storage/sdcard1/Videos', 'storage/sdcard1/Books'], ['storage/emulated/0/', 'storage/sdcard1'], 
			function(array){
				console.log("My list of qualifying files!");
                console.log(array);
            }, 
			function(e){
				console.log("Error:" + e);
            });

        });
});
```

Using the returned **Promise**:
```javascript
angular.module('myApp', ['angular-fs-reader']).service('DeviceDataService', ['$q' 'angularFsReader', function ($q, angularFsReader) {

        document.addEventListener('deviceready', function () {
            
            angularFsReader.getFiles('file:///storage', ['mp3', 'wav'], ['storage/sdcard1/Videos', 'storage/sdcard1/Books'], ['storage/emulated/0/', 'storage/sdcard1'], null, function(e){
				console.log("Error:" + e);
            })
			.then(function(array){
				console.log("My list of qualifying files!");
                console.log(array);
            });;            
        });
});
```

Using the **liveArray** property for data-binding:
```javascript
angular.module('myApp', ['angular-fs-reader']).controller('myController', ['$scope', 'angularFsReader', function ($scope, angularFsReader) {

	document.addEventListener('deviceready', function () {
		angularFsReader.getFiles('file:///storage', ["mp3"], ['storage/sdcard1/Videos', 'storage/sdcard1/Books'], ['storage/emulated/0/', 'storage/sdcard1'], null, function (e) {
			console.log(e);
		});

		$scope.localFiles = angularFsReader.liveArray;
	});
}]);
```
