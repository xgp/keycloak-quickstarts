servlet-saml-service-provider: Servlet SAML Service Provider
=============================================================

Level: Beginner
Technologies: JavaEE
Summary: JSP Profile Application
Target Product: <span>Keycloak</span>, <span>WildFly</span>

What is it?
-----------

This quickstart demonstrates how to protect a SAML Service Provider that authenticates using <span>Keycloak</span>. 
Once authenticated the application shows the users profile information.

System Requirements
-------------------

To compile and run this quickstart you will need:

* JDK 17
* Apache Maven 3.8.6
* Wildfly 29+
* Keycloak 21+
* Docker 20+

Starting and Configuring the Keycloak Server
-------------------

To start a Keycloak Server you can use OpenJDK on Bare Metal, Docker, Openshift or any other option described in [Keycloak Getting Started guides]https://www.keycloak.org/guides#getting-started. For example when using Docker just run the following command in the root directory of this quickstart:

```shell
docker run --name keycloak \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  --network=host \
  quay.io/keycloak/keycloak:{KC_VERSION} \
  start-dev \
  --http-port=8180
```

where `KC_VERSION` should be set to 21.0.0 or higher.

You should be able to access your Keycloak Server at http://localhost:8180.

Log in as the admin user to access the Keycloak Administration Console. Username should be `admin` and password `admin`.

Import the [realm configuration file](config/realm-import.json) to create a new realm called `quickstart`.
For more details, see the Keycloak documentation about how to [create a new realm](https://www.keycloak.org/docs/latest/server_admin/index.html#_create-realm).

Starting the Wildfly Server
-------------------

In order to deploy the example application, you need a Wildfly Server up and running. For more details, see the Wildfly documentation about how to [install the server](https://docs.wildfly.org/).

Once you have Wildfly server downloaded somewhere, it is needed to install SAML adapter into it. It can be installed with the usage of Galleon tools. Please follow these instructions:

1. Download Galleon tools. The ZIP can be downloaded for example from [this page](https://github.com/wildfly/galleon/releases) and unzip to some location on your laptop.

2. Install Keycloak SAML adapter into Wildfly via Galleon. It can be done for instance with the command similar to this (replace environment variables according to your environment and used versions):

```
cd wildfly-$WILDFLY_VERSION
$GALLEON_PATH/bin/galleon.sh install org.keycloak:keycloak-saml-adapter-galleon-pack:$KC_VERSION --layers=keycloak-client-saml
```

There are alternative ways for doing that as described in the [Wildfly SAML documentation](https://docs.wildfly.org/30/WildFly_Elytron_Security.html#Keycloak_SAML_Integration). You can check what path suits
best your needs.

3. Start Wildfly server and make sure the server is accessible from `localhost` and listening on port `8080`.

Build and Deploy the Quickstart
-------------------------------

1. Open a terminal and navigate to the root directory of this quickstart.

2. The following shows the command to deploy the quickstart:

   ````
   mvn -Djakarta clean wildfly:deploy
   ````

Access the Quickstart
----------------------

You can access the application with the following URL: <http://localhost:8080/servlet-saml-service-provider>

You should be able to authenticate using any of these users:

| Username | Password | Roles              |
|----------|----------|--------------------|
| alice    | alice    | user               |

Undeploy the Quickstart
--------------------

1. Open a terminal and navigate to the root directory of this quickstart.

2. The following shows the command to undeploy the quickstart:

````
mvn -Djakarta install wildfly:undeploy
````

Running tests
--------------------

Make sure Keycloak is [running](#starting-and-configuring-the-keycloak-server).

You don't need Wildfly running because a temporary server is started during test execution.

1. Open a terminal and navigate to the root directory of this quickstart.

2. Run the following command to build and run tests:

   ````
   mvn -Djakarta clean verify
   ````

References
--------------------

* [Keycloak SAML Adapter](https://www.keycloak.org/docs/latest/securing_apps/#_saml_jboss_adapter)
* [Keycloak Documentation](https://www.keycloak.org/documentation)
* [Wildfly SAML documentation](https://docs.wildfly.org/30/WildFly_Elytron_Security.html#Keycloak_SAML_Integration)
