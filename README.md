### Features
* No typings.json or typings folder, they have been replaced by better **'@types'** modules in package.json.
* ts-node(typescript execution environment for node) in cucumberOpts. 
* All scripts written with > Typescript2.0 & Cucumber2.0.
* Neat folder structures with transpiled js files in separate output folder.
* Page Object design pattern implementation.
* Extensive hooks implemented for BeforeFeature, AfterScenarios etc.
* Screenshots on failure feature scenarios.


### To Get Started

#### Pre-requisites
1.NodeJS installed globally in the system.
https://nodejs.org/en/download/

2.Chrome or Firefox browsers installed.

3.Text Editor(Optional) installed-->Sublime/Visual Studio Code/Brackets.

#### Setup Scripts
* Clone the repository into a folder
* Go inside the folder and run following command from terminal/command prompt
```
   npm install 
```
* All the dependencies from package.json and ambient typings would be installed in node_modules folder.

#### Run Scripts

* First step is to fire up the selenium server which could be done in many ways,  **webdriver-manager** proves very handy for this.The below command should download the **chrome & gecko driver** binaries locally for you!

```
npm run webdriver-update
``` 

* Then you should start your selenium server!
```
npm run webdriver-start
```

* The below command would create an output folder named 'typeScript' and transpile the .ts files to .js.
```
npm run tsc
```

* Now just run the test command which launches the Chrome Browser and runs the scripts.
```
npm test
```
![result](https://raw.githubusercontent.com/igniteram/protractor-cucumber-typescript/master/images/protractor-cucumber-typescript-result.gif)

#### Writing Features
```
Feature: To search typescript in google
@TypeScriptScenario

  Scenario: Typescript Google Search
    Given I am on google page
    When I type "Typescript"
    Then I click on search button
    Then I clear the search text
```
#### Writing Step Definitions
    
```
    import { browser } from 'protractor';
    import { SearchPageObject } from '../pages/searchPage';
    import { defineSupportCode } from 'cucumber';
    let chai = require('chai').use(require('chai-as-promised'));
    let expect = chai.expect;

    defineSupportCode(function ({Given}) {
    let search: SearchPageObject = new SearchPageObject();

        Given(/^I am on google page$/, async () => {
        await expect(browser.getTitle()).to.eventually.equal('Google');
    });
})
```

#### Writing Page Objects
```
import {$} from 'protractor';
    
export class SearchPageObject {
    public searchTextBox:any;
    public searchButton:any;

    constructor() {
        this.searchTextBox = $("input[name='q']");
        this.searchButton = $("button[name='btnG']");
    }
}
```
#### Cucumber Hooks
Following method takes screenshot on failure of each scenario
```
After(async function (scenarioResult) {
        let world = this;
    if (scenarioResult.isFailed()) {
            let screenShot = await browser.takeScreenshot();
            // screenShot is a base-64 encoded PNG
            world.attach(screenShot, 'image/png');
        }
})
```
#### CucumberOpts Tags
Following configuration shows to call specific tags from feature files
```
cucumberOpts: {
    compiler: "ts:ts-node/register",
    strict: true,
    format: ["pretty"],
    require: ['../StepDefinitions/*.ts', '../Support/*.ts'],
    tags: '@TypeScriptScenario or @CucumberScenario or @ProtractorScenario'
}
```
#### HTML Reports
Currently this project has been integrated with [cucumber-html-reporter](https://github.com/gkushang/cucumber-html-reporter), which is generated in the `reports` folder when you run `npm test`.
They can be customized according to user's specific needs.

![cucumberreporterscreen](https://raw.githubusercontent.com/igniteram/protractor-cucumber-typescript/master/images/cucumberReporter.PNG)

## Contributions
For contributors who want to improve this repo by contributing some code, reporting bugs, issues or improving documentation - PR's are highly welcome, please maintain the coding style , folder structure , detailed description of documentation and bugs/issues with examples if possible.

## License
```   
MIT License

Copyright (c) 2017 Ram Pasala
```
