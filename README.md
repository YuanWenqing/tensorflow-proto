# TensorFlow proto

Since [TensorFlow](https://github.com/tensorflow/tensorflow) and [TensorFlow-Serving](https://github.com/tensorflow/serving) 
do not support official jars for their protobuf files right now, 
we create this project to do it just for convenience and sharing.

## Usage

`tensorflow-proto` contains and compiles all protobuf files in TensorFlow,
and `serving-proto` does that for TensorFlow-Serving.
Since compiling of `serving-proto` imports protobuf from `tensorflow-proto`,
actually `serving-proto` contains all that `tensorflow-proto` has.
Therefore `serving-proto` is the only library you need when you want to do something with TensorFlow-Serving. 


maven:

~~~xml
<dependencies>
    <dependency>
        <groupId>xyz.codemeans.tensorflow4j</groupId>
        <artifactId>tensorflow-proto</artifactId>
        <version>1.12.0</version>
    </dependency>

    <dependency>
        <groupId>xyz.codemeans.tensorflow4j</groupId>
        <artifactId>serving-proto</artifactId>
        <version>1.12.0</version>
    </dependency>
</dependencies>

~~~

gradle:

~~~groovy
implementation 'xyz.codemeans.tensorflow4j:tensorflow-proto:1.12.0'

implementation 'xyz.codemeans.tensorflow4j:serving-proto:1.12.0'
~~~


## Sync Protobuf Files

Clone [TensorFlow](https://github.com/tensorflow/tensorflow) and [TensorFlow-Serving](https://github.com/tensorflow/serving) , checkout the latest version(or any other version you want), 

~~~shell
git clone -b 1.15.0 git@github.com:tensorflow/serving.git tensorflow-projects/serving
git clone -b 1.15.0 git@github.com:tensorflow/tensorflow.git tensorflow-projects/tensorflow
~~~

and then run:

~~~bash
python sync_proto.py tensorflow-projects/tensorflow tensorflow-proto
python sync_proto.py tensorflow-projects/serving serving-proto
~~~

NOTE: a little BUGFIX must be done to `tensorflow/compiler/tf2xla/host_compute_metadata.proto`,
change `java_outer_classname` as below:


~~~proto
// option java_outer_classname = "Tf2XlaProtos";
option java_outer_classname = "HostComputeMetadataProtos";
~~~

## Build JAR Library

Requirements:

* protoc
* gradle
* protobuf lib

run:

~~~bash
gradle clean build
~~~

If you want to publish the built jars, you can run:

~~~bash
# publish to local maven repository (`~/.m2`)
gradle local
~~~

or

~~~bash
# config maven publishing and publish to nexus
gradle publish
~~~


## Version

Current version of TensorFlow and TensorFlow-Servng are both 1.12.0.

If you want to build library for another version, you MUST:

* checkout TensorFlow and TensorFlow-Serving source of exact version you want, then sync all proto files
* change version and publish setting in `build.gradle` for publishing somewhere

 