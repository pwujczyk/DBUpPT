<!--Category:Powershell--> 
 <p align="right">
    <a href="https://www.powershellgallery.com/packages/ProductivityTools.ConvertTcx2Gpx/"><img src="Images/Header/Powershell_border_40px.png" /></a>
    <a href="http://productivitytools.tech/convert-tcx-to-gpx/"><img src="Images/Header/ProductivityTools_green_40px_2.png" /><a> 
    <a href="https://github.com/pwujczyk/ProductivityTools.ConvertTcx2Gpx"><img src="Images/Header/Github_border_40px.png" /></a>
</p>
<p align="center">
    <a href="http://productivitytools.tech/">
        <img src="Images/Header/LogoTitle_green_500px.png" />
    </a>
</p>


# DbUp 

DbUp is a package which manages database updates. ProductivityTools DbUp package adds additional functionality to base DbUp like creating the database and creating log table.
<!--more-->

I like to use it as in the DbUp I need to provide whole connection string and here only server name and database name. 

To use it please reference package with the package manager console

`Install-Package ProductivityTools.DbUp`

Next create Console application and in the main add following code:

```c#
string serverName = "ServerName";
string dbName = "DatabaseName";
DBUpHelper.DBUp dBUp = new DBUpHelper.DBUp("gc");
Assembly assembly = Assembly.GetExecutingAssembly();
dBUp.PerformUpdate(serverName, dbName, assembly, false)
```
 
Next add Scripts Folder and create inside SQL scripts which you want to execute. In the properties of the script please select **Embeded resource**



When application will start, DBUpHelper will:

  * Check if database "DatabaseName" exists if not it will create it
  * In the database it will create schema provided in the constructor "gc"
  * It will start executing the scripts from the Script folder.


## Original DbUp code

Those four lines above can are shorten version of the code from DbUp page

```C#
            var connectionString = "Server=.\\SQL2019;Database=PTTeamManagment;Trusted_connection=true";
            EnsureDatabase.For.SqlDatabase(connectionString);
            var upgrader =
                DeployChanges.To
                    .SqlDatabase(connectionString)
                    .WithScriptsEmbeddedInAssembly(Assembly.GetExecutingAssembly())
                    .LogToConsole()
                    .Build();

            var result = upgrader.PerformUpgrade();

            if (!result.Successful)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine(result.Error);
                Console.ResetColor();
#if DEBUG
                Console.ReadLine();
#endif
            }

            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Success!");
            Console.ResetColor();

```