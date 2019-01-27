# TensorFlow proto

Since [TensorFlow](https://github.com/tensorflow/tensorflow) and [TensorFlow-Serving](https://github.com/tensorflow/serving) 
do not support official jars for their protobuf files right now, 
we create this project to do it just for convenience and sharing.


## Sync Protobuf Files

Clone those two projects, 
checkout the latest version(or any other version you want), 
and then run:

~~~bash
python sync_proto.py /path/to/tensorflow_project tensorflow
python sync_proto.py /path/to/tensorflow_serving_project serving
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
# publish to nexus
gradle publish
~~~

NOTE: protoc compiling for `serving` depends on protobuf in `tensorflow`, thus <strong>serving.jar contains all in tensorflow.jar</strong>

## Version

Current version of TensorFlow and TensorFlow-Servng are both 1.12.0.

If you want to build library for another version, you MUST:

* checkout tensorflow and tensorflow-serving source of exact version you want, then sync all proto files
* change version setting in `build.gradle` for publishing somewhere

 