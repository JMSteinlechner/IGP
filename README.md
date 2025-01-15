# My Custom Sakai Tool

```
git clone --recurse-submodules https://github.com/JMSteinlechner/IGP
```

then follow rest of docker-sakai-builder guide

```
cd IGP/docker-sakai-builder

docker stop sakai-mariadb; docker stop sakai-tomcat; docker rm sakai-mariadb; docker rm sakai-tomcat; docker rm sakai-build; git clean -f -d

# May need to run this to clean up the deploy, run clean_deploy if the case
cd sakai
../sakai-dock.sh clean_deploy
../sakai-dock.sh build
cd ..

# if build success
./sakai-dock.sh clean_data
./sakai-dock.sh mariadb

# wait 1 min ca.. for db to start
# Remove if you already made a sakai-tomcat docker image and you want to a fresh one!
# docker rm sakai-tomcat
./sakai-dock.sh tomcat
```
