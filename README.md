
This repository serves as a base for cloning when setting up Sakai, including the **bbb-tool** and our custom **rollcall-tool** for tracking attendance in BigBlueButton (BBB).

Following the build instructions will result in a fully functional Sakai instance running inside a Docker container, preconfigured with **bbb-tool** and **rollcall-tool**. It uses a test configuration for **bbb-tool** (specified in the `sakai.properties` of `docker-sakai-builder` Tomcat), eliminating the need to run BigBlueButton in Docker for testing.

The repository includes **docker-sakai-builder**, with a Sakai fork as a submodule. Additionally, the **bbb-tool fork** and our custom **rollcall-tool** are included as submodules within the Sakai fork.

This setup ensures a streamlined build process with all necessary components integrated.

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

The repository uses forks because we need to downgrade the JDK in **docker-sakai-builder** from version 17 to 11 to ensure compatibility with the Sakai **23.x** branch.

A **Sakai fork** is used to directly include **bbb-tool** and **rollcall-tool** in the Sakai build. Additionally, minor modifications to `pom.xml` are necessary for proper integration. While it's likely possible to build and deploy each tool individually, this setup represents the current working state.

To display an icon for the **rollcall-tool** in the navigation bar, we need to define it within the Sakai source styles, as is done for other community-contributed tools. Another approach would be to create a custom theme, which would allow us to define the icon separately. However, since we are not developing a full custom theme, modifying the Sakai source is required.

A **bbb-tool fork** is included to ensure the correct **23-SNAPSHOT** version is used.

# Build

```
git clone --recurse-submodules https://github.com/JMSteinlechner/IGP
```

then follow rest of docker-sakai-builder guide or use the code below

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


Tomcat (sakai) will need atleast 5-10 minutes to start up.
Connect at http://localhost:8080/portal

Enjoy 😄
