The approvals API accepts a complete and valid SAML Entity Descriptor XML Fragment and stores it for later retrieval. Once retrieved, it is re-validated on the fly for any submission errors that require approval. The queued SAML Entity Descriptor should be **deleted** from the queue once fully approved - this is left to the client. A typical flow involving the Approval API is:
1. User adds or edits an Entity Descriptor, and submits it to the SAML Metadata Entities API.
- The SAML Metadata Entities API returns an error indicating it requires approval before it can be submitted - error set to be defined, e.g.:

  ```
  <Map>
  <timestamp>1466520037530</timestamp>
  <status>400</status>
  <error>Bad Request</error>
  <exception>uk.ac.jisc.saml.mdman.pipeline.ScopeNotAuthorisedException</exception>
  <message>Scope [cardiff.ac.uk] for EntityDescriptor https://idp-dev.cardiff.ac.uk/idp/shibboleth with path [/null/EntityDescriptor/Extensions/Scope] not authorised,Scope [cardiff.ac.uk] for EntityDescriptor https://idp-dev.cardiff.ac.uk/idp/shibboleth with path [/null/EntityDescriptor/IDPSSODescriptor/Extensions/Scope] not authorised,Scope [cardiff.ac.uk] for EntityDescriptor https://idp-dev.cardiff.ac.uk/idp/shibboleth with path [/null/EntityDescriptor/AttributeAuthorityDescriptor/Extensions/Scope] not authorised</message>
  <path>/saml-md-api/v0.9.1/entities/https%3A%2F%2Fidp-dev.cardiff.ac.uk%2Fidp%2Fshibboleth</path>
  </Map>
  ```

- If the Entity Descriptor is to be approved, the exact same SAML Entity Descriptor XML Fragment is _posted_ to the Approvals API.
2. The full list of Entity Descriptors that require approval (along with the reasons it requires approval) can be retrieved by _getting_ from the Approvals API.
3. Once an approval has been made (i.e. a member was retrieved from the Member API via a _GET_, modified to include the approved changes by the client, and _PUT_ back to the Member API) the Entity Descriptor should be resubmit to the SAML Metadata Entities API, and the entity should be _deleted_ from the Approvals API. Any subsequent authorisation issues should repeat steps 1-5.
