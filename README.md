[![ruff](https://github.com/LeskoIam/kronoterm_cloud_relay/actions/workflows/ruff.yml/badge.svg?branch=master)](https://github.com/LeskoIam/kronoterm_cloud_relay/actions/workflows/ruff.yml)
# Kronoterm cloud relay

Relay server for [Kronoterm](https://kronoterm.com//) cloud. It gets data from cloud and exposes it through REST API to local network. 

## Install and run relay
### Docker
On your host system that has [Docker](https://www.docker.com/) installed create `docker-compose.yml` 
file with following content. 
> Update file with your username and password!
```yaml
services:
  kronoterm_cloud_relay:
    image: leskoiam/kronoterm_cloud_relay:latest
    container_name: kronoterm_cloud_relay
    restart: unless-stopped
    ports:
      - "8555:8555"  # Adjust the port mappings as needed
    environment:
      # Add your kronoterm cloud username and password
      - KRONOTERM_CLOUD_USER=your-user
      - KRONOTERM_CLOUD_PASSWORD=your-password
```
Spin up the image
```shell
docker compose up -d
```

## Try it out
With your favorite browser navigate to 

`ip-or-host-address:8555/version`

return should be the current version of kronoterm-cloud-relay
```python
{"version": "0.0.8"}
```
or

`ip-or-host-address:8555/relay/echo/is_it_working`

return should be whatever the last argument is, in this case `is_it_working`
```python
{"message": "relay - echo 'is_it_working' - OK"}
```

#### Home Assistant uses this one to avoid multiple API calls
- `http://ip-or-host-address:8555/hp_info/info_summary`

### Some currently supported endpoints
#### Get data from heat pump (cloud)
Method: `GET`
- `http://ip-or-host-address:8555/hp_info/initial_data`
- `http://ip-or-host-address:8555/hp_info/basic_data`
- `http://ip-or-host-address:8555/hp_info/system_review`
- `http://ip-or-host-address:8555/hp_info/heating_loop/<heating_loop_number>`
  - `<heating_loop_number>`
    - heating loop 1: `1`
    - heating loop 2: `2`
    - sanitary water (heating loop 5): `5`
- `http://ip-or-host-address:8555/hp_info/alarms`
#### Set heat pump configuration
Method: `POST`
- http://ip-or-host-address:8555/hp_control/set_target_temperature
  - payload = `{"temperature": "24.5", "heating_loop": 1}`
  - payload = `{"temperature": "24.5", "heating_loop": 2}`
  - payload = `{"temperature": "24.5", "heating_loop": 5}`
- http://ip-or-host-address:8555/hp_control/set_heating_loop_mode
  - payload = `{"mode": "AUTO", "heating_loop" : 1}`
  - payload = `{"mode": "ON", "heating_loop" : 1}`
  - payload = `{"mode": "OFF", "heating_loop" : 1}`


## Usage with [Home Assistant](https://www.home-assistant.io/)
Home Assistant has [REST](https://www.home-assistant.io/integrations/rest) integration which can request and post data from `relay`.

Refer to [Home Assistant Readme](./docs/home_assistant.md) for details.

## Debugging
Tail container logs:
```shell
docker logs -f kronoterm_cloud_relay
```
Shut down container:
```shell
docker compose down kronoterm_cloud_relay
```

