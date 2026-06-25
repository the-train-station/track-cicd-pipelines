# Deployment Strategies Comparison Matrix

Use this template to consolidate your understanding of each deployment strategy.
Fill in the table as you work through the lesson, then add your own notes below.

## Comparison Table

| Strategy | Traffic Shift Method | Rollback Speed | Infrastructure Cost | Complexity | Best For |
|----------|---------------------|----------------|---------------------|------------|----------|
| **Blue/Green** | Instant switch (DNS/LB cutover) | Seconds (switch back to old env) | High (2x environments) | Medium | Mission-critical apps requiring instant rollback |
| **Canary** | Gradual percentage-based routing | Fast (route 100% back to stable) | Medium (small canary pool) | High | High-traffic services needing risk validation |
| **Rolling** | Instance-by-instance replacement | Slow (must re-deploy previous version) | Low (reuses existing infra) | Low-Medium | Stateless services with many replicas |
| **Recreate** | Kill all old, start all new | Slow (full redeploy required) | Low (single environment) | Low | Dev/staging environments, or apps that cannot run multiple versions |

## Key Tradeoffs to Remember

- **Speed vs Safety**: Blue/Green gives fastest rollback but costs the most infrastructure.
- **Complexity vs Granularity**: Canary gives the finest control but requires sophisticated routing and monitoring.
- **Cost vs Downtime**: Recreate is cheapest but has a downtime window during deployment.
- **Stateful vs Stateless**: Rolling updates work poorly with stateful applications that cannot handle mixed versions.

## Your Notes

### Blue/Green Observations
<!-- What surprised you? When would you NOT use this? -->


### Canary Observations
<!-- What metrics would you watch? What percentage would you start with? -->


### Rolling Observations
<!-- How does this interact with database migrations? -->


### Recreate Observations
<!-- When is downtime actually acceptable? -->


## Additional Strategies Worth Knowing

| Strategy | Description | When to Consider |
|----------|-------------|-----------------|
| **A/B Testing** | Route by user segment, not percentage | Feature validation with specific cohorts |
| **Shadow/Dark** | Duplicate traffic to new version without serving responses | Testing under real load without user impact |
| **Feature Flags** | Deploy code but toggle features independently | Decoupling deploy from release |

<!-- Add any other strategies you discover here -->
