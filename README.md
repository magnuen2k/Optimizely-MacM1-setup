# Run Optimizely on Mac M1 with .NET6

## Steps

1. Install Docker
2. Create Optimizely project (From nuget templates)
   ```bash
   dotnet new epi-cms-empty
   ```
   Template installation guide:
   https://docs.developers.optimizely.com/content-cloud/v12.0.0-content-cloud/docs/installing-optimizely-net-5
   
   
3. Start local MSSQL Server and create DB (Specific image to work on M1)
   ### With docker compose

   `docker-compose.yml`
   ```yaml
   services:
     epi-cms-db:
       image: mcr.microsoft.com/azure-sql-edge:latest
       environment:
         - ACCEPT_EULA=Y
         - MSSQL_SA_PASSWORD=MyPass@word
         - MSSQL_PACKAGE=/backup
       ports:
         - 1434:1433
       volumes:
         - epi-cms-storage:/var/opt/mssql/
         - ./backup:/backup:ro
      ```
   
      - Run docker compose
      ```bash
      docker-compose up | down | rm
      ```
   
      - You can also populate your DB at first run by adding a DACPAC or BACPAC in the `/backup` folder.
   
      ### Or spin up container and create db manually
      ```bash
       docker run -e "ACCEPT_EULA=1" -e "MSSQL_SA_PASSWORD=MyPass@word" -e "MSSQL_PID=Developer" -e "MSSQL_USER=SA" -p 1433:1433 -d --name=sql mcr.microsoft.com/azure-sql-edge
      ```
      - Create Database (Use Azure Data Studio etc.)
         ![image](https://user-images.githubusercontent.com/53129079/199503980-69e00942-9a99-4966-a091-2921a9cc837e.png)


4. Update connection string in `appsettings.Development.json` with your values:

   ```bash
   "ConnectionStrings": {
    "EPiServerDB": "Data Source=localhost;Initial Catalog=CustomMssqlDb;Integrated Security=True;Connect Timeout=30;TrustServerCertificate=true;Trusted_Connection=false;User=sa;Password=MyPass@word;"
   },
   ```
5. Run project from Visual Studio or from command line:
   ```bash
   $ dotnet run
   ```
