---
title: Solyx Energy Local
description: Instructions on how to monitor your Solyx Energy Nymo locally, over your home network, in Home Assistant.
ha_release: 2026.12
ha_categories:
  - energy.meters_monitoring
  - energy.solar_batteries
ha_platforms:
  - sensor
ha_iot_class: Local Polling
ha_quality_scale: bronze
ha_config_flow: true
ha_codeowners:
  - '@MartinaeyNL'
  - '@pankalog'
ha_domain: solyx_energy_local
ha_integration_type: device
ha_zeroconf: true
related:
  - url: https://www.solyxenergy.nl
    title: Solyx Energy website
  - url: https://www.solyxenergy.nl/faq/
    title: Frequently asked questions
  - url: https://www.solyxenergy.nl/contact/
    title: Ask a question or report device issues
  - url: https://github.com/home-assistant/core/issues?q=label%3A%22integration%3A+solyx_energy_local%22
    title: Issues list for the Solyx Energy Local integration
---

The **Solyx Energy Local** {% term integration %} is used to integrate with Nymo devices from [Solyx Energy](https://www.solyxenergy.nl) directly over your home network. The Nymo controls an electric boiler with exactly the amount of solar energy that would otherwise be fed back into the grid. It allows households to store their surplus solar power and minimize feed-in, without huge investments.

This integration talks to the Sensorbox of the Nymo on your home network, so it works without the Solyx Energy cloud and without an internet connection.

{% note %}
To use the Solyx Energy cloud instead, see the [Solyx Energy Cloud](/integrations/solyx_energy_cloud) integration. Use one of the two for a Nymo, not both.
{% endnote %}

## Prerequisites

- A Nymo that is set up in the **Solyx Energy** app, and whose Sensorbox runs firmware with local control. The Sensorbox updates its firmware automatically.
- Home Assistant and the Nymo on the same home network.

To get the connection details:

1. In the Solyx Energy app, select your Nymo and press **Modify device settings**.
2. Press **Local control**, and then **Show connection details**.
3. Copy the **Access token** and the **Certificate fingerprint**, and use them when adding the integration in Home Assistant.

{% include integrations/config_flow.md %}

{% configuration_basic %}
Host:
    description: "The hostname or IP address of the Nymo, for example `solyx-a1b2c3.local`. Only asked when you add the integration by hand; a discovered Nymo is found automatically."
Access token:
    description: "The access token of your Nymo. You can find it in the Solyx Energy app under **Modify device settings** > **Local control** > **Show connection details**."
Certificate fingerprint:
    description: "The SHA-256 fingerprint of the certificate of your Nymo, shown next to the access token in the Solyx Energy app. Home Assistant only talks to a Nymo that presents this certificate."
{% endconfiguration_basic %}

## Automatic discovery

If your Nymo is on the same network as Home Assistant, it is discovered automatically via mDNS/Zeroconf. You then only need to enter the access token and certificate fingerprint. When the Nymo gets a new IP address, Home Assistant updates it automatically.

## Supported functionality

The **Solyx Energy Local** integration provides the same sensors as the [Solyx Energy Cloud](/integrations/solyx_energy_cloud) integration.

### Sensors

- **Boiler current**
  - **Description**: Measures the electrical current drawn by the boiler in amperes (A).

- **Boiler power**
  - **Description**: Measures the boiler's electrical power consumption in watts (W).

- **Boiler voltage**
  - **Description**: Measures the electrical voltage supplied to the boiler in volts (V).

- **Days since maximum temperature**
  - **Description**: Shows how many days have passed since the boiler last reached its maximum temperature.

- **Grid power**
  - **Description**: Measures the power exchanged with the electrical grid in watts (W). This can be a positive or negative value.

- **Days since last legionella prevention**
  - **Description**: Number of days since the boiler last ran its legionella prevention cycle, in days (d).

- **Saved today**
  - **Description**: Shows the amount of energy saved during the current day, in watt-hours (Wh).

- **Saved this week**
  - **Description**: Shows the amount of energy saved during the current week, in kilowatt-hours (kWh).

- **Saved this month**
  - **Description**: Shows the amount of energy saved during the current month, in kilowatt-hours (kWh).

## Data updates

The **Solyx Energy Local** integration polls the Nymo every 30 seconds over your home network, as often as the Sensorbox reads the Nymo itself. No internet connection is needed.

## Known limitations

- The integration can only read the Nymo. Changing its operating mode is not yet supported.
- When the Nymo is set up again in the Solyx Energy app, it gets a new access token and certificate. Remove the integration and add it again with the new connection details.

## Troubleshooting

### Can’t set up the device

#### Invalid authentication

Verify that you have copied the access token from the Solyx Energy app correctly.

#### The device presented a different certificate

The certificate fingerprint does not match the Nymo. Copy the certificate fingerprint from the Solyx Energy app again. If the Nymo was set up again in the app, its certificate changed.

#### Failed to connect

Check that the Nymo is powered and connected to your home network, and that Home Assistant can reach it. If you added it by hand, check the hostname or IP address; `solyx-a1b2c3.local` names only work when your network supports mDNS.

### The Nymo is not discovered

Discovery uses mDNS. If Home Assistant runs on a different network or VLAN than the Nymo, or your network blocks mDNS, add the integration by hand with the IP address of the Nymo.

### Entities are unavailable

The Nymo could not be reached. Check that it is powered and connected to your home network. The entities become available again as soon as the Nymo responds.

## Removing the integration

This integration follows standard integration removal.

{% include integrations/remove_device_service.md %}
