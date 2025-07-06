# InfluxDB3 custom component for ESPHome

Using this component, ESPHome-based devices can submit their sensor readings directly to InfluxDB. This removes unnecessary steps and points of failure.

This repository is based on a fork of previous work, for which much credit is due. 

Changes from CalvinSchwartz/esphome-influxdb:
- Made usable as an External Component. Benefit: installation is handled by ESPHome, without manual steps
- Documentation updates to reflect earlier changes

Changes from Jepsson/esphome-influxdb to CalvinSchwartz/esphome-influxdb2
- Changed to InfluxDB2 API
 
Changes from Jepsson/esphome-influxdb2 to dark544/esphome-influxdb3
- Changed to InfluxDB3 API

## Installation and Usage
Just add this repository as an External Component in your ESPHome yaml. Also, add a `influxdb3` section to your ESPHome configuration file to configure the client.

### Example configuration

```yaml
external_components:
  - source: github://dark544/esphome-influxdb3
    components: [ influxdb3 ]

influxdb3:
  host: "influxdb-host"
  orgid: "influx_org"
  token: "Token abcdef=="
  bucket: "influx_bucket"
  device: "tempsensor"
  sensors:
    meter_id:
      ignore: True
    ams_temperature:
      measurement: 'temperature'
      tags: 
        room: kitchen
```

### Configuration variables

* **host** (Required, string): Hostname or IP for the InfluxDB server
* **port** (Optional, int, default: 8086): Port number the InfluxDB server is listening on.
* **orgid** (Required, string): Organization ID when connecting to influxdb.
* **token** (Required, string): Token used when connecting to influxdb. Format: "Token abcdef=="
* **bucket** (Required, string): Name of influxdb bucket.
* **device** (Required, string): Name for this device.
* **send_timeout** (Optional, time, default: "500ms"): Time to wait before sending UDP packets which have not been filled to max size.
* **publish_all** (Optional, boolean, default: True): If true, publish updates from all sensors unless explicitly ignored in per sensor configuration. If false, only publish updates from sensors explicitly configured.
* **tags** (Optional, mapping, default 'node: <esphome.name>'): Mapping of tag keys and values. 
* **sensors** (Optional, mapping, default: {}): Per sensor configuration. Keys are sensor IDs. All types of sensors are included in this mapping, there is no distinction between float, binary and text sensors.

#### Sensor configuration variables

* **ignore** (Optional, boolean, default: False): Whether or not to include updates for this sensor.
* **measurement** (Optional, string): Name of measurements with update from this sensor. Defaults to the sanitized name of the sensor.
* **tags** (Optional, mapping, default: {}): Additional tags added for this sensor.

### TO DO 

* Add a readings cache to retry when calls fail, so that the system is robust against either InfluxDB or the network being down for a while.
