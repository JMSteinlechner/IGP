# Why this repo

Repo should function as a base to clone when building sakai together with bbb-tool and our custom rollcall-tool to track attendance in bbb.

Result of following build instruction should be a running sakai with bbb-tool and rollcall in a docker container. Uses test configuration for bbb-tool (added in sakai.properties of docker-sakai-builder tomcat) so no need to run big blue button in docker to test it.

Repo contains docker-sakai-builder with sakai fork included as submodule and bbb-tool fork and our custom rollcall tool as submodules in the sakai fork.

```

├── docker-sakai-builder
│   ├── sakai
│   │   ├── bbb-tool
│   │   ├── rollcall
│   ├── sakai-dock.sh
│   └── work
│       ├── mysql
│       └── tomcat
└── README.md
```

It uses forks, because we need to change the JDK docker-sakai-builder uses from 17 to 11 for it to work with sakai 23.x branch.

Sakai fork because we directly include bbb-tool and rollcall in the sakai build and need to make minor changes to the sakai pom.xml for it to work... i know is probably also possible to build each tool individually and deploy... but this is the current working state for now.

BBB-tool fork so we can change to the appropiate 23-SNAPSHOT.


# Build

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
sleep 1m

# Remove if you already made a sakai-tomcat docker image and you want to a fresh one!
# docker rm sakai-tomcat
./sakai-dock.sh tomcat
```
