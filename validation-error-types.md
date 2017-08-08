| ErrorType  | Description  | Element Type |
|:-:|:-:| :-:|
| EntityIDApproval  | The entity ID has not been granted by or to the owning organisation  | EntityDescriptors
| DomainApproval  |   The domain e.g. in a scope in an EntityDescriptor or directly as a Domain on a Member, has not been granted by or to the owning organisation | Members/Domain Owners, EntityDescriptors
| Domain  |  The domain of the organisation is invalid in a way that can not be manually approved e.g. incorrect public suffix. |Members/Domain Owners, EntityDescriptors
|EntityAttributeApproval| An supplied EntityAttribute requires approval| EntityDescriptors
|Permissions|The client does not have permission to operate on behalf of the organisation |
|SchemaValidation|The XML Document does not match the declared schemas |Members/Domain Owners, EntityDescriptors
|Unknown | An error has been configured that does not have a mapping. |Members/Domain Owners, EntityDescriptors
