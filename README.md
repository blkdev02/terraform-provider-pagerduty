Status:  This is currently just documentation of what resources we should expect in an initial Pagerduty provider, no implementation.

Upstream Reference: https://developer.pagerduty.com/documentation/rest/authentication/

Example:
```
provider "pagerduty" {
  authorizaton_token = "YOURTOKENHERE",
  requester_id = "PP123123",
  base_url "https://yoursubdomain.pagerduty.com/api/v1"
}

resource "pagerduty_user" "user1" {
  name = "Some Developer",
  email = "some.developer@yourorg.com"
}

// not doing contact_methods since that should be self-service

resource "pagerduty_team" "team1" {
  name = "A Mob of Emus",
  description "Marauding across Australia"
}

resource "pagerduty_escalation_policies" "escalation1" {
  name = "Escalation Policy",
  escalation_rules {
    escalation_delay_in_minutes = 22,
    targets = [{
      type = "user",
      id = "BLAHBLAH"
    }]
  },
  num_loops = 1
}

resource "pagerduty_service" "service1" {
  name = "My Service",
  escalation_policy_id = "${pagerduty_escalation_policies.escalation1.id}",
  type = "generic_events_api",
  description = "My Service Description",
  acknowledgment_timeout = "1800",
  auto_resolve_timeout = "14400",
  severity_filter = "???"
}

resource "pagerduty_service_maintenance_window" "maint_window1" {
  maintenance_window {
    start_time = "",
    end_time = "",
    service_ids = ["PF123123", "PF234234"]
  }
}
```
