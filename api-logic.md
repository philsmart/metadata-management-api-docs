### Entity API

Calls to the following API methods invokes the logic described:

#### Get Entity



| Stage | Operation | Description |
|:-------:|:-----------:|:-------------:|
| During entity retrieval from repository | Filter on client Organization | Filter out the requested EntityDescriptor if it does not belong to the same Organisation as the client that requested it.|
| After retrieval   |     entity.render.GET MDA Pipeline      |      Removes managed elements from the EntityDescriptor. 1. mdrpi:RegistrationInfo 2. XML ID (the UK Fed ID of the entity)  3. xsi:schemaLocations        |

#### Get All Entities

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| During entity retrieval from the repository | Filter on client Organization | Filter out any EntityDescriptors that do not belong to the same Organisation as the client that requested them.|
| After retrieval    |     entity.render.GET MDA Pipeline      |      Removes managed elements from the set of EntityDescriptors retrieved . 1. mdrpi:RegistrationInfo 2. XML ID (the UK Fed ID of the entity)  3. xsi:schemaLocations        |

#### Add Entity

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Add | Check Duplicates |  Check the new EntityDescriptor does not exist in the repository. Throw 409 if it does.|
| Pre Add | Check EntityID exists | Check the EntityID exists on the EntityDescriptor to be added |
| Pre Add | Apply entity.ADD MDA Pipeline | 1. Validate the EntityDescriptor against its schema. 2. Check Scopes are authorised 3. Check EntityID is authorised 4. Perform import.xsl XSLT transformation 5. Validate the EntityDescriptor against its schema

#### Update Entity

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Update | Check EntityID resource | Check if the EntityID on the EntityDescriptor is the same as that on the resource request.|
| Pre Update | Check EntityID exists | Check the EntityID exists on the EntityDescriptor to be updated |
| Pre Update | Check EntityDescriptor exists | Check the EntityDescriptor exists in the repository, hence an update is being performed|
| Pre Update | Apply entity.UPDATE MDA Pipeline | 1. Validate the EntityDescriptor against its schema 2. Check Scopes are authorised 3. Check EntityID is authorised |
| Pre Update |  Apply entity.UPDATE.merge MDA Pipeline | 1. Merge update EntityDescriptor with existing EntityDescriptor (overwrite /md:EntityDescriptor/md:Extensions/mdrpi:RegistrationInfo and /md:EntityDescriptor/@ID in update document)|


#### Delete Entity

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Delete | Check EntityDescriptor exists | Check the EntityDescriptor exists in the repository and hence can be deleted|

****

### Member API

#### Get Member

| Stage | Operation | Description |
|:-------:|:-----------:|:-------------:|
| During Member retrieval from repository | Filter on client Organization | Filter out the requested Member/DomainOwner if it does not belong to the same Organisation as the client that requested it.|
| After retrieval   |     member.render.GET MDA Pipeline      |      currently does nothing       |

#### Get All Member

| Stage | Operation | Description |
|:-------:|:-----------:|:-------------:|
| During Member retrieval from repository | Filter on client Organization | Filter out the requested Member/DomainOwner if it does not belong to the same Organisation as the client that requested it.|
| After retrieval   |     member.render.GET MDA Pipeline      |      currently does nothing       |

#### Add Member

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Add | Check Duplicates |  Check the new Member/DomainOwner does not exist in the repository. Throw 409 if it does.|
| Pre Add | Check OrgID exists | Check the OrgID exists on the Member/DomainOwner to be added |
| Pre Add | Check Correct Type | Check the XML Element is a Member or DomainOwner|
| Pre Add | Apply members.validate.schema.ADD-UPDATE Pipeline | 1. Assemble All Members/DomainOwners including the new Member/DomainOwner into one Document 2. Validate the combined Members XML Document against its schema|
| Pre Add | Applymembers.validate.ADD Pipeline |  1. Validate any Domains 2. Check all Grants have been authorised.|
| Pre Add | Check client rights over domains | If the Member/DomainOwner being added includes Domains, check the client has Admin privileges|

#### Update Member

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Update | Check OrgID resource | Check if the OrgID on the Member/DomainOwner is the same as that on the resource request.|
| Pre Update | Check OrgID exists | Check the OrgID exists on the Member/DomainOwner to be updated |
| Pre Update | Apply members.validate.schema.ADD-UPDATE Pipeline | 1. Assemble All Members/DomainOwners including the new Member/DomainOwner into one Document 2. Validate the combined Members XML Document against its schema|
| Pre Update | Check Member/DomainOwner exists | Check the Member/DomainOwner exists in the repository, hence an update is being performed|
| Pre Update | Apply members.validate.UPDATE | 1. Add lastVerified timestamp (now) to Domains 2. Validate any Domains 2. Check all Grants have been authorised. |
| Pre Update |  Apply members.merge.UPDATE MDA Pipeline | 1. Merge update Member/DomainOwner with existing Member/DomainOwner (overwrite /mem:Member/mem:Name in update document)|
| Pre Update | Check client rights over domains | If the Member/DomainOwner being updated includes new Domains, check the client has Admin privileges|


#### Delete Member

| Stage | Operation | Decsription |
|:-------:|:-----------:|:-------------:|
| Pre Delete | Check Member/DomainOwner exists | Check the Member/DomainOwner exists in the repository and hence can be deleted|
