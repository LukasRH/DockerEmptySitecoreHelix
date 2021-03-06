# Starting your journey 

## Prerequisites

Ensure you have followed the steps listed on the [installation documentation](https://sitecore.github.io/Helix.Examples/install.html).

The Helix examples assume you have some experience with (or at least an understanding of) Docker container-based Sitecore development. For more information, see the [Sitecore Containers Documentation](https://containers.doc.sitecore.com).

## Creating The Helix Solution



## Initialize Sitecore

Open a PowerShell administrator prompt and run the following command, replacing the `-HostName` with the name of your solution and
`-LicenseXmlPath` with the location of your Sitecore license file.

```
.\init.ps1 -HostName basic-company -LicenseXmlPath C:\License\license.xml
```

You can also set the Sitecore admin password using the `-SitecoreAdminPassword` parameter (default is "b").

This will perform any necessary preparation steps, such as populating the Docker Compose environment (.env) file, configuring certificates, and adding hosts file entries.

A final manual step is to go to `docker/traefik/config/dynamic/cert_config.yaml` and replace `project` in the certificate names to the HostName

## Build the solution and start Sitecore

Run the following command in PowerShell.

```
docker-compose up -d
```

This will download any required Docker images, build the solution and Sitecore runtime images, and then start the containers. The example uses the *Sitecore Experience Management (XM1)* topology.

Once complete, you can access the instance with the following.

* Sitecore Content Management: https://cm.Your-HostName.localhost
* Sitecore Identity Server: https://id.Your-HostName.localhost
* Basic Company site: https://www.Your-HostName.localhost

## Publish

The serialized items will automatically sync when the instance is started, but you'll need to publish them.

Login to Sitecore at https://cm.Your-HostName.localhost/sitecore and perform a site smart publish. Use "admin" and the password you specified on init ("b" by default).

> you might also need to _Populate Solr Managed Schema_ and rebuild indexes from the Control Panel. You may also need to `docker-compose restart cd` due to workaround an issue with the Solr schema cache on CD.ad

You should now be able to view the Basic Company site at https://www.Your-HostName.localhost.

## Stop Sitecore

When you're done, stop and remove the containers using the following command.

```
docker-compose down
```