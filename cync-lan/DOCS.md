# CyncLAN Bridge
CyncLAN bridge is a software stack that allows Home Assistant to communicate with Cync (formerly C by GE) 
smart devices over a local network connection, bypassing the need for cloud services.
This app uses MQTT for communication and supports auto-discovery of devices in Home Assistant.

You must use [DNS redirection](https://github.com/baudneo/hass-addons/tree/dev/docs/cync-lan/DNS.md) to forward: 
- `cm-sec.gelighting.com`
- `cm.gelighting.com`
- `cm-ge.xlink.cn`

to your Home Assistant server's local IP address. This will trick Cync devices into connecting to the `nCync` 
(say: _bye, bye, bye_ to Cloud only) TCP socket server running in this app, enabling you to control your devices locally.

>[!NOTE]
> You will still need to use the Cync app to add new devices to your Cync account. 
> Once added and a new config is exported, you can control the newly added devices locally


## First Run Steps
>[!IMPORTANT]
> Before you can manage your devices locally, you must export your Cync device list from the Cync cloud API
> using the apps ingress page.

1. Configure the Cync account username, account password and MQTT broker connection details in the app configuration.
2. Start the app
3. Visit the ingress page of the app in Home Assistant (`Open Web UI` button near the app `UNINSTALL` button OR the lightbulb icon in the sidebar if you enabled `Show in sidebar`)
4. Click the "Start Export" button
5. Follow the prompts, check your Cync account email for the OTP code and enter it into the ingress page form, click 'submit'
6. Wait for the success message indicating that the device list has been exported, the config is displayed in a text box with syntax highlighting and a copy button
7. Restart the app to load the newly exported configuration, it is exported and overwrites any previously existing config file
8. MQTT auto-discovery will create entities in Home Assistant for each device and a 'bridge' device to represent the CyncLAN controller itself
9. As long as DNS redirection is set up correctly and you power cycled your Wi-Fi Cync devices, all supported and discovered devices should now be controllable from Home Assistant (Even BTLE only devices!)

## Migration
To perform a seamless migration from the old monolithic, non app setup:
- SSH into HASS or get to the CLI on the device
- create a folder to hold the config in the correct location: `mkdir -p /homeassistant/.storage/cync-lan/config`
- copy your existing `cync_mesh.yaml` into the new dir: `cp /path/to/cync_mesh.yaml /homeassistant/.storage/cync-lan/config`
- Start the app, it will automatically detect the existing config and use it
- Change your DNS redirection to point to the Home Assistant server's local IP address
- Power cycle the Cync devices, so they perform a DNS request and get the new IP address of the CyncLAN bridge

## Exporting Device Configuration
Visit the CyncLAN 'ingress' webpage (from the sidebar, or from the app page `Open Web UI` button). You will be 
greeted with a simple form that has provisions for being sent an OTP and to enter and submit the OTP.

- The `Start Export` button will check for a cached access token and use it to export your device config, removing the need for an OTP email to be sent
  - If no cached access token is found, the button will initiate the export process by sending your Cync account credentials to the Cync cloud API and requesting an OTP to be sent to your account email address
- The `Submit OTP` button will evaluate the OTP textbox and send the OTP to the backend export server
    - You can also request an OTP from the Cync app and then use the Submit OTP button, bypassing the `Start Export` button.
    - After submitting an OTP, the backend will use the OTP and Cync account creds to generate a new access token
    - The access token and metadata are stored on disk in a cache for future operations (Cync sets a 24 hr expiration on new access tokens)
    - Cync cloud API supplies a refresh token, but I need to figure out the endpoint and data structure to use it for renewing access tokens
- After a successful export, the `cync_mesh.yaml` contents will be displayed in a text-box with syntax highlighting (via PRISM) with a `Copy` button. Also, a `Download Config File` button will be available to allow downloading the newly exported config
  - The config file is saved to disk and the app will use it after a restart


## Tips / Troubleshooting
See the [tips documentation](https://github.com/baudneo/cync-lan/tree/python/docs/tips.md) for tips on how to have a better experience with the app.

See the [troubleshooting documentation](https://github.com/baudneo/hass-addons/tree/dev/docs/cync-lan/troubleshooting.md) for common issues and how to resolve them.