The approvals API accepts a complete and valid SAML Entity Descriptor, Member or DomainOwner XML Fragment and stores it for later retrieval. Once retrieved, it is re-validated on the fly for any submission errors that require approval. The queued Element should be **deleted** from the queue once fully approved - this is left to the client. A typical flow involving the Approval API is:
1. User adds or edits an Entity Descriptor, and submits it to the SAML Metadata Entities API.
- The SAML Metadata Entities API returns an error indicating it requires approval before it can be submitted - error set to be defined, e.g.:

  ```
	<Map>
   <path>/saml-md-api/v0.9.2/entities</path>
   <validationErrors>
      <error>
         <id>https://idp2.iay.org.uk/idp/shibboleth5555</id>
         <message>Scope [diay.org.uk] for EntityDescriptor https://idp2.iay.org.uk/idp/shibboleth5555 with path [/null/EntityDescriptor/Extensions/Scope] not authorised</message>
         <validationComponentId>checkEntityScope</validationComponentId>
         <errorType>DomainApproval</errorType>
         <requiresApproval>true</requiresApproval>
      </error>
   </validationErrors>
   <timestamp>1472208079363</timestamp>
   <status>422</status>
</Map>
  ```

- If the Entity Descriptor is to be approved, the exact same SAML Entity Descriptor XML Fragment is _posted_ to the Approvals API.
2. The full list of Entity Descriptors that require approval (along with the reasons it requires approval) can be retrieved by _getting_ from the Approvals API.
3. Once an approval has been made (i.e. a member was retrieved from the Member API via a _GET_, modified to include the approved changes by the client, and _PUT_ back to the Member API) the Entity Descriptor should be resubmit to the SAML Metadata Entities API, and the entity should be _deleted_ from the Approvals API. Any subsequent authorisation issues should repeat steps 1-5.
