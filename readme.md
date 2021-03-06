# Whitewalker Adapters
This is test runner adapters for whitewalker.

## What's ready

- nightwatch adapter is functional
- protractor is functional for running test but still lack configWriter and configTranslator method to allow installer to generate a config.

## Making a framework adapter to use in whitewalker

So you want to use whitewalker but you use another framework than nightwatch or protractor.
No problem just create your own framework adapter.

First your adapter module must reside in a file named after the adapter name e.g. nightwatch.js, protractor.js...
Basicly it will look like this:

```javascript
module.exports = {
	// how the configFileName should be named
	configFileName: 'nightwatch.json'
	// command line to launch with placeholders for selenium environment name and the testFilename to execute
	, cmdTemplate: 'nightwatch -e {{environment}} -c ./nightwatch.json -t {{testFilename}}'
	// return an internal representation of a file containing an adapter config
	, configLoader: function(cfgPath){
		/*
		must return a config object with following structure:
		{
			selenium: {
				path: data.selenium.server_path || ''
				, host: data.selenium.host || '127.0.0.1'
				, port: data.selenium.port || 4444
			}
			, environments:  {
				'environmentname': { 'desiredCapabilities': {}}
			}
			, adapter: data.whitewalker || {}
			, rawConfig: { /*{object representing the real config data} */ }
		}
		*/
	}
	, configTranslator: function(cfg){
		// return an object of the translated config from internal representation to config format required by the adapter
	}
	, configWriter: function(cfgPath, cfg){
		// take an internal config representation, translate it to the adapter format and write it to the disk.
		// it must return the output filename on success or false on error
	}
	,testPreparator: function(testName, testPath, tmpPath, environment){
		// receive name of the testCase to run, path to the test directory, path to temporary directory and the test environment settings
		// it must return a promise of  full path to the temporary file to test
	}
};
```
