# LLM Credit Balance Monitoring System

Early warning system for monitoring LLM API credit balances to prevent circuit breaker trips and task failures due to insufficient funds.

## Problem Statement

Raph has experienced multiple circuit breaker trips due to insufficient credits across multiple LLM providers:
- Kimi K2.5
- or-deepseek-v3
- or-gemini-flash
- or-claude-haiku

## Solution Architecture

### 1. Credit Monitoring Service (`credit-monitor.js`)
- Polls each LLM provider's credit/balance API every 30 minutes
- Tracks credit consumption trends and burn rates
- Calculates remaining days of operation based on usage patterns
- Stores historical data for analysis

### 2. Alerting System
- **Warning Threshold**: 20% credits remaining or < 48 hours of runtime
- **Critical Threshold**: 5% credits remaining or < 12 hours of runtime
- **Emergency Threshold**: < 2% credits remaining

### 3. Notification Channels
- Discord #dev-updates for warnings
- Discord #sentinel-alerts for critical alerts
- Direct notification to Islem for emergency situations
- Automatic downgrade to lower-cost models when credits are low

### 4. Circuit Breaker Integration
- Prevents tasks from being queued when credits are critically low
- Automatically switches to fallback providers when primary is exhausted
- Graceful degradation of service rather than abrupt failure

## Implementation Plan

### Phase 1: Basic Monitoring (Current)
- Create monitoring service with API integration
- Implement alert logic
- Set up Discord notifications

### Phase 2: Predictive Analytics (Future)
- Machine learning for usage pattern prediction
- Auto-recommendation for credit purchase timing
- Cost optimization suggestions

### Phase 3: Automated Remediation (Future)
- Auto-purchase credits via API (with Islem approval)
- Dynamic model switching based on cost/performance
- Budget allocation optimization

## Supported LLM Providers

| Provider | API Endpoint | Credit Unit | Alert Thresholds |
|----------|-------------|-------------|-----------------|
| Kimi K2.5 | `https://api.moonshot.cn/v1/balance` | tokens | 20K tokens |
| or-deepseek-v3 | `https://api.deepseek.com/v1/balance` | tokens | 50K tokens |
| or-gemini-flash | Google Cloud Billing API | USD | $50 remaining |
| or-claude-haiku | Anthropic Billing API | USD | $50 remaining |

## Integration Points

1. **Task Executor**: Check credits before executing LLM-dependent tasks
2. **Agent Brain**: Monitor overall system credit health
3. **Sentinel**: Critical alert routing and validation
4. **Health Monitor**: Include credit status in system health checks

## Files to Implement

- `services/llm-credit-monitor.js` - Main monitoring service
- `config/llm-providers.json` - Provider configuration
- `scripts/test-credit-monitor.js` - Testing script
- `docs/CREDIT-MONITORING-GUIDE.md` - Operational guide