# Introduction

This is the API Tests (Postman Collection) for [mod-orders](https://github.com/folio-org/mod-orders/blob/master/README.md) module.

## Collection utility functions

Each collection has functions and request body templates defined in the `Pre-request Scripts` section of the collection to reuse code as much as possible.

## Known limitations
Once the collection run is completed, the second one might fail. The reason is that when DB schemas are deleted for test tenant, some modules still keep open DB connection in pool for a minute (please refer to [AsyncConnectionPool.java](https://github.com/folio-org/vertx-mysql-postgresql-client/blob/release-connection-pool-3-5-1-folio-1/src/main/java/io/vertx/ext/asyncsql/impl/pool/AsyncConnectionPool.java#L52)). When test tenant is created again in less then 1 min after previous run, the module's connection from pool cannot access DB schema because it is recreated but the connection still tries to access "old" DB schema.  
To avoid this issue, either wait for at least 1 minute after previous run completed or change `testTenant` collection level variable to some new value.

# Collections

## [acquisitions-units](acquisitions-units.postman_collection.json)
The collection verifies `acquisitions-units` CRUD APIs behavior

### Collection structure
Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`- Create tenant and enable modules` | Creates new tenant for API tests, enables required modules and creates admin user for this tenant
`- Create regular user` | Creates user with `acquisitions-units` permissions only
`Positive Tests` | Contains various requests and tests to verify success cases
`Negative Tests` | Contains various requests and tests to verify expected error cases
`Cleanup` | Deletes test tenant

### Collection variables

Variable | Initial Value | Description  
 --- | --- | --- 
`testTenant` | acq_units_api_tests | Tenant identifier which is going to be used (created) for API tests

## [mod-orders](mod-orders.postman_collection.json)
The collection verifies `orders` APIs and different workflows

### Collection structure

Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`- Create tenant and enable modules` | Creates new tenant for API tests, enables required modules and creates admin user for this tenant
`- Update configs` | Update PO Lines limit (based on variable with default value 10);
`- Load all schemas for validation` | Loads various schemas for validation
`- Prepare global data` | Prepares data like `institution`, `campus` etc.
`- Prepare vendors` | Prepares active and inactive vendors
`- Prepare finance data` | Prepares active and inactive vendors
`- Prepare inventory types` | Prepares inventory types to not rely on reference data presence
`- Create regular user` | Creates user with `acquisitions-units` permissions only
`Positive Tests` | Contains various requests and tests to verify success cases
`- Empty Order` | Verifies that an order can be created and deleted without order lines
`- Pending Order` | Verifies that an order in `Pending` status can be created. Verifies that existing PO Lines can be updated/deleted; new PO Lines can be added/updated/deleted.
`- Pending To Open order` | Verifies that order transition from `Pending` to `Open` status can be successful and expected records created in the Inventory.
`- Open order` | Verifies that an order can be successfully created in `Open` status and expected records created in the Inventory.
`- Receiving` | Verifies that resources can be successfully received for `Open` orders and item records updated in the Inventory.
`- Close orders updating payment status of each PO Line` | Update PO Lines' payment statuses of `Open` orders so the system should close the order eventually (see [MODORDERS-218](https://issues.folio.org/browse/MODORDERS-218)).
`- Check closed orders and re-open` | Contains sets of requests: <br> 1. Get orders and verify that their workflow status is `Closed`. If this is `true`, modify one of PO Line's payment status so the order becomes `Open` eventually.  <br> 2. Roll back one of received piece so the order becomes `Open` eventually. <br> 3. Verify that orders were successfully re-opened by operations described above. <br> <br> See [MODORDERS-218](https://issues.folio.org/browse/MODORDERS-218) for more details.
`- PO Number` | Verifies PO Number generation/validation
`- Get Orders` | Verifies that orders can be be retrieved by CQL query
`- Get Order Lines` | Verifies that order lines can be be retrieved by CQL query
`- Pieces Creation` | Verifies that piece records can be created for check-in flow
`- Receiving History` | Verifies receiving history endpoint
`- Create Inventory` | Various tests to verify order creation in `Open` status with different tenant configs
`- Check-in` | Verifies that resources can be successfully checked-in for `Open` orders and item records updated in the Inventory.
`- Check Order automatically changes workflowStatus` | Various tests to verify automatic workflow transition from Open to Closed based on different PO line's statuses
`- ISBN validation` | ISBN numbers validation
`Negative Tests` | Contains various requests and tests to verify expected negative cases e.g. validation of the request etc.
`- Create Order for tests` | Create order and PO line with content required for negative tests
`- Order` | Various requests and tests to verify expected negative cases for endpoints managing order 
`- Lines` | Various requests and tests to verify expected negative cases for endpoints managing order lines 
`- PO Number` | Various requests and tests to verify expected negative cases for endpoints validating PO number 
`- Receiving` | Various requests and tests to verify expected negative cases like already received resources cannot be received again.
`Cleanup` | Revert configuration settings back to initial values. Delete created inventory records. Delete created orders and verifies deletion.

### Collection variables

Variable | Initial Value | Description  
 --- | --- | --- 
`testTenant` | orders_api_tests | Tenant identifier which is going to be used (created) for API tests
`mod-ordersResourcesURL` | https://raw.githubusercontent.com/folio-org/mod-orders/master/src/test/resources | Path to mod-orders test resources
`poLines-limit` | 10 | Purchase Order Lines Limit to be used for configuration update
`inventory-identifierTypeName` | ordersApiTestsIdentifierTypeName | Inventory identifier type name which is going to be used for PO Lines product type
`inventory-instanceTypeCode` | ordersApiTestsInstanceTypeCode | Inventory instance type code which is going to be used for instance creation when order transits to `Open` status
`inventory-instanceStatusCode` | ordersApiTestsInstanceStatusCode | Inventory instance status code which is going to be used for instance creation when order transits to `Open` status
`inventory-loanTypeName` | ordersApiTestsLoanTypeName | Inventory loan type name which is going to be used for item creation when order transits to `Open` status
`tenant.addresses` | `{"address": "sample address", "name": "sample name"}` | Tenant address

## [mod-orders-acq-units](mod-orders-acq-units.postman_collection.json)
The collection verifies `orders` APIs behavior depending on acquisition unit(s) assignment

### Collection structure
Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`- Create tenant and enable modules` | Creates new tenant for API tests, enables required modules and creates admin user for this tenant
`- Update configs` | Update PO Lines limit (based on variable with default value 10);
`- Prepare required external data` | Prepares data in the external modules e.g. active vendor
`- Create units` | Creates test acq units
`- Create regular user` | Creates user with all `orders` permissions
`- Create limited user` | Creates user with limited set of `orders` permissions
`- Setup new tenant` | Create new tenant to verify tenant-specific logic
`Positive Tests` | Contains various requests and tests to verify success cases
`Negative Tests` | Contains various requests and tests to verify expected error cases
`Cleanup` | Deletes test tenant

### Collection variables

Variable | Initial Value | Description  
 --- | --- | --- 
`mod-ordersResourcesURL` | https://raw.githubusercontent.com/folio-org/mod-orders/master/src/test/resources | Path to mod-orders test resources
`poLines-limit` | 10 | Purchase Order Lines Limit to be used for configuration update
`testTenant` | orders_acq_units_test | Tenant identifier which is going to be used (created) for API tests

# Issue tracker

See project [MODORDERS](https://issues.folio.org/browse/MODORDERS) at the [FOLIO issue tracker](https://dev.folio.org/guidelines/issue-tracker).

# Other documentation

 * [Conduct API testing](https://dev.folio.org/guides/api-testing/)
 * Other [modules](https://dev.folio.org/source-code/#server-side) are described, with further FOLIO Developer documentation at [dev.folio.org](https://dev.folio.org/)
