# Adding a New Integration to Reportei

## Getting Started

Now that your environment is set up, you can begin integrating a new network. If you haven't set up the environment yet, follow the instructions in the **[original repository](https://github.com/reportei/generator-community)** where we configured the setup.

## **Start Here**

- [Checklist](./sections/checklist.md): Review the essential considerations before starting the integration process.

- [Payloads](./sections/metrics.md): Understand the payload structure required for metric formatting and data processing.

- [Authentication](./sections/authentication.md): Follow the authentication process for integrating with external APIs.

- [New Integration](./sections/new_integration.md): Step-by-step guide to implementing a new network in Reportei.

## **Start Here**
To begin the integration process, refer to the **`new_integration.md`** file, which provides a step-by-step guide for implementing a new network in Reportei.

## **Postman Collection**
Download the **Postman collection** [here](./sections/collection/Generator%20Community.postman_collection.json) to test API requests easily during development.

## **Testing the Integration**

- Use **`/source`** with the created [payload](./sections/metrics.md) to test the developed integration and ensure the metric formatting process works correctly.

- Use **`/auth`** to test the authentication process for the integration.

Following these steps ensures that your integration is properly configured, authenticated, and tested within the Reportei system.

