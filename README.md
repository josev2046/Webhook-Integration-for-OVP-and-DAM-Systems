# Webhook Integration between OVP and DAM Systems
Webhooks offer a powerful mechanism for synchronising assets across systems, such as OVPs and DAMs. This document outlines the potential use of webhooks in Digital Asset Management (DAM) and Media Asset Management (MAM) integrations. 

![image](https://github.com/user-attachments/assets/27987c69-b286-489e-9462-f855a1f0ec9b)

### Vimeo Webhook Functionality:

Vimeo app webhooks provide instant updates on user actions. Webhooks can therefore enable near real-time notifications of events within an application. By configuring webhooks, you can subscribe to specific events and receive automated notifications via HTTP POST requests to a designated URL.

#### Prerequisites:

* Vimeo Enterprise Account
* Vimeo API Application
* Webhooks Capability (`CAPABILITY_WEBHOOK_SCOPE`)
* Access Token (with `public`, `private`, `edit`, `delete`, and `webhooks` scopes)
* Consider using official SDKs (e.g., PHP, Python, Node.js)

#### Configuration:

A webhook's target URL is a user-defined, publicly accessible endpoint.
Vimeo delivers event notifications via HTTP POST requests to this URL.

#### Operation:

##### Create:

`POST <https://api.vimeo.com/apps/{app_id}/webhooks>`


**Headers:**

`Authorization: bearer {access_token}
Content-Type: application/json
Accept: application/vnd.vimeo.*+json;version=3.4`


**Body:**

**Example:**

~~~json
{
  "is_enabled": true,
  "webhook_url": "[https://example.com/webhook](https://example.com/webhook)",
  "webhook_type": "video-transcode-playable",
  "secret": "your-secret-key"
}
~~~
**Configurable parameters:** `is_enabled` (Boolean), `webhook_url` (String), `webhook_type` (String), `secret` (String)

##### Retrieve:

**Single:**

`GET <https://api.vimeo.com/apps/{app_id}/webhooks/{webhook_id}>`


**All:**

`GET <https://api.vimeo.com/apps/{app_id}/webhooks>`


**Headers:**

`Authorization: bearer {access_token}
Accept: application/vnd.vimeo.*+json;version=3.4`


##### Update:

`PATCH <https://api.vimeo.com/apps/{app_id}/webhooks/{webhook_id}>`


**Headers:** Same as `POST`

**Body:** Include modified parameters (e.g.,  `{"is_enabled": false}`)

##### Delete:

`DELETE <https://api.vimeo.com/apps/{app_id}/webhooks/{webhook_id}>`


**Headers:** Same as `POST`

**Webhook Security:**

Implement webhook signature validation.
Use a secret key during webhook creation/update.
Vimeo signs requests with the X-Webhook-Signature header.
Validate signatures using SHA256 HMAC.

**Webhook Events and Payloads:**

Events trigger webhooks with specific payloads.
Delivery headers: `Content-Type`, `X-Webhook-Signature`
Supported events: `live-event-started`, `live-event-updated`, `live-event-ended`, `live-event-archive-available`, `live-event-clip-created`, `video-deleted`, `video-created`, `video-transcode-playable`, `video-transcode-fully-playable`, `video-transcode-complete`, `transcript-status-complete`, `automatic-thumbnail-available`, `video-upload-failed`, `content-scan-completed`.
Payload structure: `webhook_type, data object, timestamp`
Refer to Vimeo documentation for detailed payload information for each event.

**Rate Limits:**

Webhook requests are subject to rate limits based on Vimeo account type.
Rate limiting headers: `X-Webhook-RateLimit-Limit`, `X-Webhook-RateLimit-Remaining`, `X-Webhook-RateLimit-Reset`
Exceeding rate limits will result in blocked requests until the quota resets.

**Error Handling:**

Common errors:
`2204` (parameter problem)
`2205` (bad body format)
`8002` (unrecognized access token)
Resolution: Verify parameters, body format, and access tokens.

**Further information:**

https://developer.vimeo.com/api/app-webhooks
