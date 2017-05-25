# jdrum-datastore-berkeley

## Intro

This is a [JDrum](https://github.com/RovoMe/JDrum) datastore extension which uses a BerkeleyDB key/value database as backing synchronization store. `InMemoryEntry` instances will first be sorted by JDrum and afterwards synchronized with the backing BerkeleyDB key/value store in a single-pass strategy. 

Depending on the invoked JDrum operation, the entry is either only checked for its availability in the datastore or added to (or updated in) the datastore. A combination of check and update is possible as well.

## Usage

In order to make us of a BerkeleyDB as a backing key/value store for synchronizing `InMemoryEntry` elements, add the following dependency next to the JDrum Maven dependency into your `pom.xml` file.

```xml
<dependency>
    <groupId>at.rovo</groupId>
    <artifactId>jdrum-datastore-berkeley</artifactId>
    <version>${drum.version}</version>
</dependency>
```

By default JDrum ships with a `SimpleDataStoreMerger` which synchronizes entris with a backing file. To make use of a BerkeleyDB managed data store instead of a file-based data store, simply specify `BerkeleyDBStoreMerger` as a datastore as depicted below.

```java
Drum<V, A> drum = new DrumBuilder<>("keyValueName", V.class, A.class)
                                           .numBuckets(4).bufferSize(64)
                                           .dispatcher(new LogFileDispatcher<>())
                                           .listener(this)
                                           .datastore(BerkeleyDBStoreMerger.class)
                                           .build();
```

## Licencing

Due to the utilization of a Sleepycat BerkeleyDB database this code has to be licensed under the Sleepycat/Oracle/BerkeleyDB software license. Any software or code that makes use of `jdrum-datastore-berkeley` therefore also has to adhere to this software license which can be found in the project root.
