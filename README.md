# docker-compose
Docker compose files that I am using privately. This can be seen a bit in the same way as [linuxserver.io project](https://www.linuxserver.io/)

The main purpuse is to have it up for myself, but hey feel free to use them :) Also if you have anything to add, please send me a ping or create pull request so that it can be added as well. 

# Contributers
Many thanks to the following contributers:

| Name | Profile |
| -- | -- |
| Thomas Mater |  https://github.com/thomasmaters |

# Layout
All the compose files should have the same layout so that it's easier to read.

Preferably everything that can be added to the environment file without maing the compose file less readble should be added there. If anything comes from the environment file a [fallback/error message needs to be specified](https://docs.docker.com/reference/dockerfile/#environment-replacement).

It's also advisable to try to have the containers running under your own user instead iof running under the `root` user domain. Mor info about this can be found under [here](https://github.com/TimoTielens/docker-compose/documentation/puid_and_pgid.md).

The order of the options should be in a default order as well. The order should be:

    Services
        Name of the container
            Image name and tag
            Hostname
            Name of the container //https://docs.docker.com/compose/compose-file/compose-file-v3/#container_name
            Restart policiy //https://docs.docker.com/compose/compose-file/compose-file-v3/#restart
            Environment variables //https://docs.docker.com/compose/compose-file/compose-file-v3/#environment
            Port definition //https://docs.docker.com/compose/compose-file/compose-file-v3/#ports
            Volume mapping //https://docs.docker.com/compose/compose-file/compose-file-v3/#volumes
            Network mapping//https://docs.docker.com/compose/compose-file/compose-file-v3/#networks

An example of this can be found below:

    services:
	    sqlserver:
		    image: mcr.microsoft.com/mssql/server:${MSSQL_IMAGE_VERSION:? database image version required} //Name of the container with 
		    hostname: ${MSSQL_NAME:? database name required}
		    container_name: ${MSSQL_NAME:? database name required}
		    restart: unless-stopped
		    environment:
				- PUID=${MSSQL_PUID:? database PUID is required}
      			- PGID=${MSSQL_PGID:? database PUID is required}
			    - TZ=${TIMEZONE:? timezone required}
			    - ACCEPT_EULA=Y
			    - SA_PASSWORD=${MSSQL_SA_PASSWORD:? database master password required}
			    - MSSQL_COLLATION=${MSSQL_COLLATION:? database collation required}
			ports:
				- 1433:1433
			volumes:
				- ./mssql/data:/var/opt/mssql:rw
			networks:
				- mssql