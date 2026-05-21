---
title: "Start"
trigger: homeassistant.start
domain: homeassistant
description: "Triggers when Home Assistant starts up."
related_triggers:
  - homeassistant.shutdown
---

The **Start** trigger is useful when you want Home Assistant to do something as soon as it is ready. You can use it to refresh important entities, restore part of your routine after a restart, or send a message that your system is back online.

{% include triggers/ui_header.md %}

To use this trigger in an automation:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation, or select **Create automation** > **Create new automation**.
3. In the **When** section, select **Add trigger**.
4. Select what you want to monitor. Under **By target**, there is nothing to select for this trigger because it applies to the Home Assistant instance itself.
5. Select **Home Assistant**.
6. Under **Event:**, select **Start**.
7. Select **Save**.

### Options in the UI

This trigger has no additional options in the UI.

{% include triggers/yaml_header.md %}

In YAML, use `trigger: homeassistant` with `event: start`. A basic example looks like this:

{% example %}
trigger: |
  trigger: homeassistant
  event: start
{% endexample %}

This runs when Home Assistant starts.

### Options in YAML

{% options_yaml %}
event:
  description: The Home Assistant lifecycle event to watch. For this trigger, use `start`.
  required: true
  type: string
trigger:
  description: The trigger type. For this trigger, use `homeassistant`.
  required: true
  type: string
{% endoptions_yaml %}

## Good to know

- This trigger fires when Home Assistant starts, not when you reload automations or reload other configuration.
- It does not use a target because it applies to the Home Assistant instance itself.
- To run an automation before Home Assistant stops, use [Shutdown](/triggers/homeassistant.shutdown/).

{% include triggers/try_it.md %}

For this trigger, there is no target entity to change. To test it, restart Home Assistant from {% my restart title="**Settings** > **System** > **Restart**" %}.

{% include triggers/more_examples.md %}

### Automation: send a notification when Home Assistant starts

If you restart Home Assistant for an update or maintenance, this automation lets you know when it is ready again. It sends a message to your phone as soon as startup finishes.

- **Trigger**: Start
- **Action**: Send a notification message
  - **Target**: My Device (`notify.my_device`)

{% details "YAML example for notifying when Home Assistant starts" %}

{% example %}
automation: |
  alias: "Notify when Home Assistant starts"
  triggers:
    - trigger: homeassistant
      event: start
  actions:
    - action: notify.send_message
      target:
        entity_id: notify.my_device
      data:
        message: "Home Assistant has started."
{% endexample %}

{% enddetails %}

### Automation: refresh an important entity after Home Assistant starts

If you rely on a sensor that does not update often, you may want to refresh it right after Home Assistant starts. This automation updates one entity as soon as startup finishes so your dashboard gets fresh data sooner.

- **Trigger**: Start
- **Action**: Update entity

{% details "YAML example for refreshing an entity after Home Assistant starts" %}

{% example %}
automation: |
  alias: "Refresh an important entity after startup"
  triggers:
    - trigger: homeassistant
      event: start
  actions:
    - action: homeassistant.update_entity
      target:
        entity_id: sensor.energy_usage
{% endexample %}

{% enddetails %}

{% include triggers/stuck.md %}

{% include triggers/related.md %}
