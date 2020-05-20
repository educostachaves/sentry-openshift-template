## Sentry Openshift

Sentry.io template deployment for Openshift.

This Sentry version used is `9.1.2`. You can check other images in [Sentry DockerHub](https://hub.docker.com/_/sentry/). Changing the version, you might change `configMap` settings inside this project.

### Installing sentry

1. Create a Project in Openshift. Log using `oc login` and then, `oc project <your-project>`.
2. Create an `Redis Ephemeral` from Openshift Catalog Template. Follow the default instructions.
3. Create an `Postgres Persistent` from Openshift Catalog Template. Follow the default instructions. Keep attention in Pesistent Volume creation.
4. `stack.yaml` contains lots of environment specific settings, please edit those first. Everithing is set as default, but you can choose to edit everything.
5. Run `oc create -f stack.yaml`. All settings will be installed in your project.
6. Check if `postgres` is up and running. you can check those `oc get pods`.
7. Run `oc rsh postgres` and then write `psql` and insert command `CREATE EXTENSION IF NOT EXISTS citext;` in the previous created database. In our example you need to change the database to `sentry`, then in `psql` you need to run `\c sentry` and then run the query above. 
8. Edit `populator.yaml` env variables with your own specific settings, then run `oc create -f populator.yaml`
9. That populator will populate the db. You can follow the process doing `oc get pods | grep database-populator` and `oc logs database-populator`. This might take few minutes.
10. After the `database-populator` is Completed, run `oc delete pod database-populator`.
11. try login in `https://yoursentryurl` set in route configuration. You can do a new route as you wish in openshift.
