# Lightstreamer - Room-Ball Demo - Java Adapter

<!-- START DESCRIPTION lightstreamer-example-roomball-adapter-java -->

This project includes the resources needed to develop the Metadata and Data Adapters for the [Room-Ball Demo](https://github.com/Weswit/Lightstreamer-example-RoomBall-client-javascript) that is pluggable into Lightstreamer Server.<br>

The *Room-Ball Demo* implements a simple pure Server-side Mode multiplayer soccer game:
* Physics runs on server side only
* User commands are streamed from clients to server
* Position updates are streamed from server to clients
* Clients are pure renderer (no feedback, no prediction, no interpolation)
 
For more information, see this slide deck from the HTML5 Developer Conference:<br>
http://www.slideshare.net/alinone/slides-html5-devconf-20131022

## Details

The project is comprised of source code and a deployment example.

### Dig the Code

#### Java Data Adapter and MetaData Adapter

A Java Adapter implementing both the [SmartDataProvider](http://www.lightstreamer.com/docs/adapter_java_api/com/lightstreamer/interfaces/data/SmartDataProvider.html) interface and the [MetadataProviderAdapter](http://www.lightstreamer.com/docs/adapter_java_api/com/lightstreamer/interfaces/metadata/MetadataProviderAdapter.html) interface, to inject data into Lightstreamer server with real time information about the movement of every player in the room. The adapter accepts also message submission for the chat room.<br>
The adapter receives input commands from Lightstreamer server, which forwards messages arrived from clients to the adapter in relation to:
* movement commands;
* changing last message for the player.

The Metadata Adapter inherits from the reusable [LiteralBasedProvider](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java) and just adds a simple support for message submission. It should not be used as a reference, as no guaranteed delivery and no clustering support is shown.

<!-- END DESCRIPTION lightstreamer-example-roomball-adapter-java -->


### The Adapter Set Configuration

This Adapter Set is configured and will be referenced by the clients as `ROOMBALL`. 

The `adapters.xml` file for the *Room-Ball Demo*, should look like:

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

Please refer [here](http://www.lightstreamer.com/latest/Lightstreamer_Allegro-Presto-Vivace_5_1_Colosseo/Lightstreamer/DOCS-SDKs/General%20Concepts.pdf) for more details about Lightstreamer Adapters.

## Install

If you want to install a version of the *Room-Ball Demo* in your local Lightstreamer Server, follow these steps.

* Download *Lightstreamer Server* (Lightstreamer Server comes with a free non-expiring demo license for 20 connected users) from [Lightstreamer Download page](http://www.lightstreamer.com/download.htm), and install it, as explained in the `GETTING_STARTED.TXT` file in the installation home directory.
* Make sure that Lightstreamer Server is not running.
* Get the `deploy.zip` file of the [latest release](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java/releases), unzip it, and copy the `RoomBall` folder into the `adapters` folder of your Lightstreamer Server installation.
* [Optional] Supply a specific "LS_RoomDemo_Logger" and "LS_demos_Logger" category in logback configuration `Lightstreamer/conf/lightstreamer_log_conf.xml`.
* Copy the `ls-generic-adapters.jar` file from the `lib` directory of the sibling "Reusable_MetadataAdapters" SDK example to the `shared/lib` subdirectory in your Lightstreamer Server installation home directory.
* Launch Lightstreamer Server.
* Test the Adapter, launching the [Lightstreamer - Room-Ball Demo - HTML Client](https://github.com/Weswit/Lightstreamer-example-RoomBall-client-javascript) listed in [Clients Using This Adapter](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java#clients-using-this-adapter).

## Build

To build your own version of `LS_RoomBall_Demo_Adapters.jar` instead of using the one provided in the `deploy.zip` file from the [Install](https://github.com/Weswit/Lightstreamer-example-RoomBall-adapter-java#install) section above, follow these steps.

* Download this project.
* Get the `ls-adapter-interface.jar`, `ls-generic-adapters.jar`, and `log4j-1.2.15.jar` files from the [latest Lightstreamer distribution](http://www.lightstreamer.com/download), and copy them into the `lib` directory.
* Get the `ua-parser-1.2.2.jar` file from [ua_parser Java Library](https://github.com/tobie/ua-parser/tree/master/java) and copy it into the `lib` directory.
* Get the `snakeyaml-1.11.jar` files from [SnakeYAML](https://code.google.com/p/snakeyaml/) and copy it into the `lib` directory.
* Get the `jbox2d-library-2.2.1.1.jar` file from [JBox2D](https://code.google.com/p/jbox2d/) and copy it into the `lib` directory.
* Create the `LS_RoomBall_Demo_Adapters.jar` with commands like these:
```sh
 >javac -source 1.7 -target 1.7 -nowarn -g -classpath lib/log4j-1.2.15.jar;lib/ls-adapter-interface.jar;lib/ls-generic-adapters.jar;lib/jbox2d-library-2.2.1.1.jar;lib/ua-parser-1.2.2.jar;lib/snakeyaml-1.11.jar -sourcepath src/ -d tmp_classes src/com/lightstreamer/adapters/RoomBall/RoomBallAdapter.java
 
 >jar cvf LS_RoomBall_Demo_Adapters.jar -C tmp_classes com
```

## See Also

### Clients Using This Adapter
<!-- START RELATED_ENTRIES -->

* [Lightstreamer - Room-Ball Demo - HTML Client](https://github.com/Weswit/Lightstreamer-example-RoomBall-client-javascript)

### Related Projects

* [Lightstreamer - Reusable Metadata Adapters - Java Adapter](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java)
* [Lightstreamer - 3D World Demo - Java Adapter](https://github.com/Weswit/Lightstreamer-example-3DWorld-adapter-java)
* [Lightstreamer - 3D World Demo - Three.js Client](https://github.com/Weswit/Lightstreamer-example-3DWorld-client-javascript)

<!-- END RELATED_ENTRIES -->
## Lightstreamer Compatibility Notes

* Compatible with Lightstreamer SDK for Java Adapters since 5.1

