# Run Optimizely on Mac M1 with .NET6

### Steps

1. Install Docker
2. Create Optimizely project (From nuget templates)
   ```bash
   dotnet new epi-cms-empty
   ```
   Template installation guide:
   https://docs.developers.optimizely.com/content-cloud/v12.0.0-content-cloud/docs/installing-optimizely-net-5
3. Start local MSSQL Server (Specific image to work on M1)
   ```bash
    docker run -e "ACCEPT_EULA=1" -e "MSSQL_SA_PASSWORD=MyPass@word" -e "MSSQL_PID=Developer" -e "MSSQL_USER=SA" -p 1433:1433 -d --name=sql mcr.microsoft.com/azure-sql-edge
   ```
4. Create Database (Use Azure Data Studio etc.)
   ![image](https://user-images.githubusercontent.com/53129079/199503980-69e00942-9a99-4966-a091-2921a9cc837e.png)
5. Update connection string in `appsettings.Development.json` with your values:
   ```bash
   "ConnectionStrings": {
    "EPiServerDB": "Data Source=localhost;Initial Catalog=CustomMssqlDb;Integrated Security=True;Connect Timeout=30;TrustServerCertificate=true;Trusted_Connection=false;User=sa;Password=MyPass@word;"
   },
   ```
6. Run project from Visual Studio or from command line:
   ```bash
   $ dotnet run
   ```
