store4j
=======

store4j from https://code.google.com/p/store4j/


简介
====

在Message系统中，经常会有数据大量的保存，并且在很短时间内删除的需求；很明显，这样的需求采用数据库是不合适的，本项目实现了通过文件来保存这类数据的方法。 在现在的硬盘数据存储中，对性能影响最大的就是随机读写，为了增加这个存储的性能，采用了以下方式避免大量的随机读写：

数据文件一直增加，顺序在文件后面添加数据，避免随机写。
不存在索引文件，通过一个日志文件来记录操作，也是顺序写日志文件。
索引建立在内存中，取数据是先通过内存中的索引获取数据位置，进行访问（该步骤存在随机读操作，但是由于索引在内存中，只会有一次读操作）
初始化时需要通过日志文件恢复内存索引。
由于数据是一直写的，没有回收操作，可能会导致数据过大，这里解决的办法是每nM作为一个文件，并且加入引用计数，当文件不存在有效数据后，删除之。
通过上面的方法，可以解决文件的读写性能，但是也会有一些限制：

索引在内存中，会引起数据不能过大（每个索引占用60字节左右）
数据不能过于分散，否则会导致数据文件不能被有效清理。

性能
====
PC机可以实现每秒万次的读写删操作。

应用
====
该存储方式比较适合消息发送、消费类的应用，现在应用在淘宝网notify系统中。