## Introduction

This is the API Tests (Postman Collection) for [mod-orders](https://github.com/folio-org/mod-orders/blob/master/README.md) module.

## Additional information

### Collection structure

Folder | Description  
--- | --- 
`Setup` | Contains various preparation requests/operations required for test runs
`- Auth` | Authorize user
`- Verify required modules enabled` | Validate that required modules enabled i.e. mod-orders, mod-configuration etc.
`- Update configs` | Update PO Lines limit (based on variable with default value 10);
`- Load schemas for validation` | Loads various schemas for validation
`Positive Tests` | Contains various requests and tests to verify success cases
`- Empty Order` | Verifies that an order can be created and deleted without order lines
`- Pending Order` | Verifies that an order in `Pending` status can be created. Verifies that existing PO Lines can be updated/deleted; new PO Lines can be added/updated/deleted.
`- Pending To Open order` | Verifies that order transition from `Pending` to `Open` status can be successful and expected records created in the Inventory.
`- Open order` | Verifies that an order can be successfully created in `Open` status and expected records created in the Inventory.
`- PO Number` | Verifies PO Number generation/validation
`- Get Orders` | Verifies that orders can be be retrieved by CQL query
`Negative Tests` | Contains various requests and tests to verify expected negative cases e.g. validation of the request etc.
`Cleanup` | Revert configuration settings back to initial values. Delete created inventory records. Delete created orders and verifies deletion.

### Collection variables

Variable | Description  
 --- | --- 
`mod-ordersResourcesURL` | Path to mod-orders test resources
`schema_loc` | Path to acquisitions schemas
`module` | Sub-Path to indicate where order storage schemas located
`schema_composite_purchase_order` | File name of the composite purchase order schema
`schema_adjustment` | File name of the adjustment schema
`schema_alert` | File name of the alert schema
`schema_claim` | File name of the claim schema
`schema_composite_po_line` | File name of the composite purchase order line schema
`schema_cost` | File name of the cost schema
`schema_details` | File name of the details schema
`schema_eresource` | File name of the eresource schema
`schema_fund_distribution` | File name of the fund distribution schema
`schema_location` | File name of the location schema
`schema_physical` | File name of the physical schema
`schema_renewal` | File name of the renewal schema
`schema_reporting_code` | File name of the reporting code schema
`schema_source` | File name of the source schema
`schema_vendor_detail` | File name of the vendor detail schema
`schema_metadata` | File name of the metadata schema
`raml_loc` | Path to RMB schemas
`poLines-limit` | Purchase Order Lines Limit to be used for configuration update

### Collection utility functions

The functions are defined in the Pre-request Scripts section of the collection. The main idea is to create reusable functions to not duplicate data in the tests.

### Issue tracker

See project [MODORDERS](https://issues.folio.org/browse/MODORDERS) at the [FOLIO issue tracker](https://dev.folio.org/guidelines/issue-tracker).

### Other documentation

 * [Conduct API testing](https://dev.folio.org/guides/api-testing/)
 * Other [modules](https://dev.folio.org/source-code/#server-side) are described, with further FOLIO Developer documentation at [dev.folio.org](https://dev.folio.org/)
