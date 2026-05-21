---
title: "Shutdown"
trigger: homeassistant.shutdown
domain: homeassistant
description: "Triggers when Home Assistant shuts down."
related_triggers:
  - homeassistant.start
---

The **Shutdown** trigger is useful when you want one last automation run before Home Assistant stops. You can use it for planned maintenance, restarts, or other moments when you want to save state, send a message, or do a final cleanup task.

{% include triggers/ui_header.md %}

To use this trigger in an automation:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation, or select **Create automation** > **Create new automation**.
3. In the **When** section, select **Add trigger**.
4. Select what you want to monitor. Under **By target**, there is nothing to select for this trigger because it applies to the Home Assistant instance itself.
5. Select **Home Assistant**.
6. Under **Event:**, select **Shutdown**.
7. Select **Save**.

### Options in the UI

This trigger has no additional options in the UI.

{% include triggers/yaml_header.md %}

In YAML, use `trigger: homeassistant` with `event: shutdown`. A basic example looks like this:

{% example %}
trigger: |
  trigger: homeassistant
  event: shutdown
{% endexample %}

This runs when Home Assistant begins shutting down.

### Options in YAML

{% options_yaml %}
event:
  description: The Home Assistant lifecycle event to watch. For this trigger, use `shutdown`.
  required: true
  type: string
trigger:
  description: The trigger type. For this trigger, use `homeassistant`.
  required: true
  type: string
{% endoptions_yaml %}

## Good to know

- This trigger fires when Home Assistant shuts down, including when you restart Home Assistant.
- Automations triggered by shutdown have 20 seconds to run before Home Assistant continues shutting down.
- It does not use a target because it applies to the Home Assistant instance itself.
- To run an automation after Home Assistant is ready again, use [Start](/triggers/homeassistant.start/).

{% include triggers/try_it.md %}

For this trigger, there is no target entity to change. To test it, restart Home Assistant from {% my restart title="**Settings** > **System** > **Restart**" %}.

{% include triggers/more_examples.md %}

### Automation: save persistent states before a planned restart

If you are about to restart Home Assistant for maintenance, you can save persistent states right away before shutdown continues. This gives you a simple automation that runs just before Home Assistant stops.

- **Trigger**: Shutdown
- **Action**: Save persistent states

{% details "YAML example for saving persistent states before a planned restart" %}

{% example %}
automation: |
  alias: "Save persistent states before a planned restart"
  triggers:
    - trigger: homeassistant
      event: shutdown
  actions:
    - action: homeassistant.save_persistent_states
{% endexample %}

{% enddetails %}

### Automation: send a notification when Home Assistant is shutting down

If you are working on your system remotely, it can help to know when Home Assistant is shutting down. This automation sends a message to your phone as soon as the shutdown process starts.

- **Trigger**: Shutdown
- **Action**: Send a notification message
  - **Target**: My Device (`notify.my_device`)

{% details "YAML example for notifying when Home Assistant shuts down" %}

{% example %}
automation: |
  alias: "Notify when Home Assistant shuts down"
  triggers:
    - trigger: homeassistant
      event: shutdown
  actions:
    - action: notify.send_message
      target:
        entity_id: notify.my_device
      data:
        message: "Home Assistant is shutting down."
{% endexample %}

{% enddetails %}

{% include triggers/stuck.md %}

{% include triggers/related.md %}
