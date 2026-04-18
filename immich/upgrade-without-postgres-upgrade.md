# Small guide to update Immich database version on TrueNAS when postgres-upgrade isn't available

1. Make sure to be on TrueNAS SCALE 25.04 or later, as it has the ability to convert to custom app which we will use.
2. Stop Immich
3. Make a backup of your datasets: data and pgData. data shouldn't be modified but it's better to be safe. (A snapshot should be enough as you can rollback to it, but a copy of both dataset could also be an option)
4. Create a temporary dataset for the install that will follow. Create it with the generic preset
5. Install a second Immich from the App catalog, reuse the same passwords and point its data dataset to the same dataset as the first instance. For the database dataset point it to the temporary dataset you previously created and check the "apply permissions" checkbox
6. Convert this new Immich to a custom app (Click on the app, then click on the 3 dots next to the edit button, then click on Convert to Custom App)
7. On this custom app click on Edit (This will allow you to edit the yml (the config) of the app) the changes you have to make are the following:
   * Around line 251 edit version of postgres-upgrade from whatever it is to 1.1.11 so it should be like `postgres-upgrade:1.1.11`
   * Around line 155, 231 and 285 replace the path of the temporary db dataset with the path of the actual db dataset. The path will be at multiple places and will look like `/mnt/PATH/TO/YOUR/TEMPORARY_DATASET`, replace it with `/mnt/PATH/TO/YOUR/DB_DATASET` everywhere you see it
8. Save the changes. The app should start to upgrade the database, check the logs of the different containers to see the progress, once the upgrade is done you should have a working Immich with the new version.
9. Once Immich is running, you can remove both Immich instances and reinstall the latest version from the catalog, point it to the same dataset. This way you get the latest version with the "cool" icon and all the benefits (and drawbacks) of the catalog version
