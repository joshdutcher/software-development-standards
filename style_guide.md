# \[Company\] Development Standards - Style Guide

The Style guide comprises what should be considered the standard coding elements that are required to ensure a high level of technical interoperability between shared code. This should allow employees to easily read and understand each others' code -- any developer familiar with the Standards should be able to work on any codebase which conforms to the Standards. We should write code which minimizes the time it takes someone else to understand it, including new employees.

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

## HTML
* HTML code **SHOULD** conform to the [w3schools HTML5 Style Guide and Coding Conventions](https://www.w3schools.com/html/html5_syntax.asp).
* Developers writing HTML **SHOULD** familiarize themselves with [w3schools HTML5 Semantic Elements](https://www.w3schools.com/html/html5_semantic_elements.asp) and use them when appropriate.

## CSS
* CSS code **SHOULD** conform to [WordPress' CSS Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/css/).

## JavaScript/TypeScript
* JavaScript code **SHOULD** conform to the [w3schools JavaScript Style Guide and Coding Conventions](https://www.w3schools.com/js/js_conventions.asp).
* JavaScript developers **SHOULD** conform to [this additional set of JavaScript code conventions](javascript.md) which was based on [these conventions](http://javascript.crockford.com/code.html) from crockford.com.

### Common Components
* **Application Framework** - [Angular](#angular)
* **Logging Framework**
    * JS Developers **MAY** use automatic logging to a centralized visible location (production) and **SHOULD** use runtime logging (development) for javascript errors.
    * Suggested Tools
        * [Bugsnag](https://www.bugsnag.com/platforms/javascript/) - This **MAY** be used on a per project basis. Client-side javascript logging can be expensive to the client's resources.
        * Built in Browser Debugging Tools
* **Testing Frameworks**
    * Unit Tests - [Jasmine](https://jasmine.github.io/) + [Karma](https://karma-runner.github.io/1.0/index.html)
* **Code Quality Tooling**
    * JS Developers **SHOULD** use plugins and libraries to automatically format code to standards and detect code execution errors within the editor.
    * Suggested Tools
        * JS Lint
            * [jslint plugin](https://atom.io/packages/jslint) for atom editor.
            * [jslint plugin](https://github.com/73rhodes/Sublime-JSLint) for sublime text.
            * [built in jslint plugin](https://www.jetbrains.com/help/webstorm/2017.1/jslint.html) for webstorm IDE.

## Angular
* Developers writing in the Angular framework **SHOULD** conform to [Angular's style guide](https://angular.io/docs/ts/latest/guide/style-guide.html).
* Developers writing in the Angular framework **SHOULD** familiarize themselves with [Angular's architecture overview](https://angular.io/docs/ts/latest/guide/architecture.html).

### Common Components
* **Logging Framework**
    * Angular JS Developers **MAY** use automatic logging to a centralized visible location (production) and **SHOULD** use runtime logging (development) for javascript errors.
    * Suggested Tools
        * [Bugsnag](https://www.bugsnag.com/platforms/javascript/) - This **MAY** be used on a per project basis. Client-side javascript logging can be expensive to the client's resources.
        * Built in Browser Debugging Tools
* **Testing Frameworks**
    * Unit Tests - [Jasmine](https://jasmine.github.io/) + [Karma](https://karma-runner.github.io/1.0/index.html) + [Angular Testing Classes](https://angular.io/docs/ts/latest/guide/testing.html) _recommended by angular dev team_.
    * e2e / Integration Tests - [Protractor](http://www.protractortest.org/#/) _recommended by angular dev team_.
* **Code Quality Tooling**
    * Angular Developers **SHOULD** use plugins and libraries to automatically format code to standards and detect code execution errors within the editor and at compile time.
    * Suggested Tools
        * JS Lint
            * [jslint plugin](https://atom.io/packages/jslint) for atom editor.
            * [jslint plugin](https://github.com/73rhodes/Sublime-JSLint) for sublime text.
            * [built-in jslint plugin](https://www.jetbrains.com/help/webstorm/2017.1/jslint.html) for webstorm IDE.
        * Transpiler
            * [Typescript](https://www.typescriptlang.org/docs/home.html)

## PHP & Laravel
* PHP code **SHOULD** conform to the [PSR-2 standard](http://www.php-fig.org/psr/psr-2/) coding style guide and the [PSR-4 standard](http://www.php-fig.org/psr/psr-4/) for Class Autoloading.
* PHP developers **SHOULD** always write documentation blocks and those blocks **SHOULD** be compatible with [PHPDoc](https://www.phpdoc.org/).

### Common Components
* **Application Framework**
  * [Lumen](https://lumen.laravel.com) for small services without a GUI
  * [Laravel](https://laravel.com/) for larger projects that require a GUI
* **Logging Framework**
  * PHP developers **SHOULD** enable automatic centralized logging to a visible location on the server of server side errors and any other events we want to log
  * Suggested Tools
	  * [Monolog](https://github.com/Seldaek/monolog) _default in Lumen/Laravel_
* **Testing Frameworks**
    * **Unit Tests** - [PHPUnit](https://phpunit.de/)
* **Code Quality Tooling**
	* PHP developers **SHOULD** use plugins or functionality to automatically format code to standards and detect code execution errors within the editor
	* Suggested Tools
		* Sublime Text
			* [phpfmt](https://github.com/nanch/sublime-phpfmt) for PSR-4 style checking and [SublimeLinter and SublimeLinter-php](http://www.sublimelinter.com/) for code linting. Standard settings are available [here](settings/sublime-text-3/phpfmt.sublime-settings).
			* or [sublime-phpcs](https://github.com/benmatselby/sublime-phpcs) for style checking and code linting
		* PHPStorm
			* [PHP Code Sniffer](https://confluence.jetbrains.com/display/PhpStorm/PHP+Code+Sniffer+in+PhpStorm) for customizable style checking and code linting. Standard settings are available [here](settings/phpstorm/psr2.xml).

## C&#35;

* C# code **SHOULD** conform to Microsoft's [C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/articles/csharp/programming-guide/inside-a-program/coding-conventions).

### Common Components
- The latest version of [Visual Studio](https://www.visualstudio.com/) on Windows **SHOULD** be used as the IDE
- [NuGet](https://www.nuget.org/) **SHOULD** be used for dependency management and resolution.
- When using Visual Studio, the [Resharper](https://www.jetbrains.com/resharper/) extension **SHOULD** be used to provide real-time code quality analysis
- **.NET Framework Versions**
  - The latest version of [.NET Standard](https://docs.microsoft.com/en-us/dotnet/standard/library) **MUST** be used by non-Windows-specific Class Library projects
  - The latest version of [.NET Core](https://dotnet.github.io/) **MUST** be used by non-Windows-specific Console and ASP.NET projects
- The `Microsoft.Extensions.Logging` framework within .NET Standard **SHOULD** be used for emitting logs
- The [Nunit](https://www.nunit.org/) Testing Framework **SHOULD** be used for writing unit tests.

## SQL
* SQL code and queries **SHOULD** conform to the [Company SQL Style Guide](sql.md)

## Python
* Python code **SHOULD** conform to the [PEP 8 Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/).
* Python developers **SHOULD** familiarize themselves with this article on [Python Idioms and Efficiency](https://www.memonic.com/user/pneff/folder/python/id/1bufp).

## REST APIs

* REST APIS **SHOULD** conform to the [Company REST API Style Guide](rest_style_guide.md).

## Accessibility
**Note:** Accessibility requirements include ***design*** requirements as well as coding standards. Developers should ensure design requirements have been met by the design team and that requirements such as text & background colors, alt text for images, etc have been provided.

If a project's requirements include accessibility requirements:

* All front-end code for the project **MUST** meet [W3C Accessibility Standards](https://www.w3.org/standards/webdesign/accessibility).
* All front-end code for the project **SHOULD** meet [Section 508 Guidelines](https://www.section508.gov/).
* Developers **SHOULD** use online automated accessibility testing tools to test for accessibility.

## Agile Methodology

* Developers **SHOULD** conform to the [Company Agile Methodology](agile.md)
