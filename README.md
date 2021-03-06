CFStatic is a framework for the inclusion and packaging of CSS and JavaScript files for CFML 
applications by Dominic Watson (http://fusion.dominicwatson.co.uk/).

CFStatic takes care of:

* Minifying your CSS and JavaScript
* Including your CSS and JavaScript in the correct order, with all dependencies satisfied
* Adding sensible cachebusters to CSS image paths and CSS and JavaScript includes

Key features:

* Minifies your files for you using YuiCompressor
* Compiles your LESS CSS for you
* Dependency configuration through JavaDoc style documentation
* Easy, zero-code, switching between inclusion of raw source files and minified files
* Small API, only 4 public methods (including the constructor)
* Three minify modes (All, Package and File)
* Built for production; no need for CSS and JavaScript file packaging in your build scripts, 
  code can be put in production
* Minified files are saved to disk, CF is not involved in serving the files


Quick start guide
-----------------

1. Ensure your JavaScript and CSS files are marked up with appropriate documentation (see 
  'JavaDoc style documentation', below)
2. Unpack the /org/cfstatic directory (and its contents) somewhere on your server
3. Create a mapping, '/org/cfstatic/', that points to the unpacked directory
4. Create an instance of the CfStatic component ('org.cfstatic.CfStatic'), passing in your 
   configuration (see 'Configuration', below), e.g.

    `application.cfstatic = CreateObject('component', 'org.cfstatic.CfStatic').init( argumentCollection=config );`

5. Use the include() method to include files for your requested page. Files can be included 
   individually, or you can include whole directories, e.g.

    `application.cfstatic.include('/js/plugins/myplugin.js');`

    `application.cfstatic.include('/js/core/');`

6. Use the renderIncludes() method to output the necessary HTML to include your JS and CSS, e.g.
	
    `application.cfstatic.renderIncludes('css');`

    `application.cfstatic.renderIncludes('js');`


JavaDoc Style Documentation
---------------------------
In order for CFStatic to include your JavaScript and CSS files correctly, it needs to know 
certain things about the files, i.e. jquery.ui.js depends on jquery.js, ie-only.css should 
only be output for Internet Explorer, etc. This is achieved through the use of JavaDoc style 
comments that take the form:

    /**
     * Description of your file (useful for other developers though CfStatic will ignore it)
     *
     * @attribute value
     * @attribute another value
     */

CfStatic will process the following attributes:

    @depends: This should be a path to another JavaScript or CSS file. The base path will be the 
              root of either the JS or CSS directory, depending on the type of file, i.e. within 
              a javascript file: /plugins/timers.js would translate to /path/to/js/folder/plugins/timers.js.
              You can also have external dependencies such as http://someurl.com/somefile.css

    @media:   For CSS only, indicates the target media, e.g. print. The default is 'screen, projection'
    @ie:      Internet Explorer inclusion directives. e.g. '@ie IE LT 8' to only include for IE 7 
              and below, or '@ie IE' to only include for any IE version.


Configuration
-------------
CfStatic takes a few configuration options, most of which have defaults. The options are:

    staticDirectory:   Full path to the directory in which static files reside (e.g. /webroot/static/), 
    staticUrl:         Url that maps to the static directory (e.g. http://mysite.com/static or /static)
    jsDirectory:       Relative path to the directoy in which javascript files reside. Relative to 
                       static path. Default is 'js'
    cssDirectory:      Relative path to the directoy in which css files reside. Relative to static 
                       path. Default is 'css'
    outputDirectory:   Relative path to the directory in which minified files will be output. Relative to 
                       static path. Default is 'min'
    minifyMode:        The minify mode. Options are: 'none', 'file', 'package' or 'all'. Default is 'package'.
    downloadExternals: If set to true, CfMinify will download and minify locally any external dependencies 
                       (e.g. http://code.jquery.com/jquery-1.6.1.min,js). Default = false
    debugAllowed:      Whether or not debug is allowed. Defaulting to true, even though this may seem like a 
                       dev setting. No real extra load is made on the server by a user making use of debug 
                       mode and it is useful by default. Default = true.
    debugKey:          URL parameter name used to invoke debugging (if enabled). Default = 'debug'
    debugPassword:     URL parameter value used to invoke debugging (if enabled). Default = 'true'
    forceCompilation:  Whether or not to check for updated files before compiling (true = do not check). 
                       Default = false.
    checkForUpdates:   Whether or not to attempt recompilation on every request. Default = false

Minify modes
------------

1. None: no minification
2. File: Individual files are minified and not concatenated
3. Package (default): Files are minified and concatenated in packages (all files in the same folder)
4. All: All files are minified and concatenated into a single file (one for js and one for css)

Less CSS
--------

Simply create your LESS CSS files with the .less extension anywhere in your CSS directories. CfStatic will compile those files to css, saving them as `yourfile.less.css`, before minifying and packaging the files with the rules you configure.

Resources
---------

Source: https://github.com/DominicWatson/cfstatic/
Wiki:   https://github.com/DominicWatson/cfstatic/wiki/
Issues: https://github.com/DominicWatson/cfstatic/issues/
Blog:   http://fusion.dominicwatson.co.uk/categories/CfStatic/