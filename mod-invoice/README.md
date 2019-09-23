# Introduction

This is the API Tests (Postman Collections) for [mod-invoice](https://github.com/folio-org/mod-invoice/blob/master/README.md) module.

## Collection utility functions

The functions and request body templates are defined in the `Pre-request Scripts` section of particular collection.

## Known limitations 
Once the collection run is completed, the second one might fail. The reason is that when DB schemas are deleted for test tenant, some modules still keep open DB connection in pool for a minute (please refer to [AsyncConnectionPool.java](https://github.com/folio-org/vertx-mysql-postgresql-client/blob/release-connection-pool-3-5-1-folio-1/src/main/java/io/vertx/ext/asyncsql/impl/pool/AsyncConnectionPool.java#L52)). When test tenant is created again in less then 1 min after previous run, the module's connection from pool cannot access DB schema because it is recreated but the connection still tries to access "old" DB schema.  
To avoid this issue, either wait for at least 1 minute after previous run completed or change `testTenant` collection level variable to some new value.

# Collections
## [mod-invoice](mod-invoice.postman_collection.json)
The collection contents set of tests to verify `invoicing` APIs and different workflows

### Collection structure

Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`Positive Tests` | Contains various requests and tests to verify success cases
`Negative Tests` | Contains various requests and tests to verify expected negative cases e.g. validation of the request etc.
`Cleanup` | Deletes test tenant

### Collection variables

Variable | Initial Value | Description  
 --- | --- | --- 
`testTenant` | invoicing_api_tests | Tenant identifier which is going to be used (created) for API tests
`resourcesUrl` | https://raw.githubusercontent.com/folio-org/mod-invoice/master/src/test/resources | Path to mod-invoice test resources
`mod-ordersResourcesURL` | https://raw.githubusercontent.com/folio-org/mod-orders/master/src/test/resources | Path to mod-orders test resources
`poLines-limit` | 10 | Purchase Order Lines Limit to be used for configuration update
`inventory-instanceTypeCode` | invoicingApiTestsIdentifierType | Inventory instance type code which is going to be used for instance creation when order transits to `Open` status
`finance-ledgerCode` | invoicingApiTests | Ledger code which is going to be used for fund creation (required for fund distributions)
`finance-fundCode` | invoicingApiTests | Fund code which is going to be used for fund distributions
`voucherNumberPrefix` | testPrefix | The config value for voucher prefix

## [mod-invoice-acq-units](mod-invoice-acq-units.postman_collection.json)
The collection contents set of tests to verify `invoicing` APIs behavior depending on acquisition unit(s) assignment

### Collection structure
Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`- Create tenant and enable modules` | Creates new tenant for API tests, enables required modules and creates admin user for this tenant
`- Prepare required external data` | Prepares data in the external modules e.g. active vendor
`- Create units` | Creates test acq units
`- Create regular users` | Creates user with all invoice permissions
`- Create limited user` | Creates user with granular permissions
`Positive Tests` | Contains various requests and tests to verify success cases
`Cleanup` | Deletes test tenant

### Collection variables

Variable | Initial Value | Description  
 --- | --- | --- 
`mod-invoicesResourcesURL` | https://raw.githubusercontent.com/folio-org/mod-invoice/master/src/test/resources | Path to mod-invoice test resources
`testTenant` | invoices_acq_units_test | Tenant identifier which is going to be used (created) for API tests

# Issue tracker

See project [MODINVOICE](https://issues.folio.org/browse/MODINVOICE) at the [FOLIO issue tracker](https://dev.folio.org/guidelines/issue-tracker).

# Other documentation

 * [Conduct API testing](https://dev.folio.org/guides/api-testing/)
 * Other [modules](https://dev.folio.org/source-code/#server-side) are described, with further FOLIO Developer documentation at [dev.folio.org](https://dev.folio.org/)
