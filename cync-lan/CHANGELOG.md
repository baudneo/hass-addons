### 0.0.3b10
- Host tailwinds, animate.css and prism.js locally

### 0.0.3b9
- add `raw_debug` options to config. When enabled, will output binary data to/from TCP devices in the logs (VERY verbose)
- fix device type 107: switch to light

### 0.0.3b8
- typo fix: `model-info` to `model_info`: GH mobile editor strikes again

### 0.0.3b7
- add device types 47, 71 and 107

### 0.0.3b6
- fix `functools.partial` error

### 0.0.3b5
- split switch and plug logic

### 0.0.3b4
- fix brightness scale (was 0-255, we want 0-100)
- add device types 39, 57
  - 39: direct connect on/off paddle switch - neutral required
  - 57: direct connect on/off paddle switch - NO neutral required (same model # with a NN appended for No Neutral, good to know)
- Updated Github notifications so I actually receive notifications of issues/PR's, etc.
  - Sorry, I've been a bit stunned lately. Hopefully this is the last beta release before 0.0.3

### 0.0.3b3
- Fix new logic in 'supported_color_modes'
- Add device types: 17, 18 
  - 2700K dimmable bulbs [CLED199L2]
  - 18 may be the same bulb with a newer SoC

### 0.0.3b2
- Nothing of note

### 0.0.3b1
- Add default 'supported_color_modes' of 'brightness' in device / entity registration messages to stop throwing deprecation warnings (2025.3)
- Changed 'object_id' to 'default_entity_id' in device / entity registration messages ('object_id' retained for older versions)
- Add random delay (5-15s) after hass birth message before re-announcing device config and state.
  - Fixes no devices after a hass restart
- bumped min ver to get the fix out

### 0.0.2b2
- Add restart button to export: unhidden after receiving success from submitting OTP button
- Fix cached token reading: attempted to read a binary file in text mode
- Fix device closing logic: expected exception is now `pass`ed, proper input to asyncio.wait()
- Optimizations

### 0.0.2b1
- Add "state" key and value when updating brightness, temp or RGB. Even though hass docs say it is optional, HASS logs shows exceptions when this is omitted due to using direct access to a dict key: variable_dict["key"] instead of checking for key existence or using .get().

### 0.0.2a1
- Rough in fan support (WIP)
- optimizations

## 0.0.1
- Initial release