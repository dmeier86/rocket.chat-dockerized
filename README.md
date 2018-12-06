# docker-compose for rocket.chat
my goal was to have a "scalable" docker-compose solution which is working behind a reverse-proxy over TLS with an mongodb-cluster whose db-data is stored locally e.g. on a ec2 mounted volume which can be snapshotted on a regular basis.
i wanted to have version upgrades as easy as possible.
after a few experiments i endet up with this docker-compose script which is scalable (multiple containers on a single node).

## requironments
docker-compose

## usage
create a folder called **rc**.
inside this folder you should create some more folders:
 - data
 - tls
 - uploads

because you should alway's use TLS those day's make sure you have your **crt** and your **key** file ready *exactly* named as

 - default.crt
 - default.key

inside the **tls** folder.
put the *docker-compose.yml* inside your **rc** folder and run it with parameter --scale=rc=x.
x will be the amout of running rocket.chat container nodes linked in nginx reverse-proxy.
e.g.

    docker-compose up -d --scale=rc=2
sure you can scale down your docker-nodes also - and yep both way's will upgrade the nginx reverse-proxy list.

## version upgrade
stop old containers

    docker-compose stop <CONTAINER_1> <CONTAINER_2>
delete old containers

	docker-compose rm CONTAINER_1> <CONTAINER_2>
make your changes in docker-compose-yml e.g.

    image: mongo:3.2
to

    image: mongo:3.4
and start up your cluster agin with --scale.
you should chek for old images wich are no longer in use:

    docker images -a
and delete them to via

    docker rmi IMAGE_ID

## wait - there's more!
##### confine resources
[https://docs.docker.com/compose/compose-file/#resources](https://docs.docker.com/compose/compose-file/#resources)

##### understanding container RAM useage
[http://fuzzyblog.io/blog/docker/2017/06/25/docker-tutorial-understanding-container-memory-usage.html](http://fuzzyblog.io/blog/docker/2017/06/25/docker-tutorial-understanding-container-memory-usage.html)

## contribute
please feel free to contribute. If there's any info missing or you've found out something new, please feel free to contribute to my stuff  ![](https://img.icons8.com/metro/1600/like.png =15x15)
