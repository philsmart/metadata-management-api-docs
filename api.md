# saml-md-man-api API documentation version v0.9
http://api.dev.ja.net:8100/saml-md-api/v0.9

---

## SAML Metadata Entitites

### /entities/{entityId}
Represents a single SAML Metadata Entity Descriptor resource. The EntityID URI parameter must be URL encoded.

* **get**: Returns the SAML Metadata Descriptor relating to the EntityId if it exists.
* **put**: Updates an existing SAML Entity Descriptor with given {entityId} if it exists.
* **delete**: Deletes a SAML Metadata Descriptor from the repository. Currently, deletes are DEFERRED only. The delete will not occur IMMEDIATLY on the LIVE repository, and will take affect on the LIVE repositories normal update schedule.

### /entities/
A Collection of all known SAML Metadata Entity Descriptors. Returned as a list of indivudual items.

* **get**: 
* **post**: Adds a new SAML Entity Descriptor fragment to the repository - subject to validation. Any validation errors are returned to the client.

