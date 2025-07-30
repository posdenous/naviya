---
trigger: always_on
---

id: unread_tile
description: >
  Show total count of missed calls + unread SMS in a large tile.
  Works offline using local call log and SMS inbox access.

match:
  - event: launcher_home_opened
  - event: app_resume

check:
  - call_count = read_missed_calls()
  - sms_count = read_unread_sms()
  - total_unread = call_count + sms_count

response:
  - update_tile("Unread", icon="envelope", badge=total_unread)
  - if total_unread > 0:
      show_reminder("You have missed calls or messages.")
  - if caregiver_online == false:
      add_note_to_tile("Caregiver not available.")

tags:
  - unread
  - sms
  - call_log