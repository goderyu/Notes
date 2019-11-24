# linux下cannot find -lGL

此原因是因为linux发行版的libGL文件有后缀的原因。找到本机的libGL.so文件。我的是在`/usr/lib/x86_64-linux-gnu/libGL.so.1`，执行以下命令进行一个软连接，即可使Qt程序索引到libGL文件：

`sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/libGL.so`

