# vCon Lifecyce Events and Operations

[vCons][draft-vcon] are powerful means to capture and collaborate on the details of a conversation, enabling entities to more accurately respond to customer needs, and entities to more effectively manage the health of their business.

vCons contain multiple elements of Personally Identifiable Information of the parties involved, including phone numbers, emails, voice and facial prints to location information.
These elements are what make vCons so powerful to assist customers.
However, the information must be handled responsibly, giving the involved parties an opportunity to consent to their information being captured, and the ability to revoke consent with the right to be forgotten.

Multiple regulations, including the US born [California Consumer Protection Act (CCPA)][CCPA] and the EU born [General Data Protection Regulation (GDPR)][GDPR] outline requirements for entities to responsibly use and dispose of PII information upon request.

This draft provides an overview of the requirements and a workflow to onboard customers with end-to-end, interoperable services and tooling.
The workflow enables entities in the workflow to be collaborate on a vCon, while assuring all entities in the workflow are adhering to relevant PII regulations.

## Conventions and Definitions

The following terms are from [draft-james-privacy-primer-vcon][privacy-primer-vcon], defining the three key roles in data processing:

- **Data Subject**: The individual whose personal information is being processed (also referred to as “consumer” in this RFC and many personal data privacy laws).
- **Data Controller**: An organization or individual with decision-making authority over data processing who determines the purposes and methods of data processing, bears primary responsibility under privacy laws and is the main target of most privacy and data protection regulations.
- **Data Processor**: Often a third-party service provider who processes data on behalf of the data controller.
Under Health Insurance Portability and Accountability Act (HIPAA), data processors are referred to as "business associates." Data processors may be hired for specialized tasks or to improve efficiency; can subcontract to other processors, creating a chain of responsibility; must operate within the scope defined by the data controller; and are expected to maintain trust and adhere to the controller's guidelines.

### Additional Terms

For the purposes of processing a vCon, the following additional terms are used:

- **Data Recorder**: The organization that records and initiates the vCon, identifying the parties, including the media of the conversation.
For a phone call, this may include the phone numbers and the audio recording.
For internet-based calls, such as Microsoft Teams, Zoom, Google meet, this may include emails and audio/video recording.
The data recorder is a facilitator with access to the content of the call.
The Data Recorder is not responsible for gaining consent, as they are providing infrastructure.
One or all of the Data Subjects must initiate consent, giving the Data Recorder the right to record the conversation.
- **Conserver**: A vCon workflow engine that ingests vCons, routing them for processing and enhancements.
A conserver doesn’t store a vCon, rather processes it real-time, retrieving it from, and saving it to a vCon Registry
- **vCon Registy**: A storage service, capable of storing vCons, including rich metadata and larger attachments including audio and video recordings.
- **Party**: A party, or participant of a vCon, as identified in the vCon draft.
- **Entity**: A generic reference to companies, groups or individuals that may share, alter, consume a vCon.
An entity may be a Data Recorder, Data Controller, Data Processor or some other role that has not yet been defined that participates in the possession and/or processing of a vCon.
- **SCITT Transparency Service**: An append-only ledger for integrity protecting vCons, ensuring the state of a vCon is known at a point in time for conformance to governance and regulatory requirements.

## vCon Lifecycle

vCons have a lifecycle from creation with consent, processing, enhancements, to revocation and deletion.
The workflow involves a vCon moving across multiple companies and network boundaries, often fanning out in 1:many relationships of sharing.
An entity that creates or consumes vCons will have several components to their vCon services:

<img src="./media/vcon-lifecycle.svg" alt="vCon Lifecycle" style="height: 300px;"/>

### Cloud and OnPrem services

While each entity of a workflow will want and need to manage their own instances of the vCon Services, the services are likely cloud-hosted SaaS, assuring the entity maintains their core business focus.

## Revocation and the Right to Be Forgotten

At any point, a Data Subject can request revocation, and/or the right to be forgotten, requiring all possessors of the vCon to act on the Data Subjects wishes.
A vCon must be tracked for where it was shared, and who to contact when a Data Subject makes their request.
The SCITT Transparency Service maintains the metadata for the state of a shared and ingested vCon, assuring the intent of the parties are integrity and inclusion protected.

<img src="./media/vcon-revoke-consent.svg" alt="vCon Lifecycle" style="height: 300px;"/>

## Standards Based Interoperability

The breadth of industries and scenarios that can benefit from virtual conversations spans vast numbers of companies.
The power in the technology is the ability to collaborate across conversations.
By implementing a standards-based approach, collaborative and competitive companies can easily integrate with solutions from transcription, consent management, CRM integration.

By packaging the conversation and the associated metadata within a vCon format, the parties have a means to communicate what has been shared.

Due to the personally identifiable information and digital fingerprinting of vCons, it’s just as important to understand and adhere to regulatory requirements for how PII information should be managed.
Defining and implementing a standard provides stability for how to manage the information and its lifecycle.

Recording what elements of the vCon were shared with various parties holding each party accountable for what was shared, what was received and when it occurred.

## vCon Lifecycle Events

The following events, or recorded operations outline important “moments that matter”, for the lifecycle events of a vCon.

A few operations overlap, for instance a vCon could be initially created (vcon_created), or it may be received as it passes across network boundaries, enabling a Data Controller or Data Processor to record the state of the vCon when they received it.
Other overlapping operations include a vCon that was redacted and deleted, or a vCon that may have expired and then was deleted.

The following events are not intended to be stored within the vCon, rather operations that are stored on SCITT, to provide context to the intent of the vCon at that point in time.

Note: should all vCon Operation Events be prefaced with “vcon_”

