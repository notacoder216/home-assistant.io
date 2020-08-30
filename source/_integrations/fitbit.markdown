---
title: Fitbit
description: Instructions on how to integrate Fitbit devices within Home Assistant.
ha_category:
  - Health
ha_iot_class: Cloud Polling
ha_release: 0.19
ha_domain: fitbit
---

The Fitbit sensor allows you to expose data from [Fitbit](https://fitbit.com/) to Home Assistant.

Enable the sensor by adding the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: fitbit
    clock_format: 12H
    monitored_resources:
      - "body/weight"
```

Restart Home Assistant once this is complete. Go to the frontend. You will see a new entry in Notifications for configuring Fitbit. Click on the Notification and follow the instructions there to complete the setup process.

The steps involved in the instructons in the front end are:
1. The notificaion will have an image with an example and will ask that you go to https://dev.fitbit.com/apps/new and create a new app with the URL of your HomeAssistant appended with /api/fitbit/callback. (examples: https://your.ha.ip.address:8123/api/fitbit/callback or https://ha.duckdns.org/api/fitbit/callback) This step can be done at anytime. Once that is completed there will be a OAuth 2.0 Client ID and a Client Secret.
2. Go to your HomeAssistant default directory and open the fitbit.conf file and change the client_id and the client_secret obtained from step 1 and save that file.
3. Go back to the Notification and click below the example image in order to acknowledge that the Client ID and a Client Secret were saved in the fitbit.conf file.
4. The Notification will now populate with a link to authorize fitbit by clicking on the provided link. In some cases clicking on the link won't navigate to the authorization page and instead it needs to be copied and pasted in a new tab or window.
5. Check all of the boxes and select Allow then download the callback file. Once that is done the Notification clears from the Frontend and the Fitbit sensors are now available to be used.

Please be aware that Fitbit has very low rate limits, 150 per user per hour. The clock resets at the _top_ of the hour (meaning it is not a rolling 60 minutes). There is no way around the limits. Due to the rate limits, the sensor only updates every 30 minutes. You can manually trigger an update by restarting Home Assistant. Keep in mind that 1 request is used for every entry in `monitored_resources`.

The unit system that the sensor will use is based on the country you set in your Fitbit profile.

{% configuration %}
monitored_resources:
  description: Resource to monitor.
  required: false
  default: "`activities/steps`"
  type: list
clock_format:
  description: Format to use for `sleep/startTime` resource. Accepts `12H` or `24H`.
  required: false
  default: "`24H`"
  type: string
unit_system:
  description: Unit system to use for measurements. Accepts `default`, `metric`, `en_US` or `en_GB`.
  required: false
  default: "`default`"
  type: string
{% endconfiguration %}

Below is the list of resources that you can add to `monitored_resources`. One sensor is exposed for every resource.

```text
activities/activityCalories
activities/calories
activities/caloriesBMR
activities/distance
activities/elevation
activities/floors
activities/heart
activities/minutesFairlyActive
activities/minutesLightlyActive
activities/minutesSedentary
activities/minutesVeryActive
activities/steps
activities/tracker/activityCalories
activities/tracker/calories
activities/tracker/distance
activities/tracker/elevation
activities/tracker/floors
activities/tracker/minutesFairlyActive
activities/tracker/minutesLightlyActive
activities/tracker/minutesSedentary
activities/tracker/minutesVeryActive
activities/tracker/steps
body/bmi
body/fat
body/weight
devices/battery
sleep/awakeningsCount
sleep/efficiency
sleep/minutesAfterWakeup
sleep/minutesAsleep
sleep/minutesAwake
sleep/minutesToFallAsleep
sleep/startTime
sleep/timeInBed
```
