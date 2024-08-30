
# WebExpenses API Integration with Azure API Management

## Overview

This project integrates the WebExpenses API with Azure API Management (APIM) to enforce access control based on `subscriptionId`. The project is designed to map `subscriptionId` to `divisionId` and inject this `divisionId` into the API request body, ensuring that users only access data specific to their assigned division.

## Features

- **Subscription-Based Access Control**: Users are assigned unique `subscriptionId`s, which are mapped to specific `divisionId`s. Only users with valid `subscriptionId`s can access the API.
- **Dynamic Request Modification**: The request body is dynamically modified to include the `divisionId`, ensuring that the backend API processes the request according to the user's division.
- **Centralized API Management**: All API requests are managed and routed through Azure API Management, providing a secure and scalable way to manage access to the WebExpenses API.

## Prerequisites

Before you start, ensure you have the following:

- An active **Azure Subscription**.
- **Azure API Management** instance set up in your Azure portal.
- Access to the **WebExpenses API** with appropriate credentials.
- **Azure AD** (Azure Active Directory) configured for user authentication, if needed.

## Project Structure

```
├── policies/
│   ├── inbound-policy.xml  # Inbound policy to handle request processing
│   └── outbound-policy.xml # Outbound policy to handle response processing
├── README.md               # Project documentation
└── .env                    # Environment variables (e.g., for Azure AD, API credentials)
```

## Setup Instructions

### 1. Configure Backend Service

- Log in to your Azure portal and navigate to your Azure API Management instance.
- Define a backend service for the WebExpenses API:
  - **Name**: `webexpensesapi`
  - **URL**: The base URL for your WebExpenses API.
  - **Authentication**: Configure any necessary authentication (e.g., Basic, OAuth).

### 2. Configure the API in Azure API Management

- Create a new API or use an existing one within your APIM instance.
- Link the API to the `webexpensesapi` backend service.

### 3. Define API Management Policies

- **Inbound Policy**: This policy handles the mapping of `subscriptionId` to `divisionId` and injects the `divisionId` into the request body.

  Example policy (`inbound-policy.xml`):

  ```xml
  <inbound>
    <base />
    
    <!-- Set the backend service to direct requests to Webexpenses API -->
    <set-backend-service id="apim-generated-policy" backend-id="webexpensesapi" />

    <!-- Set the subscriptionId variable -->
    <set-variable name="subscriptionId" value="@(context.Subscription.Id)" />
    
    <!-- Map subscriptionId to divisionId -->
    <choose>
      <when condition="@(context.Subscription.Id == 'subscription-id-for-abp-uk')">
        <set-variable name="divisionID" value="1" />
      </when>
      <when condition="@(context.Subscription.Id == 'subscription-id-for-abp-ireland')">
        <set-variable name="divisionID" value="2" />
      </when>
      <when condition="@(context.Subscription.Id == 'subscription-id-for-c-and-d')">
        <set-variable name="divisionID" value="3" />
      </when>
      <!-- Add additional conditions for other subscriptions -->

      <!-- Otherwise block to set divisionID to -1 for unmatched subscriptions -->
      <otherwise>
        <set-variable name="divisionID" value="-1" />
      </otherwise>
    </choose>
    
    <!-- Modify the request body to include divisionID -->
    <set-body>@{
        var originalBody = context.Request.Body.As<JObject>(true);
        originalBody["divisionID"] = context.Variables.GetValueOrDefault<string>("divisionID");
        return originalBody.ToString();
    }</set-body>

  </inbound>
  <backend>
    <forward-request />
  </backend>
  <outbound>
    <base />
  </outbound>
  ```

- **Outbound Policy**: Typically used for modifying the response. In this project, it is left as default unless specific response modifications are required.

### 4. Assign Subscription IDs

- Create users in Azure API Management and assign them unique `subscriptionId`s.
- Map each `subscriptionId` to a corresponding `divisionId` within the `inbound-policy.xml` file.

### 5. Test the Configuration

- Use tools like Postman or curl to send requests to the API.
- Test with valid and invalid `subscriptionId`s to ensure the policy correctly maps to `divisionId` and restricts access accordingly.

  **Example Scenarios:**
  - Valid `subscriptionId` returns the correct `divisionId` in the request body.
  - Invalid or no `subscriptionId` results in access denied or `divisionId` set to `-1`.

## Error Handling

- **Access Denied**: If no `subscriptionId` is provided or it is invalid, the user will receive an access denied message.
- **Unknown Division**: If the `subscriptionId` does not match any known division, the `divisionId` is set to `-1`, indicating an unknown or unauthorized division.

## Troubleshooting

- **JWT Validation Errors**: Ensure that your `subscriptionId` is correctly linked to a user and that the inbound policy is configured correctly.
- **Backend Service Not Found**: Verify that the `backend-id` in the `set-backend-service` policy matches the backend service configured in Azure API Management.

## Future Enhancements

- **Dynamic Backend Selection**: Extend the policy to dynamically select different backend services based on additional criteria.
- **Advanced Error Handling**: Implement more sophisticated error handling and logging mechanisms.
- **Role-Based Access Control**: Integrate Azure AD groups for more granular access control.

## Next Steps

### 1. Deploying the Live Version for Olleco Division

- Once the current setup has been thoroughly tested and validated, deploy the live version of this integration specifically for the Olleco division.
- Assign a unique `subscriptionId` for Olleco users and map it to the corresponding `divisionId`.
- Monitor the deployment for any issues and gather feedback from the users in Olleco to ensure that the integration works smoothly.

### 2. Expanding to Other Divisions

- If the deployment for Olleco is successful, continue the process by mapping more `subscriptionId`s to `divisionId`s for other divisions as per company requirements.
- Use the same methodology to ensure each division has secure and appropriate access to the WebExpenses API.
- Regularly review and update the policy configurations to accommodate any new divisions or changes in company structure.

## Contributing

Please update this file with enhancements, bug fixes, or documentation improvements.