- `vcon_created:` The initial vCon document as it was first recorded.
The vCon may start with a basic structure to define the bare-bones metadata, as the recording or other attachments may be added asynchronously as their encodings are completed.
The following, mimimal elements MUST be set to be a valid vCon.
Additional elements MAY be set.
(TBD which elements)
- `vcon_amended:` The vCon has had some element amended.
While a vCon is considered immutable, information can be amended or appended.
Meaning, the previous values exist, but may be superseded with newer values.
This could include a transcription correction, or the changing of consent.
The vcon_amended operation should be used when no other specific amended/append statement is available.
It indicates it’s not the original vCon, but doesn’t have a well-known changed field.
- `vcon_transcribed:` The vCon was transcribed.
This may not be an important operation to individually record, as it may be one of many links in a conserver chain.
- `vcon_shared:` The vCon was shared with an external party.
This operation seals the integrity of the vCon, proving what was shared, to whom and when.
- `vcon_received:` Indicates the vCon was received from an external entity.
This indicates the entity has taking possession at a given time, sealing the integrity of the content received.
A vcon_received compared to a vcon_shared must be identical as it proves the receiving party received the exact vCon that was shared from a sending party.
A vCon may be shared with multiple entity.
To assure a chain of custody, knowing where a vCon was shared, the sending party must record who, when and how a vCon was shared with a receiving party, and if the receiving party accepted receipt.
- `vcon_consent_accepted:` One or more of the parties have given consent to the vCon being recorded and shared for its intended purpose.
Vcon_consent_accepted may not be set in the initial creation of the vCon, as it’s the responsibility of the Data Controller to manage the consent.
A Data Recorder provides infrastructure for the recording, and may not be responsible for tracking the consent of the parties.
As a result, a vCon may be created and shared across network boundaries and companies before the Data Controller has the opportunity to amend the vCon with who and how consent was given.
- `vcon_consent_revoked:` One or more parties have revoked their consent for one or more purposes.
Depending on the license and regulatory compliance associated with the vCon, one revocation of consent may require all data processors to act upon the revocation.
The specific actions to be completed are deferred to the license and associated regulatory compliance.
The recording of vcon_consent_revoked is a trigger for all entities in possession of the vCon to be notified and act accordingly.
- `vcon_party_redacted:` A parties to the vCon has been redacted.
This operation differs from consent revoked as a vCon may be capable of continuing if a party’s information can be redacted while maintaining the integrity and usefulness of the vCon.
For example, a party that listens, but does not communicate in the conversation may be redacted from being represented as a listening participant.
Note that redaction indicates the Data Controller may have history of the party’s participation, but is removed from any future shared copies of the vCon, and the redaction is shared to all consuming entities of the vCon.
A vCon that has been previously shared (vcon_shared) must communicate to all receiving entities of the vCon, that its state has changed, and they must act upon the vcon_party_redacted operation.
- `vcon_deleted:` The vCon has been deleted, as the outcome of an implicit act.
The vCon may be deleted due to revocation, or no longer needed by the Data Controller or Data Processors.
When a vCon is deleted, and no longer maintained by a Data Controller, the Data Controller must communicate with all receiving parties they must either delete the vCon, or become a Data Controller for it going forward.
- `vcon_expired:` The vCon has expired, due to terms of its license or compliance.
An expired vCon must be communicated with any receivers of the vCon, and they must delete it.
An expired vCon is a trigger for why a vCon may be deleted, identifying no specific entity requested its deletion, rather it’s simply timed out.
Identifying the future expiration date of a vCon is outside the scope of this operational identification.
This operation indicates it’s been expired, forcing deletion and communication to shared entities.

## Example vCon Lifecycles

The following are some examples for the lifecycle of a vCon, how it may be created, shared, consumed and updated across entities.

### vCon Creation

### vCon Consumption

### Data Subject Visibility

**TODO:** What visibility and access does a Data Subject have?

- Can they be sent a link to their vCons?
- What can they see or listen to?
- Can they, should they be able to see all the places their vCon has been shared?
  1. Is that too scary, and too much visibility into the sausage factory?
  1. Can they set the consent for purpose, or change the purpose?
  1. Can they revoke, and see the status of revocation (50% of all sharing has been completed)
  1. If someone has received a copy, but hasn’t acknowledged it, should the Data Subject be told which company that is?

## What Differentiates SCITT from a Database

The information written to SCITT could be compared to information written in a standard database.
So, what makes SCITT different?

To be elaborated upon:

- Immutable and Append Only nature
- Cryptographically secure as statements are pre-signed, making SCITT a notary of previously signed statements, assuring the SCITT service can’t alter the contents as it’s written
- Independently verifiable
- Separation of the immutable, append only ledger, the evidentiary metadata store which can be deleted and/or redacted for PII governance requirements
- The standards by which to communicate and enforce regulations and compliance

## Q & A

- What makes SCITT different from an immutable database?
- What makes SCITT different from Blockchain Technology?
- Are different ledger formats supported? 
  - Yes – Datatrails implements a Merkle Mountain Range implementation, providing performance, scale and the ability to purge older information reducing storage, while maintaining the overall integrity protection.
Other implementations like Microsoft CCF or RFC 9162 provide alternative formats.

## Appendix

### Open Items

#### vCon Manifests

vCons represent several elements, which contain rich privacy information, or large formats.
For reasons of privacy, conformance to governance, or cost efficiency of storage, vCon elements may be individually stored as blobs.
A vCon Manifest references the blobs, enabling the sharing of a subset of a vCon, while still integrity protecting the contents.

[CCPA]:                 https://oag.ca.gov/privacy/ccpa
[GDPR]:                 https://gdpr.eu/
[privacy-primer-vcon]:  https://datatracker.ietf.org/doc/draft-james-privacy-primer-vcon/
[draft-vcon]:           https://datatracker.ietf.org/wg/vcon/about