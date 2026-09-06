
**ABAP CDS Views
Project & Business Scenario Specification: **

Common Delivery Standards
Area	Standard
Naming	ZI_* for interface/composite entities; ZC_* for consumption entities; use organization naming standards.
Client handling	Use CDS client behavior supported by the platform. Do not manually add client predicates without design justification.
Semantics	Apply currency, unit, amount, quantity, date, and text associations.
Performance	Filter early, select only required fields, use set-based calculations, and avoid unnecessary distinct operations.
Authorization	Use DCL where row-level restrictions are required; test positive and negative access.
Extensibility	Prefer released CDS extension points and metadata extensions.
Consumption	ABAP SQL, analytical query, OData/service definition, Fiori Elements, RAP, or external API as applicable.
Quality gate	Activation, syntax check, ATC, data reconciliation, authorization test, performance trace, and regression test.

 1. Production Status
Specification Attribute	Definition
Module / Domain	PP / Manufacturing
Business Objective	Give production supervisors a near-real-time view of active production orders, work centers, planned quantity, confirmed quantity, remaining quantity, and operational status.
Primary Users	Production supervisor, shop-floor lead, manufacturing analyst
Suggested Deliverable	Reusable CDS model with an approved consumption channel

1. Business Requirement
Give production supervisors a near-real-time view of active production orders, work centers, planned quantity, confirmed quantity, remaining quantity, and operational status.
2. Functional Scope
•	Provide searchable and filterable business-ready data for the stated user group.
•	Support navigation from summarized information to relevant detail where relationships exist.
•	Expose semantic units, currencies, dates, texts, and status information.
•	Exclude write-back unless implemented separately through RAP or another approved transactional service.
3. Candidate Source Model
Candidate Source	Purpose
AUFK	Order master/header candidate
AFKO	Production order header data
AFPO	Production order item
AFVC	Operations
CRHD	Work center master
AFRU	Confirmations
MARA / MAKT	Material and text

4. Proposed Output Fields
#	Field	Role
1	ProductionOrder	Key
2	Plant	Key
3	Material	Attribute / measure
4	MaterialDescription	Attribute / measure
5	WorkCenter	Attribute / measure
6	Operation	Attribute / measure
7	BasicStartDate	Attribute / measure
8	BasicFinishDate	Attribute / measure
9	PlannedQuantity	Attribute / measure
10	ConfirmedYield	Attribute / measure
11	ScrapQuantity	Attribute / measure
12	RemainingQuantity	Attribute / measure
13	SystemStatus	Attribute / measure
14	DelayIndicator	Attribute / measure

5. Business Rules and Calculations
•	Remaining quantity = planned quantity minus confirmed yield, with unit consistency.
•	Expose one row per production order and operation, or explicitly aggregate to order/work-center level.
•	Determine active/cancelled/completed status from approved status logic, not from free-text interpretation.
•	Delay indicator must compare an agreed reference date with the planned finish date and consider completion status.
6. Proposed CDS Layering
Source tables / released standard CDS
        ↓
ZI_ProductionOrder
        ↓
ZI_ProductionOperation
        ↓
ZI_WorkCenter
        ↓
ZI_ProductionConfirmation
        ↓
ZI_ProductionStatus
        ↓
ZC_CProdStatus
Layer responsibilities: interface entities isolate source complexity; composite entities apply reusable relationships and calculations; consumption entities provide annotations, filters, analytics, and service-ready projection.
7. CDS Capabilities to Practice
Capability	Application
Associations	Represent navigable business relationships and avoid premature flattening.
Annotations	Add labels, semantics, value help, analytics, UI metadata, and authorization intent as applicable.
Calculated fields	Implement documented set-based calculations with correct type and null handling.
Parameters / filters	Use only when required by the consumption pattern; prefer flexible consumer filters for broad reuse.
DCL	Restrict organizational or sensitive data based on approved authorization design.
OData / service definition	Expose only the approved projection and associations when a UI or API is required.

8. Non-Functional Requirements
•	Performance: avoid SELECT * behavior, excessive joins, accidental Cartesian multiplication, and calculations in ABAP loops.
•	Security: enforce least privilege and validate row-level access with DCL where required.
•	Data quality: display an exception or quality status when required source information is unavailable.
•	Maintainability: document source ownership, rule ownership, annotations, cardinalities, and extension strategy.
•	Compatibility: validate table names, released CDS entities, data types, and annotations in the target release.
9. Acceptance Tests
•	An active order appears for its assigned plant and work center.
•	Confirmed yield and scrap reconcile with the approved confirmation source.
•	Completed or technically completed orders follow the agreed inclusion rule.
•	Quantities display with the correct unit.
•	Unauthorized plant data is not returned.
10. Out of Scope / Future Extensions
•	Transactional update behavior unless separately designed with RAP or an approved API.
•	Predictive scoring or machine-learning recommendations.
•	External data acquisition unless a governed integration is available.
•	Additional mobile/Fiori UX design beyond the consumption metadata blueprint.
