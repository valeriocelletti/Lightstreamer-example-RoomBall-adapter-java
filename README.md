# Lightstreamer - Room-Ball Demo - Java Adapter #
<!-- START DESCRIPTION lightstreamer-example-roomball-adapter-java -->

This project includes the resources needed to develop the Metadata and Data Adapters for the [Room-Ball Demo](https://github.com/Weswit/Lightstreamer-example-RoomBall-client-javascript) that is pluggable into Lightstreamer Server. Please refer [here](http://www.lightstreamer.com/latest/Lightstreamer_Allegro-Presto-Vivace_5_1_Colosseo/Lightstreamer/DOCS-SDKs/General%20Concepts.pdf) for more details about Lightstreamer Adapters.<br>
The <b>Room-Ball Demo</b> implements a simple pure Server-side Mode multiplayer soccer game:
- Physics runs on server side only
- User commands are streamed from clients to server
- Position updates are streamed from server to clients
- Clients are pure renderer (no feedback, no prediction, no interpolation)
 
For more information, see this slide deck from the HTML5 Developer Conference:<br>
http://www.slideshare.net/alinone/slides-html5-devconf-20131022

The project is comprised of source code and a deployment example.

## Java Data Adapter and MetaData Adapter ##
A Java Adapter implementing both the [SmartDataProvider](http://www.lightstreamer.com/docs/adapter_java_api/com/lightstreamer/interfaces/data/SmartDataProvider.html) interface and the [MetadataProviderAdapter](http://www.lightstreamer.com/docs/adapter_java_api/com/lightstreamer/interfaces/metadata/MetadataProviderAdapter.html) interface, to inject data into Lightstreamer server with real time information about the movement of every player in the room. The adapter accepts also message submission for the chat room.<br>
The adapter receives input commands from Lightstreamer server, which forwards messages arrived from clients to the adapter in relation to:
- movement commands;
- changing last message for the player.

The Metadata Adapter inherits from the reusable [LiteralBasedProvider](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java) and just adds a simple support for message submission. It should not be used as a reference, as no guaranteed delivery and no clustering support is shown.
<!-- END DESCRIPTION lightstreamer-example-roomball-adapter-java -->

# Build #

If you want to skip the build process of this Adapter please note that in the [deploy release](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java/releases) of this project you can find the `deploy.zip` file that contains a ready-made deployment resource for the Lightstreamer server. <br>
Otherwise follow these steps:

* Get the `ls-adapter-interface.jar`, `ls-generic-adapters.jar`, and `log4j-1.2.15.jar` files from the [latest Lightstreamer distribution](http://www.lightstreamer.com/download).
* Get the `ua-parser-1.2.2.jar` file from [ua_parser Java Library](https://github.com/tobie/ua-parser/tree/master/java);
* Get the `snakeyaml-1.11.jar` files from [SnakeYAML](https://code.google.com/p/snakeyaml/)
* Get the `jbox2d-library-2.2.1.1.jar` file from [JBox2D](https://code.google.com/p/jbox2d/)
* Build the jar `LS_RoomBall_Demo_Adapters.jar` with commands like these:
```sh
 >javac -source 1.7 -target 1.7 -nowarn -g -classpath lib/log4j-1.2.15.jar;lib/ls-adapter-interface.jar;lib/ls-generic-adapters.jar;lib/jbox2d-library-2.2.1.1.jar;lib/ua-parser-1.2.2.jar;lib/snakeyaml-1.11.jar -sourcepath src/ -d tmp_classes src/com/lightstreamer/adapters/RoomBall/RoomBallAdapter.java
 
 >jar cvf LS_RoomBall_Demo_Adapters.jar -C tmp_classes com
```

# Deploy #

You have to create a specific folder to deploy the Room-Ball Demo Adapters otherwise get the ready-made `RommBallDemo` deploy folder from `deploy.zip` of the [latest release](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java/releases) of this project.<br>
If you choose to create you own folder, follow the next steps, otherwise skip them. 

1. Create a new folder, let's call it `RoomBall`, and a `lib` folder inside it.
2. Copy the jar file of the adapter `LS_RoomBall_Demo_Adapters.jar`, compiled in the previous section, in the newly created "lib" folder.
3. Copy the files: `jbox2d-library-2.2.1.1.jar`, `ua-parser-1.2.2.jar` and `snakeyaml-1.11` from `Lightstreamer/lib` in the newly created `lib` folder.
4. Create an `adapters.xml` file inside the `RoomBall` folder and use the following content (this is an example configuration, you can modify it to your liking):

```xml      
<?xml version="1.0"?>
<adapters_conf id="ROOMBALL">
    <metadata_provider>
        <adapter_class>com.lightstreamer.adapters.RoomBall.RoomBallMetaAdapter</adapter_class>

        <!--
          TCP port on which Sun/Oracle's JMXMP connector will be
          listening.
        -->
        <param name="jmxPort">9999</param>
        
        <messages_pool>
            <max_size>10</max_size>
            <max_free>10</max_free>
        </messages_pool>
        
    </metadata_provider>
    
    <data_provider>
        <adapter_class>com.lightstreamer.adapters.RoomBall.RoomBallAdapter</adapter_class>
        
        <!--
          Frame rate for physics calculations. In milliseconds.
        -->
        <param name="frameRate">10</param>

        <!--
          Number of steps for a single frame
        -->
        <param name="stepsPerFrame">4</param>
        
          
    </data_provider>
</adapters_conf>
```
<br> 

Now your `RoomBall` folder is ready to be deployed in the Lightstreamer server, please follow these steps:<br>

1. Make sure you have installed Lightstreamer Server, as explained in the `GETTING_STARTED.TXT` file in the installation home directory.
2. Make sure that Lightstreamer Server is not running.
3. Copy the `RoomBall` directory and all of its files to the `adapters` subdirectory in your Lightstreamer Server installation home directory.
4. [Optional] Supply a specific "LS_RoomDemo_Logger" and "LS_demos_Logger" category in logback configuration `Lightstreamer/conf/lightstreamer_log_conf.xml`.
5. Lightstreamer Server is now ready to be launched.

Please test your Adapter with the [client](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java#clients-using-this-adapter) below.

# See Also #

## Clients using this Adapter ##
<!-- START RELATED_ENTRIES -->

* [Lightstreamer - Room-Ball Demo - HTML Client](https://github.com/Weswit/Lightstreamer-example-RoomBall-client-javascript)

## Related projects ##

* [Lightstreamer - Reusable Metadata Adapters - Java Adapter](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java)
* [Lightstreamer - 3D World Demo - Java Adapter](https://github.com/Weswit/Lightstreamer-example-3DWorld-adapter-java)
* [Lightstreamer - 3D World Demo - Three.js Client](https://github.com/Weswit/Lightstreamer-example-3DWorld-client-javascript)

<!-- END RELATED_ENTRIES -->
# Lightstreamer Compatibility Notes #

- Compatible with Lightstreamer SDK for Java Adapters since 5.1

