# Building a Custom Entity for Backstage

Example building a custom entity to represent Customer.

I found the docs at https://backstage.io/docs/features/software-catalog/extending-the-model#implementing-custom-model-extensions to be very difficult to follow, especially in terms of connecting the dots on all the pieces that need to be configured. Hopefully this helps someone out. <3

### Prerequsites

You must have a Backstage app built with `npx @backstage/create-app`

### Folders

* customer-common - contains the definition of our Customer entity, extremely simplified. This was buily by copying the https://github.com/backstage/backstage/tree/master/plugins/scaffolder-common package and removing unnecessary stuff.
* customer-backend - contains the validation logic used by Backstage to ensure our entity works correctly. This was generated with `yarn new --select backend-plugin` and modified to remove unnecessary stuff.

### Using

1. Copy the customer-common and customer-backend folders into the plugins/ folder of your backstage app
2. Modify the packages/backend/package.json file to include a reference to "@internal/plugin-customer-backend"
    ```
    "@internal/plugin-customer-backend": "^0.1.0",
    ```
3. Modify packages/backend/src/plugins/catalog.ts to include a reference to our new processor:

4. In app-config.yaml, add "Customer" to the catalog/rules/allow list:
    ```
    catalog:
    import:
        entityFilename: catalog-info.yaml
        pullRequestBranchName: backstage-integration
    rules:
        - allow: [Component, System, API, Resource, Location, Customer]
    ```
5. To test out your entity, create a new instance of our customer entity by modifying examples/entities.yaml, and adding the following to the bottom:
    ```
    ---
    apiVersion: myexample.com/v1beta3
    kind: Customer
    metadata:
    name: test-customer
    spec:
    owner: guests
    ```
6. Fire up Backstage `yarn dev`

### Testing

At this point, assuming everything worked correctly, you should be able to see your new Customer entity, as well as your test customer in the list, by choosing the "Customer" Kind on the catalog homepage.
