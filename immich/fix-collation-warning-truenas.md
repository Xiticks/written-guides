# How to fix Collation Error on TrueNAS

## Make a backup of the database (better safe than sorry)
1) Go to the [Immich jobs page](https://my.immich.app/admin/queues)
2) Click on *+ Create job* (upper right menu)
3) Select *Create Database dump* then hit create

## Refresh collation version
1) Go into the shell of the pgVecto (TrueNAS Apps page > Click on Immich > In the workloads, click on the first button of the pgvecto container)
2) You have to connect to each database and refresh them:
    1) Connect as immich user to the immich database: ```psql -U immich```
    2) Run the following 2 commands (wait for the first one to finish before running the second one):
        - ```ALTER DATABASE immich REFRESH COLLATION VERSION;```
        - ```REINDEX DATABASE immich;```
    3) change the open database to template1: ```\c template1```
    4) Run the same 2 commands for template1:
        - ```ALTER DATABASE template1 REFRESH COLLATION VERSION;```
        - ```REINDEX DATABASE template1;```
    5) change the open database to postgres: ```\c postgres```
    6) Run the same 2 commands for postgres:
        - ```ALTER DATABASE postgres REFRESH COLLATION VERSION;```
        - ```REINDEX DATABASE postgres;```

Once you have done this, you can restart the Immich application and the error should be gone.
