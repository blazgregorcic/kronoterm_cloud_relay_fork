Assuming you have kronoterm-cloud-relay running and reachable
following needs to be added to your home assistant `configuration.yaml` file.

```yaml
sensor:
  - platform: rest
    name: "Heat pump room temperature"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/room_temperature
    method: GET
    value_template: "{{ value_json.data }}"
    unit_of_measurement: '°C'
    device_class: temperature
    state_class: measurement
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump outlet temperature"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/outlet_temperature
    method: GET
    value_template: "{{ value_json.data }}"
    unit_of_measurement: '°C'
    device_class: temperature
    state_class: measurement
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump set room temperature"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/set_temperature
    method: GET
    value_template: "{{ value_json.data }}"
    unit_of_measurement: '°C'
    device_class: temperature
    state_class: measurement
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump outside temperature"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/outside_temperature
    method: GET
    value_template: "{{ value_json.data }}"
    unit_of_measurement: '°C'
    device_class: temperature
    state_class: measurement
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Sanitary water temperature"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/water_temperature
    method: GET
    value_template: "{{ value_json.data }}"
    unit_of_measurement: '°C'
    device_class: temperature
    state_class: measurement
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump working function"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/working_function
    method: GET
    value_template: "{{ value_json.data }}"
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump working status"
    unique_id: generate-your-own-unique-id
    resource: http://your-relay-host:8555/hp-info/working_status
    method: GET
    value_template: "{{ value_json.data }}"
    timeout: 20
    scan_interval: 60
    
  - platform: rest
    name: "Heat pump working mode"
    unique_id: generate-your-own-unique-id
    resource: your-relay-host:8555/hp-info/working_mode
    method: GET
    value_template: "{{ value_json.data }}"
    timeout: 20
    scan_interval: 60

rest_command:
  hp_set_mode_on:
    url: http://your-relay-host:8555/hp-control/set_heating_loop_mode
    method: POST
    payload: '{"mode": "ON"}'
    content_type: 'application/json'
  hp_set_mode_off:
    url: http://your-relay-host:8555/hp-control/set_heating_loop_mode
    method: POST
    payload: '{"mode": "OFF"}'
    content_type: 'application/json'
  hp_set_mode_auto:
    url: http://your-relay-host:8555/hp-control/set_heating_loop_mode
    method: POST
    payload: '{"mode": "AUTO"}'
    content_type: 'application/json'
  hp_set_temperature:
    url: http://your-relay-host:8555/hp-control/set_temperature
    method: POST
    payload: '{"temperature": "{{ set_temp }}" }'
    content_type: 'application/json'
```