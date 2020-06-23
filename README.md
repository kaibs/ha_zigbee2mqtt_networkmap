## fork of [ha_zigbee2mqtt_networkmap](https://github.com/rgruebel/ha_zigbee2mqtt_networkmap)

modified version of the Custom Component for Homeassistant from [rgruebel](https://github.com/rgruebel) to work togheter with NGINX SSL proxy and Dnsmasq.

### **changes:**

While HA is booting the file _settings.js_ is created in the directory _config/www/community/zigbee2mqtt_networkmap_ .
By default this is using the local ip-adress of HA, which prevents updating of the map when internal dns re-routing is done. Therefore I modified the initialisation of _settings.js_ in ```_init_.py```:

        
        f = open(hass.config.path('www', 'community', 'zigbee2mqtt_networkmap', 'settings.js'), "w")
        f.write("\n")
        f.write("var webhook_trigger_update_url = '"+adress+"/api/webhook/"+webhook_trigger_update_id+"';")
        f.write("\n")
        f.write("var webhook_check_update_url = '"+adress+"/api/webhook/"+webhook_check_update_id+"';")
        f.close()

Where ```xxx.duckdns.org``` is your duckdns-domain set in the _configuration.yaml_.

**configuration.yaml:**


        webhook:
        
        zigbee2mqtt_networkmap:
          #topic: (optional, default zigbee2mqtt)
          url: 'https://xxx.duckdns.org'
        panel_iframe:
          networkmap:
            title: 'Zigbee Map'
            url: '/local/community/zigbee2mqtt_networkmap/map.html'
            icon: 'mdi:graphql'
    
  You can set the graphviz engine via URL Parameter: 
  map.html?engine=circo (Default: circo, [Supported Engines](https://github.com/mdaines/viz.js/wikiSupported-Graphviz-Features))  
    
**Important:** you might have to clear the browsercache after updates.
