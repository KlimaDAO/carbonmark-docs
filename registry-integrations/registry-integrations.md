# Registry Integrations

Carbonmark leverages a modular approach to integrate environmental assets issued across a range of registries onto its platform. In doing so, it harmonizes data types and creates native interoperability for its assets across its secure, blockchain-enabled infrastructure.

## Credit Issuance Framework

Our backend infrastructure is powered by the Base public blockchain. This ensures all activities on our platform are auditable by third parties, including all credit transfers to/from our platform, credit purchases, and their retirements. There are three primary models we utilize depending on the capabilities of a credits underlying credit registry:&#x20;

1. **Asynchronous offchain:** refers to carbon registry connections where the underlying registry does not have native API capabilities and therefore cannot ensure automated syncing of credit events on our platform with their system. In this model, a public custodial account is generally utilized to house credits securely while representations of those credits are tokenized and issued onto our platform. A set schedule is then maintained where events on our platform and within the registry are synced (e.g. retirements). These syncing events are usually carried out in a batched process.&#x20;
2. **Synchronous offchain:** refers to carbon registry connections where the underlying registry has API connectivity whereby onchain and offchain carbon credit events can be synchronized. Carbon credit events include transfers, status changes (e.g. “immobilization”), retirements, etc. Note that in some cases due to limitations in carbon credit fractionalization, retirement events are sometimes batched to ensure full tonne synchronisation on the underlying registry with our platform.&#x20;
3. **Synchronous onchain:** refers to carbon registries that utilize similar blockchain infrastructure as Carbonmark, whereby assets are already natively represented as tokens and transfers can be seamlessly made to Carbonmark accounts. Note that in some cases registries utilizing this model require some level of data harmonization with our infrastructure to ensure full support.

## Required Tooling

Pursuant to maintaining full synchronization between our system and a credit’s underlying registry, Carbonmark’s preference is to integrate with registries that provide a full suite of API connectivity, for both data and action-oriented features. We’ve listed critical requirements below:

1. **Credit and project data:** Retrieve all project details, current vintages, status, balances and credit statuses (cancelled, tokenized, retired etc..) Explicitly include stable identifiers, with carbonmarkCreditId: in Carbonmark creditId format of registry-registryProjectId-vintage.
2. **Account and balance discovery/verification:** Verify participating registry accounts, their eligibility to transact, and available credit balances.
3. **Tokenization:** Carbonmark requires an onchain representation of registry credits on the Base network. Therefore, any credits represented on Base need to be locked within the registry records, or on alternate networks before Carbonmark Base issuance, to ensure there is only live representation of that credit at one time before any onchain credit issuance. The tokenization endpoint should lock that credit in the passed amount for any request except an authorized retirement or detokenization request. This status should be exposed in the project data endpoint.
4. **Detokenization:** Return credits to an eligible registry account after the corresponding onchain tokens have been verifiably burned or otherwise permanently removed from circulation. Track the request through registry confirmation.
5. **Retirement:** Submit retirement requests with beneficiary details and supporting metadata, and return confirmation, affected serial ranges, and registry retirement certificates.
6. **Status updates and reconciliation:** Retrieve transaction history and changes to credit or account status so Carbonmark can reconcile balances and lifecycle events across both systems.
7. **Smart contract requirements:**
   1. Base: If the registry already has Base credits, Carbonmark will need all relevant verified contract addresses to index these credits. You must let Carbonmark know of any contract upgrades to ensure integration remains operational.
   2. Other networks: If credits are issued on another network, Carbonmark will issue a Base instance of those credits for both auditability and integration within our system. In order to do so, we require a locking method to ensure that the underlying credits are not transacted while represented on the Base network. This can be an escrow, lock method on the contract etc
   3. If credits are not represented on any blockchain network, Carbonmark will issue credits on Base.

## Security & Control&#x20;

Carbonmark’s credit representation model provides us with a number of compliance control surfaces while maintaining the auditability and interoperability afforded by public blockchain infrastructure. These include:&#x20;

1. Ability to lock credits of different provenances to strict transfer rulesets, including locking them within Carbonmark’s platform and preventing unauthorized transfer outside of our ecosystem. This also includes gating certain behaviors or users within the ecosystem itself.
2. Ability to gatekeep certain functionality, including credit secondary sales and retirements such that only certain authorized actions can be performed on specific credit types.&#x20;
3. Auditable administrative actions: freezes, permission changes, clawbacks, cancellations.

Carbonmark provides registry dashboards to enable operators to check the status of transferred credits to ensure compliance and synchronous status with their own registry backends.

