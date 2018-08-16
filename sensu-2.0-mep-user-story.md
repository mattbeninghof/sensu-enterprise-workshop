

1. Deploy a fresh installation of sensu-backend:

   ```
   $ docker-compose up -d
   ```

   You should now be able to login to the Sensu Dashboard by visiting
   [http://localhost:3000](http://localhost:3000).

   _NOTE: this `docker-compose.yml` installs Sensu along with some other tools
   we'll use in this guide, including [InfluxDB][influxdb] and
   [Grafana][grafana]._

   [influxdb]: https://influxdb.com
   [grafana]:  https://grafana.com

2. Install & configure the Sensu CLI.

   ```
   $ curl -LO https://storage.googleapis.com/sensu-binaries/$(curl -s https://storage.googleapis.com/sensu-binaries/latest.txt)/darwin/amd64/sensuctl
   $ chmod +x sensuctl
   $ mv sensuctl /usr/local/bin/sensuctl
   $ sensuctl configure

   ? Sensu Backend URL: (http://localhost:8080)
   ? Username: admin
   ? Password: *********
   ? Organization: default
   ? Environment: default
   ? Preferred output format: none
   ```

   Test your configuration by querying the Sensu API using `sensuctl`:

   ```
   $ sensuctl entity list
   sensuctl entity list
     ID   Class   OS   Subscriptions   Last Seen
    ──── ─────── ──── ─────────────── ───────────
   ```

3. Create a Sensu organization and environment.

   ```
   $ sensuctl organization create --interactive
   ? Name: workstation
   ? Description: Local workstation organization.
   Created

   $ sensuctl environment create --interactive
   ? Name: sandbox
   ? Organization: workstation
   ? Description: Local workstation sandbox environment.
   Created

   $ sensuctl config set-organization workstation
   $ sensuctl config set-environment sandbox
   ```

   _A NOTE ABOUT MULTI-TENANCY IN SENSU: true multi-tenancy and RBAC are new
   concepts in Sensu 2.0 (these were not supported in Sensu 1.0). As a result,
   **every resource** in Sensu 2.0 must be associated with an "organization" and
   "environment". By default, a fresh Sensu 2.0 install has one organization
   called "default", and that organization contains one environment called
   "default". As we proceed through this guide, we will need to associate every
   resource we are creating (e.g. handlers, events, entities, etc) with an
   organization and environment._

4. Install and configure your first Sensu Handler.

   ```
   $ sensuctl asset create sensu-influxdb-handler --organization workstation --environment sandbox --url http://bonsai/assets/sensu-influxdb-handler.tar.gz --sha512 abc123
   $ sensuctl handler create influxdb --organization workstation --environment sandbox --url
   ```

   _A NOTE ABOUT CONFIGURATION CHANGES: the configuration model in Sensu 2.0 is
   a brand new concept. In Sensu 1.0 configuration changes were applied by  
   modifying configuration files on disk and restarting Sensu services; this is
   no longer required in Sensu 2.0 &ndash; simply modify configuration changes
   via `sensuctl` (or the dashboard; coming soon) and they are applied in
   realtime._

5. Publish some events to the Sensu API.

6. Publish some discovery events to the Sensu API.

7. Publish some telemetry events to the Sensu API.

8. Install and configure a Sensu Agent.

9. Publish some events to the Sensu Agent socket.

10. Install a monitoring plugin and configure a check.

11. Done!
