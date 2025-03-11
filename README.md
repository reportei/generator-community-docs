# Adding a New Integration to Reportei

## Getting Started

Now that your environment is set up, you can begin integrating a new network. If you haven't set up the environment yet, follow the instructions in the **[original repository](https://github.com/reportei/generator-community)** where we configured the setup.

## **Start Here**
- [Tutorial](./sections/practical_example.md) Practial example showing step-by-step how to develop an integration

- [Checklist](./sections/checklist.md): Review the essential considerations before starting the integration process.

- [New Integration](./sections/new_integration.md): Describes each file and each process of developing a new integration.

- [Payloads](./sections/metrics.md): Understand the payload structure required for metric formatting and data processing.

- [Authentication](./sections/authentication.md): Follow the authentication process for integrating with external APIs.

## **Postman Collection**
Download the **Postman collection** [here](./sections/collection/Generator%20Community.postman_collection.json) to test API requests easily during development.

## **Testing the Integration**

- Use **`/source`** with the created [payload](./sections/metrics.md) to test the developed integration and ensure the metric formatting process works correctly.

- Use **`/auth`** to test the authentication process for the integration.

Following these steps ensures that your integration is properly configured, authenticated, and tested within the Reportei system.

