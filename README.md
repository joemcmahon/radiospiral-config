# radiospiral-config
Configuration repository for the RadioSpiral streaming radio app

This repository contains the web-based config files for the RadioSpiral
streaming radio application, for RadioSpiral, based at https://radiospiral.net.

The config files here are
 - `config.json`: defines the configurations that the app should use to derive its configuration.
   Subsumes the deprecated stations.json and adds a new Azuracast-specific config section that
   the app can use to probe an Azuracast server for active streams, plus a list of streams that
   can be active but should be excluded from the app.
 - `stations.json`: (**deprecated**) a file defining the station's main streaming URL, name and short
   description. Should RadioSpiral host other streams at a later date, this file
   could be updated to push out the new streams in real time.
 - `credits.json`: defines the station staff's roles and names. Used in the iOS app's credits
   screen.

