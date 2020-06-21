# run, start, exec 总结

* 如果容器不存在，使用`docker run` 命令根据指定镜像创建容器。
* 如果容器存在：
  * 如果容器在（后台）运行，使用`docker exec` 命令进入容器
  * 如果容器不在运行，使用`docker container start` 命令启动并进入容器



