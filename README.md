# Flume Plugin: MultiLineExecSource

Flume-NG 's ExecSource is aimed at collecting every line in xxx.log as a flume event. The line is ended with '$' by default. But in some situations, one log is multiline, for instance, the error logs are mostly multiline because of stacktrace. So I have developed a MultiLineExecSource which based on ExecSource.

**NOTE: MultiLineExecSource plugin is built for Flume-NG and will not work on Flume-OG**

**NOTE 2: It lacks comprehensive test coverage. Of course contributions are welcome to make its more stable and useful**

## Compilation

The project is maintained by [Maven](http://maven.apache.org/).

## Installation instructions

After your compilation, you should ship the target jar `flume-source-plugin-1.0-SNAPSHOT.jar`  to the `$FLUME_HOME/flume-ng/lib/`. Then you can edit flume.conf to use the MultiLineExecSource instead of the default ExecSource.

Now follows a brief overview of MultiLineExecSource with usage instructions.

## Sources

### MultiLineExecSource

The MultiLineExecSource is used for generating one Flume event which is composed of  multiple lines in the log. It will inspect every line to see whether it is starting with a symbol which means a new line. The symbol is satisfying some kind of regex.

For instance, the HDFS's datanode log is usually starting with '2016-03-18 17:53:40,278'. It can be expressed with regex '\s?\d\d\d\d-\d\d-\d\d\s\d\d:\d\d:\d\d,\d\d\d'. So MultiLineExecSource will distinguish every line with this regex. If a line starts with it, it is a new line. Otherwise, it belongs to the previous line.

The MultiLineExecSource is based on the regular exec source and includes the same parameters. It also adds one additional one:

* **lineStartRegex**: This is used to distinguish every line.


Example config:

```
agent.sources.hdfs_namenode_src.type = com.urey.flume.MultiLineExecSource
agent.sources.hdfs_namenode_src.lineStartRegex = \\s?\\d\\d\\d\\d-\\d\\d-\\d\\d\\s\\d\\d:\\d\\d:\\d\\d,\\d\\d\\d
```










