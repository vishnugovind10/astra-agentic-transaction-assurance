# Commercial Model and Customer Discovery

## 1. Initial commercial wedge

Agentic procurement of external digital resources and services: compute, inference, APIs, datasets, security analysis and specialist agent services.

The first customer hypothesis is a payment infrastructure provider or agent-service marketplace that needs enterprise-grade transaction assurance alongside existing identity, wallet and payment execution systems.

## 2. Buyer and user roles

- **Economic buyer:** head of payments, platform product, treasury, risk or enterprise AI infrastructure.
- **Technical buyer:** CTO, payment architect, security architect or platform engineering lead.
- **Operator:** agent platform operator, marketplace operator or AP/treasury team.
- **External counterparty:** service provider or provider agent.

## 3. Pain hypothesis

Existing agent identity/payment layers help an agent discover a service and make a payment. Enterprise adoption also needs the principal to establish what transaction was authorised, whether the destination and terms remained unchanged, whether multiple agents are creating duplicate claims, what cumulative exposure exists and whether the observed settlement can be reconciled.

This is a hypothesis to validate through interviews and pilots, not a claim that all current systems lack these controls.

## 4. Potential customers

1. Agent-payment infrastructure providers seeking an assurance module.
2. Compute/inference marketplaces with autonomous buyer and provider agents.
3. Enterprise AI platforms that allow agents to procure paid tools and services.
4. AP/treasury technology providers extending toward autonomous invoice execution.

## 5. Commercial packaging

- Paid architecture and integration engagement.
- API/SDK licensing or platform subscription.
- Usage-based fee per verified/reconciled transaction, where commercially and legally appropriate.
- Enterprise deployment, support and assurance reporting.

Do not base the initial business model on custody, lending, issuing guarantees, underwriting credit or taking a spread on customer funds. Astra should initially sell software assurance and integration.

## 6. QDay partnership proposition

QDay provides the EVM settlement environment and may provide agent identity, wallet controls and payment discovery/execution capabilities as they become available. Astra supplies a complementary transaction-assurance layer.

Proposed milestones:

1. Paid technical architecture and integration specification.
2. Paid testnet implementation and security/integration milestone.
3. Paid integration with QDay agent/payment components when available.
4. Design-partner pilot with one compute/API/service marketplace.

Any engagement must define payment, milestone acceptance, IP ownership, background IP, new IP, licensing, attribution, support and exclusivity. Existing portfolio repositories remain the owner's pre-existing IP.

## 7. Customer discovery questions

### Agent payment infrastructure
- What assurance happens between a payment mandate and settlement?
- How are beneficiary/payee changes detected?
- How do you prevent duplicate work/quote payments across multiple agents?
- What evidence is necessary for an enterprise to recognise a payment as reconciled?
- Which part should the payment network own and which part can an external assurance SDK own?

### Compute and service marketplaces
- Does each offer bind the provider identity, receiving wallet, job, price and expiry?
- Can a buyer independently verify usage and service receipts?
- What are the common causes of duplicate or disputed charges?
- Who bears loss when the provider wallet changes or the service is not delivered?

### Enterprises
- What supplier and payment data is authoritative?
- Which systems update the payable after stablecoin settlement?
- What is the required distinction between on-chain confirmation and accounting discharge?
- What evidence and audit record would risk/treasury require?

## 8. Commercial success criteria

- One infrastructure/provider partner confirms the specific assurance gap.
- One prospective customer agrees to technical discovery or a design-partner pilot.
- One paid architecture or implementation milestone is defined.
- The scope does not require Astra to become a custodian, lender or regulated payment provider by default.
