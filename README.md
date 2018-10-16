Cinder-Config
=============

This is a configuration file loader and saver class designed for use with the open-source C++ library Cinder: http://libcinder.org

Usage example
------------

    params::InterfaceGlRef mParams;
    config::ConfigRef     mConfig;
    
    params::InterfaceGl::create( getWindow(), "Settings", toPixels( Vec2i( 400, 550 ) ) );
    mConfig = config::Config::create(mParams);
    
    mConfig->addParam("Bool type parameter", &boolParam);
    mConfig->addParam("Float type parameter", &floatParam);
    mConfig->addParam("Double type parameter", &doubleParam);
    mConfig->addParam("Int type parameter", &intParam);
    mConfig->addParam("Quatf type parameter", &quatfParam);
    mConfig->addParam("Enum type parameter", enumNames, &enumValue);
    ...
    mConfig->save( getAppPath() / fs::path("config.xml") );
    ...
    mConfig->load( getAppPath() / fs::path("config.xml") );

See sample in samples directory.

## Things to note
1. You make want to break your app's `setup()` into 2 functions: The first one to define the params, a second one (e.g. `postParamLoadingSetup()` to be called after calling `mConfig->load()` to do anything that depends on saved param values.
2. When using Cinder's param, be sure to make param names (the first argument of `addParam()`) unique accorss the app, even if you put params into groups, since that Cinder-Config only use params' names when saving them.
3. After a new param being added, `mConfig->load()` will fail if it can't find the param in the XML, and all params to be loaded after the new one will not load. To fix the problem, manually add the corresponding entry to the XML. You can also call `mConfig->save()`; copy the new lines from the XML; discard changes in the file (because many other param values will be overwritten to default), and paste the changes back.
