# DownloaderManager plugin for Phonegap (Cordova) #

The plugin can start and stop a file download from the web (HTTP).

## Adding the plugin to your project ##

1. To install the plugin, move DownloadManager.js to your project's www folder and include a reference to it in your html files. 
2. Create a folder called 'org/apache/cordova/plugins/DownloadManager' within your project's src/ folder.
3. And copy the .java files into that new folder.
4. Add the following to res/xml/plugins.xml file `<plugin name="DownloadManager" value="org.apache.cordova.plugins.DownloadManager.DownloadManager" />`.
5. If you want to use the Android Notification Bar to show the progress, and want icons there, the DownloadManager.java file needs to be edited. About Line 30, you have to import your own R. Of course, you need the icons too. You can use ones from the project (credits for: http://teusink.blogspot.com.es/2013/04/phonegap-android-downloader-plugin.html) and paste the icons on your projec's `res` folder. (You can generate icons here too --> http://android-ui-utils.googlecode.com/hg/asset-studio/dist/icons-notification.html but if you choose other filename for the icons, you need update the Downloader.java file too. About 136 line, change .setSmallIcon(R.drawable.ic_stat_notification) with your own file name).

## Using the plugin ##

The plugin creates the method `downloadmanager(action, options, win, fail)`

`action` may be: 

* `start` to beging to download a file
* `cancel` to stop one
* `isdownloading` to know the status of another

`options` is a json object containing the parameters (some optional) that DownloadManager accepts, it can be:

* `id:` an ID from a download process (required for "cancel" and "isdownloadind" action)
* `url:` the url to get (http://wherever.com/whatever.txt) (required for "start" action)
* `filePath:` path where to save the file (FileSystem/Download/YOUR_PATH) (optional, by default the app name)
* `fileName:` filename for the resource (FileSystem/Download/YOUR_PATH/YOUR_FILENAME.yy) (optional, by default the url name)
* `overwrite:` If the file already exists, download it again. (required for "start" action)
* `useNotificationBar:` `true`to use the Android notification bar to show the progress (optional, by defautl true)
* `startToast:` Message on toast at the beginning of the download process (optional)
* `endToast:` Message on toast at the end of the download process (optional)  
* `ticker:` Message to show on notification area where the download start (optional) 
* `notificationTitle:` Message to show on the notification bar while during download (optional)
* `cancelToast:`: Message on toast where the donwload is canceled (stoped) (optional)
 
`win` and `fail` are callback functions. `win` will be called when there is a progress in the download. The passed object is:

    {
    	id: "45345"			// id to handle the process
    	downloading: true 	// true or false 
    	total: 1000,      	// The total number of bytes to download.
    	file: "file.ext"  	// Name of the file
    	dir: "youappname"	// Path where to download
        progress: 46,     	// In percent
    }

## Using the plugin ##
	
* Basic: (start a download on a notification bar)

<pre>
downloadmanager(
	"start",
	{
		url: url2down,
    	overwrite: true
	},
	function(info) {
    	console.info(info.progress);
        // Example to stop a download on 30%:
        if (info.progress === 30)
    		downloadmanager("cancel", {id: info.id}, function() {}, function() {});
	},
	function(error) {
    	console.error(error);
	}
);
</pre>

* Check if a donwload is in progress:

<pre>
downloadmanager("cancel", {id: "example_id"}, function(res) {
	alert(res); // true or false
}, function() {});
</pre>

* More:

<pre>
downloadmanager(
	"start",
   	{
   		url: "http://wherever.com/lalalala/.txt",
	 	filePath: "from_myapp",
	 	fileName: "hello.txt", // (FileSystem)/Download/from_myappp/hello.txt
	 	overwrite: false,
	 	useNotificationBar: true,
	 	startToast: "Starting download...",
	 	endToast: "Download end!",
	 	ticker: "Downloading...",
	 	notificationTitle: "hello.txt",
	 	cancelToast: "Download canceled!"
   	},
  	function(info) {
		console.info("id:          "+info.id+"\n" +
	              	 "downloading: "+info.downloading+"\n" +
	              	 "total:       "+info.total+"\n" +
	              	 "file:        "+info.file+"\n" +
	              	 "dir:         "+info.dir+"\n" +
	              	 "progress:    "+info.progress+"\n"
		      		);
     	// progress bar example:
		$('#progress_bar').css('width', info.progress)
   	},
  	function(error) {
		alert(error);
  	}
);
</pre>

## Based on ##

http://teusink.blogspot.com.es/2013/04/phonegap-android-downloader-plugin.html

http://www.toforge.com/2011/02/phonegap-android-plugin-for-download-files-from-url-on-sd-card/

## Licence ##

The MIT License

Copyright (c) 2013 Abilio Almeida

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
