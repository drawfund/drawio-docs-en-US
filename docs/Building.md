# Building

https://github.com/jgraph/drawio/wiki/Building

## Building draw.io

draw.io consists of two parts, currently. The main part is the client-side JavaScript code. You can create the minified JavaScript using the default "all" task of the [Ant build.xml file](https://github.com/jgraph/drawio/blob/dev/etc/build/build.xml) which you can execute by running [`ant`](https://ant.apache.org/) in the `etc/build` folder of the repo.

Note, if you use just the client-side code, you'll be missing the Gliffy and .vsdx importers, the embed support, icon search and publishing to Imgur.

If you want to build the full war with the Java server-side code and the client-side JavaScript, invoke the "war" task in the [Ant build.xml file](https://github.com/jgraph/drawio/blob/dev/etc/build/build.xml). Deploy the resulting .war file to a servlet engine.

## Deploy the build

After building draw.io, point a web server at the /war directory and you'll get the client functionality served at /index.html.

One easy way to deploy is to map the Github pages site to your main branch, [as we have indone on the main project](https://jgraph.github.io/drawio/src/main/webapp/index.html).
