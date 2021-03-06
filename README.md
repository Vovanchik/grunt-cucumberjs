grunt-cucumberjs
================

> Runs cucumberjs features and output results in various formats including html.

> Runs cucumberjs features and/or scenarios in parallel.


## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-cucumberjs --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-cucumberjs');
```

## Sample HTML Reports 

1. [Bootstrap Theme Reports with Pie Chart][3]
2. [Foundation Theme Reports][4]
3. [Simple Theme Reports][5]

## The "cucumberjs" task

### Overview
In your project's Gruntfile, add a section named `cucumberjs` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  cucumberjs: {
    options: {
      format: 'html',
      output: 'my_report.html',
      theme: 'bootstrap'
    },
    my_features: ['features/feature1.feature', 'features/feature2.feature'],
    other_features: {
      options: {
        output: 'other_report.html'
      },
      src: ['other_features/feature1.feature', 'other_features/feature2.feature']
    }
  }
});
```

If all your feature files are located in the default location of ```features/``` then just leave the feature configuation as an empty array. See following:

```
cucumberjs: {
  options: {
    format: 'html',
    output: './public/report.html',
    theme: 'foundation'
  },
  features : []
}
```

### Usage
```bash
#runs all features specified in task
$ grunt cucumberjs

#you can override options via the cli
$ grunt cucumberjs --require=test/functional/step_definitions/ --features=features/myFeature.feature --format=pretty
```

### Options

#### options.steps
Type: `String`
Default: `''`

Passes the value as ```--steps``` parameter to cucumber.

#### options.require
Type: `String`
Default: `''`

Passes the value as ```--require``` parameter to cucumber. If an array, each item is passed as a separate ```--require``` parameter.
Use if step_definitions and hooks are NOT in default location of ```features/step_definitions```

#### options.tags
Type: `String|Array`
Default: `''`

Passes the value as ```--tags``` parameter to cucumber. If an array, each item is passed as a separate ```--tags``` parameter.

#### options.theme
Type: `String`
Default: `'foundation'`
Available: `['foundation', 'bootstrap', 'simple']`

Specifies which theme to use for the html report

#### options.templateDir
Type: `String`
Default: `'features/templates'`

Location of your custom templates. Simply name the template the same as the one you are trying to override and
grunt-cucumberjs will use it over the default template

#### options.output
Type: `String`
Default: `'features_report.html'`


#### options.format
Type: `String`
Default: `'html'`
Available: `['pretty', 'progress', 'summary', 'html']`

#### options.formats
Supports multiple formatter.
Type: `Array`
Available: `['pretty', 'progress', 'summary', 'html']`

e.g. `formats: ['html', 'pretty']`

Note: `html` formatter will provide Json as well as `html` report. Multiple formatter is supported for cucumber v@0.8.0 or higher.

#### options.executeParallel
Type: `Boolean`
Default: `'undefined'`
Available: `['true', 'false']`

A flag to enable Parallel execution.

* For Cucumber version latest or greater than v0.8.0
```
  • You can run Cucumber Features and/or Scenarios Parallel
  • `--parallel scenarios` runs scenarios parallel
  • By default or `--parallel features` runs features in parallel
```

For more information visit [cucumber-parallel][8] module


* For Cucumber version lesser than v0.8.0, it requires dependency on [parallel-cucumber][6] npmjs module
```
  • You can run only Cucumber Features in Parallel
  • `--workers <number>` defines number of workers to run in parallel
```

#### options.failFast
Type: `Boolean`
Default: `'false'`
Available: `['true', 'false']`

ends the suite after the first failure

it can also be activated without setting `options.failFast` and passing `--fail-fast` as a grunt task option

#### options.dryRun
Type: `Boolean`
Default: `'false'`
Available: `['true', 'false']`

dry-run the suite and provides snippets for pending steps

it can also be activated without setting `options.dryRun` and passing `--dry-run` as a grunt task option

#### options.debug
Type: `Boolean`
Default: `'false'`
Available: `['true', 'false']`

A flag to turn console log on or off

#### options.debugger
Type: `Boolean`
Default: `'false'`
Available: `['true', 'false']`

A flag to enabling debugging from IDE like WebStorm. Limitation of this flag is it only does not support the HTML output, yet ;)

#### options.rerun
Type: `String`
Default: `undefined`

Rerun the failed scenarios recorded in the `@rerun.txt` file.

To Re-run failed scenarios:

* Set the cucumber-js task format to `rerun:@rerun.txt`
```
options: {
     format: 'rerun:@rerun.txt',
     .....
     ....
}
```
It will record all the failed scenarios to `@rerun.txt`.

Take a look at `options.formats` to generate html report

* Run failed scenarios by passing `--rerun=path/to/@rerun.txt` grunt option


#### options.compiler
Type: `String`

Sets the Cucumber Compiler options. It can also be set by passing through command line `--compiler`


#### options.reportSuiteAsScenarios
Type: `Boolean`
Default: `'false'`
Available: `['true', 'false']`

Reports total number of failed/passed Scenarios in headers if set to `true`. 
Reports total number of failed/passed Features in headers if set to `false` or `undefined`.


### Attaching Screenshots to grunt-cucumberjs HTML report

If you are using [WebDriverJS][1] (or related framework) along with [cucumber-js][2] for browser automation, you can attach screenshots to grunt-cucumberjs HTML report. Typically screenshots are taken after a test failure to help debug what went wrong when analyzing results, for example

```javascript
this.After(function (scenario, callback) {
        if(scenario.isFailed()){
            driver.takeScreenshot().then(function (buffer) {
                scenario.attach(new Buffer(buffer, 'base64').toString('binary'), 'image/png');
                 driver.quit().then(function () {
                                callback();
                 });
            });
        }
});
```

### Add texts to the cucumber steps to grunt-cucumberjs HTML report

If you are using [WebDriverJS][1] (or related framework) along with [cucumber-js][2] for browser automation, you can attach texts to grunt-cucumberjs HTML report. This helps in debugging or reviewing your results in particular to your tests data.

```javascript
this.After(function (scenario, callback) {
  scenario.attach("test data goes here");
});
```

### Pie Charts

Sample pie chart is available at [Bootstrap Theme Report with Pie Chart][3]

Two pie charts are displayed on report

1. Features: number of passed/failed features
2. Scenarios: number of passed/failed/pending scenarios.

Please note that Pie Charts are available only for Bootstrap Theme

[1]: https://code.google.com/p/selenium/wiki/WebDriverJs "WebDriverJS"
[2]: https://github.com/cucumber/cucumber-js "cucumber-js"
[3]: http://htmlpreview.github.io/?https://github.com/gkushang/grunt-cucumberjs/blob/cucumber-reports/test/cucumber-reports/cucumber-report-bootstrap.html "Bootstrap Theme Reports"
[4]: http://htmlpreview.github.io/?https://github.com/gkushang/grunt-cucumberjs/blob/cucumber-reports/test/cucumber-reports/cucumber-report-foundation.html "Foundation Theme Reports"
[5]: http://htmlpreview.github.io/?https://github.com/gkushang/grunt-cucumberjs/blob/cucumber-reports/test/cucumber-reports/cucumber-report-simple.html "Simple Theme Reports"
[6]: https://www.npmjs.com/package/parallel-cucumber
[8]: https://www.npmjs.com/package/cucumber-parallel
