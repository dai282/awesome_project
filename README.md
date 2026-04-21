# Schema Data Migration (sdm) Xtracta Technical Assessment

This project demonstrates database schema and data migrations using the [sdm](https://github.com/Beim/schema-data-migration) tool on a local MySQL environment.

## Environment Details
* MySQL: v9.6.0
* sdm: v0.6.4
* Python: v3.12.10
* Skeema: v1.13.2-community

## How sdm works
sdm helps manage database migrations by creating migration plans that tracks schema and data states of a database in JSON format. It uses a declarative approach, you define what a database should look like, and sdm figures out how to get there.

* Schema migration is done by:
    * Editing the `.sql` file inside the `schema/` folder
    * Creating a migration plan with `sdm make-schema "plan_name"`
    * Executing the migration plan with `sdm migrate <environment>`
* Data migration is done by:
    * Creating a migration plan with `sdm make-data "plan_name"`
    * Editing the migration plan JSON file with the forward and backward scripts
    * Executing the migration plan with `sdm migrate <environment>`
* When migrations are executed, the migration history and change logs are stored in `_migration_history` and `_migration_history_log` tables inside each database. The `sdm info` command reads from the `_migration_history` table.

## Challenge encountered
One key challenge was setting up the local development environment to install and run sdm. After installing sdm dependencies (skeema, Python, pip) and attempting to clone and install sdm, I ran into several errors:
 
* The first error was that mysqlclient couldn't find MySQL headers (`MYSQLCLIENT_CFLAGS` and `MYSQLCLIENT_LDFLAGS`) in my system. This was because I had installed MySQL client with Homebrew on my MacBook. I fixed this by exporting the flags and specifying the full path where MySQL client was installed (`/opt/homebrew/...`).
* The next error was that the linker could not find the `zstd` and `openssl` libraries. I fixed this by updating the flags to include both libraries.
* After sdm was installed, running `sdm --version` threw an `AssertionError` from SQLAlchemy. This was due to a version incompatibility with Python 3.14, which was installed on my machine. I fixed this by installing Python 3.12 and running sdm with that version instead.