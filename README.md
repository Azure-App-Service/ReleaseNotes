# Azure App Service on Linux Release Notes #

With the latest updates to our back end, we pushed in May 2017, we introduced the following new features and improvements:

## May 2017 Release ##
* Stability, reliability and perf fixes in our platform code and security updates for OS.
* Site restart from portal/API will perform hard restart of the app and the kudu site
* Site slots and slot swaps will now work
* Auto scale should now work with CPU and memory metrics.
* SSH to app container from Kudu. Use {kudu site}/webssh/host or the SSH option under the Debug Console menu to get to this.
* Docker image CI/CD from DockerHub. In DockerHub webhooks, use kudu site URL with basic auth publishing credentials to call to {kudu site}/docker/hook. Additionally add appsetting DOCKER_ENABLE_CI=true
* Wildcard domain name support
* Automatic PORT detection for custom images
* Testing in Production (TiP)
* Always ON functionality to keep your site always warm and avoid cold start.
* PHP composer support during Kudu git deployment for PHP apps.
* Nodejs and docker logs will respect the Diagnostics logging setting. So you now need to turn it on if you want to see the logs. We will make this the behavior for the other stacks a bit later.


## March 2017 Release ##

* Added Websockets support.
* Auto detection of port for custom containers.
	* Platform will not autodetect the port, if a PORT app setting is defined.
* Bug fixes and performance updates.
* Security and Reliability improvements.
* Added .Net Core 1.1 to the list of built-in images.

## February 2017 Release ##

* Built-in support for Ruby 2.3

## December 2016 Release ##

* Custom Docker container image support
	* You can set the port used in the custom image by setting the **PORT** app setting
* Docker logs names is now named docker_{InstanceNumber}_out.log / err.log
* Overlapped recycling, no more 500s because of site restarts
* Added PHP XDebug
	* To enable XDebug for your PHP app, set an appsetting called PHP_ZENDEXTENSIONS with the value xdebug. Your logs will show up in the /home/LogFiles directory 
* Added PHP extensions
	* For a full list of added extensions, check our Docker files at http://github.com/appsvc
* All containers running as fakeroot
	
## September 2016 Release 2 ##

* Switching to Ubuntu 16.04 LTS

## September 2016 Release 1 ##

* Application logs will now be dumped to your /home/LogFiles directory
	* They will show up in the /home/LogFiles directory, which is a sibling to the wwwroot directory.  You can use either Kudu or FTP to grab the logs
	* For both PHP and Node js
	* Application Logs setting for all sites is enabled by default
* NodeJS using PM2 config
	* Add a JSON config file in your content root like

	```json
	{
	     "name": "worker",
	     "script": "/bin/server.js",
	     "instances": 1,
	     "merge_logs": true,
	     "log_date_format": "YYYY-MM-DD HH:mm Z",
	     "watch": ["/bin/server.js", "foo.txt"],
	     "watch_options": {
	       "followSymlinks": true,
	       "usePolling": true,
	       "interval": 5
	     }
	}
	```


	* You can set the number of node instances you want to launch using the "instances" property
	* Set AppCommandLine app setting for your web app to this JSON file, so if this file is called process.json, set the value of AppCommandLine to process.json
	* The files on whose change you want your node processes to cycle need to be added to the "watch" section
	* You need to use polling for your "watch_options"
* Custom Docker container image support
	* You can use four new app settings to specify a custom image `DOCKER_REGISTRY_SERVER_URL`, `DOCKER_REGISTRY_SERVER_USERNAME`, `DOCKER_REGISTRY_SERVER_PASSWORD`, `DOCKER_CUSTOM_IMAGE_NAME`
	* If you are getting an image from DockerHub or some public registry, you can skip the username and password. Otherwise you need to specify them as well
	* The `DOCKER_CUSTOM_IMAGE_NAME` is where you specify the tag used for the image. We are downloading the image at runtime on first request to your site.

## More Links
* [Introduction to App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-linux-intro) 
* [Creating Web Apps in App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-linux-how-to-create-a-web-app)
* [Azure App Service Web Apps on Linux FAQ](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-linux-faq) 
