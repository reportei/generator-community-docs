# API Integration Considerations

## Authentication Type
Most APIs use OAuth, but some require custom credentials. All these scenarios are covered in the authentication documentation.

## Rate Limits
Some APIs have extremely low rate limits (requests per second or minute) and offer limited ways to fetch all necessary data. This often requires making multiple parallel requests, sometimes even paginating in parallel.

In most cases, exponential backoff solves the issue. This is already implemented by default in the `ApiHandler` class.

## Methods for Consuming the API

### REST
The most common approach is manually consuming API resources based on their documentation.

### Batch Requests
Less common but worth checking if the API supports it. Batch processing allows multiple requests to be sent within a single request and should be prioritized when available.

### SDK
Some APIs provide SDKs (libraries) for interaction. Before using an SDK, consider whether it meets our needs, specifically:
- Can it fetch all predefined network metrics efficiently?
- Is it actively maintained and frequently updated?
- Avoid SDKs that may become deprecated or are poorly maintained.

## Are Entities Well-Distributed?
Entities refer to endpoints or "pivots" that point to different resources. Examples:

- `/posts` → Returns post data
- `/user` → Returns user metrics

Clearly defined entities make request generation easier. Each widget dimension can be mapped to an entity, allowing requests to be dynamically generated based on received dimensions (e.g., a widget with the "posts" dimension triggers the correct API request).

## Can the Data Be Segmented?
Determine the level of segmentation available in API requests. Can we filter by:
- Month?
- Day?
- Week?
- Specific items like campaigns or posts?

## Does the API Support Webhooks?
Some APIs provide webhooks for tracking events such as:
- Account name changes
- Instagram story posts
- New opportunities in a CRM

As of this document's creation, only RD Station has implemented webhook reception and data storage. However, if an API offers useful webhooks, they can be received without issue using the `receiver`.

## Does It Require Monitoring?
Some data cannot be retrieved retroactively, such as Instagram followers. In such cases, daily monitoring is required to maintain a historical record.

Monitoring should be a last resort, but any network can be monitored using the tracker after assessing data volume and necessity.

## Does It Provide a Sandbox?
A sandbox is essentially a "dev mode" for the API, allowing testing of all features. It is particularly useful for verifying whether features can be tested without having data in the created account.

Always check for a sandbox, as it significantly speeds up development by eliminating the need for an account with existing data or approval processes.

