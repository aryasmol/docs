---
title: "Campaigns"
description: "Manage outbound calling campaigns."
---

The `CampaignsApi` allows you to create and manage bulk outbound calling campaigns.

## Clients

```python
from smallestai.atoms.api_client import ApiClient
from smallestai.atoms.api.campaigns_api import CampaignsApi

client = ApiClient()
api = CampaignsApi(client)
```

## Operations

### Create a Campaign

Launch a new outbound campaign.

```python
from smallestai.atoms.models import CampaignPostRequest

campaign = api.campaigns_post(
    campaign_post_request=CampaignPostRequest(
        name="Q3 Outreach",
        agentId="your-agent-id",
        contacts_list_id="contacts-list-id"
    )
)
print(f"Campaign Launched: {campaign.id}")
```
