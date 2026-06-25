# Deployment Strategy Decision Exercise

For each scenario below, choose the best deployment strategy and write your justification.
Consider the constraints, team capabilities, and risk tolerance described.

---

## Scenario 1: E-Commerce Platform - Black Friday Preparation

**Company**: RetailMax, a mid-size e-commerce platform with 50K concurrent users during peak.

**Context**: The team needs to deploy a completely rewritten checkout service two weeks before Black Friday. The new service changes the payment flow and database schema. They have budget for extra infrastructure but cannot afford any customer-facing errors during the busiest sales period.

**Constraints**:
- Zero tolerance for customer-visible errors during checkout
- Database schema changes are backward-compatible
- Team has experience with AWS ALB and target groups
- Budget approved for temporary extra infrastructure
- Must be able to rollback within 30 seconds

**Your Choice**: _____________

**Your Justification**:




---

## Scenario 2: Internal Analytics Dashboard

**Company**: DataCorp, a B2B analytics company. The internal dashboard is used by 200 employees during business hours (9am-6pm EST).

**Context**: Deploying a major UI overhaul of the internal analytics dashboard. The app is stateless, runs on 4 Kubernetes pods, and the team wants to keep infrastructure costs minimal. Weekend deployments are acceptable if needed.

**Constraints**:
- Users are internal employees (not paying customers)
- Brief downtime during off-hours is acceptable
- Limited DevOps budget (startup with tight runway)
- No sophisticated traffic routing infrastructure
- Team of 3 developers, no dedicated SRE

**Your Choice**: _____________

**Your Justification**:




---

## Scenario 3: Mobile Banking API

**Company**: FinSecure Bank, serving 2M active mobile app users across multiple countries.

**Context**: Deploying a new fraud detection algorithm that changes how transaction risk scores are calculated. The algorithm has been tested extensively but real-world performance is unknown. Incorrect scoring could either block legitimate transactions (bad UX) or miss fraud (financial risk). Regulatory requirements mandate audit trails of all changes.

**Constraints**:
- Cannot risk incorrect fraud scores for all users simultaneously
- Need real-world performance data before full rollout
- Must maintain detailed audit logs of deployment progression
- Regulatory requirement: ability to prove controlled rollout
- 99.99% SLA requirement
- Sophisticated observability stack (Datadog, PagerDuty)

**Your Choice**: _____________

**Your Justification**:




---

## Scenario 4: Multi-Tenant SaaS - Legacy Monolith

**Company**: ProjectFlow, a project management SaaS with 500 enterprise customers.

**Context**: Migrating from a monolithic Node.js app to microservices. The first service being extracted (notification service) needs to be deployed. The monolith and new service must coexist during migration. Some tenants are on contracts requiring 24/7 availability, others are on free tier. The team wants to validate with free-tier tenants first.

**Constraints**:
- Different SLA tiers for different customer segments
- Need to validate with low-risk tenants before high-value ones
- Monolith and microservice must coexist (no big-bang cutover)
- Team has feature flag infrastructure (LaunchDarkly)
- Must be reversible at the tenant level
- Database is shared (multi-tenant PostgreSQL)

**Your Choice**: _____________

**Your Justification**:




---

## Answer Key

> Fold this section or cover it while working through the scenarios above.

### Scenario 1: Blue/Green Deployment

**Why**: The combination of zero error tolerance, budget availability, and the need for instant rollback makes Blue/Green the clear winner. The team can:
- Deploy the new checkout service to the "green" environment
- Run full integration tests against green without affecting production
- Switch ALB target groups instantly (< 30 second rollback)
- Keep the blue environment warm through Black Friday as insurance

**Why NOT others**:
- Canary: Too risky for a complete rewrite - even 1% of users hitting bugs during Black Friday is unacceptable
- Rolling: Rollback is too slow and having mixed versions of a checkout service with schema changes is dangerous
- Recreate: Downtime is absolutely unacceptable before Black Friday

---

### Scenario 2: Recreate (or Rolling Update)

**Why**: Given the internal-only audience, acceptable downtime window, budget constraints, and small team, a Recreate deployment during off-hours is the simplest approach. Alternatively, a Rolling Update via Kubernetes default strategy works since the app is stateless with multiple pods.

**The best practical choice**: Rolling Update via Kubernetes (it's the default, requires zero extra configuration, and with 4 pods provides reasonable availability). Deploy Friday evening for safety.

**Why NOT others**:
- Blue/Green: Over-engineered for an internal tool; doubles infrastructure cost for no business justification
- Canary: Requires traffic routing sophistication that this small team doesn't need for 200 internal users

---

### Scenario 3: Canary Deployment

**Why**: This is a textbook canary scenario. The team needs:
- Gradual rollout to validate real-world fraud detection accuracy
- Ability to compare metrics (false positive rate, detection rate) between old and new algorithms
- Documented, auditable progression for regulators
- Quick rollback if scoring degrades

**Rollout plan**: Start at 1% traffic, monitor for 24h, expand to 5%, 10%, 25%, 50%, 100% with automated rollback triggers on scoring anomalies.

**Why NOT others**:
- Blue/Green: All-or-nothing switch is too risky for an unproven algorithm
- Rolling: No ability to compare algorithm performance side-by-side
- Recreate: SLA violation from any downtime

---

### Scenario 4: Canary + Feature Flags (Hybrid approach)

**Why**: This scenario requires tenant-level routing control, which is best achieved by combining canary deployment with feature flags:
- Deploy the notification microservice alongside the monolith
- Use LaunchDarkly feature flags to route notification calls: monolith vs microservice
- Enable for free-tier tenants first (canary by customer segment)
- Monitor, validate, then progressively enable for enterprise tenants
- Instant per-tenant rollback by flipping the flag

**Why NOT others**:
- Pure Blue/Green: Doesn't support tenant-level granularity
- Pure Canary (traffic %): Need tenant-level control, not random percentage
- Rolling: Irrelevant when the architecture is about service routing, not instance replacement
- Recreate: SLA violations for enterprise customers
