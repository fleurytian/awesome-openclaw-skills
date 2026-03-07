# Event Typing Guidelines

## Detailed Classification Criteria

### 🔴 Type A: Industry Shock

**Definition**: Direct impact on industry operations, affecting supply, demand, or production capacity

**Key Characteristics**:
- Immediate price impact (minutes to hours)
- Sector-specific effects
- Often irreversible in short term

#### Sub-type A1: Supply Shock

**Examples**:
- Facility destruction (refinery explosion, factory fire)
- Logistics disruption (port closure, canal blockage)
- Production stoppage (mine accident, crop failure)
- Supply chain breakage (key component shortage)

**Investment Impact**:
- Direct and immediate price impact
- Typically affects commodities and industrial sectors
- Can create arbitrage opportunities between markets

#### Sub-type A2: Demand Shock

**Examples**:
- Sudden demand surge (pandemic hoarding, panic buying)
- Demand collapse (travel bans, lockdowns)
- Customer base loss (major client bankruptcy)
- Substitute emergence (new tech disrupting demand)

**Investment Impact**:
- Rapid price adjustments
- Can reverse quickly if shock is temporary
- Often affects consumer-facing sectors most

#### Sub-type A3: Operational Shock

**Examples**:
- Infrastructure damage (power grid, transport networks)
- Labor disruptions (strikes, shortage)
- Regulatory halt (safety shutdowns, environmental stops)

**Investment Impact**:
- Affects operational leverage
- Recovery timeline critical for valuation

---

### 🟡 Type B: Policy / Political

**Definition**: Government action or political development changing operating environment

**Key Characteristics**:
- Medium-term impact (days to weeks)
- Affects sectors through regulatory changes
- Can be reversed or modified

**Examples**:
- Sanctions and trade restrictions
- Tax policy changes
- Regulatory approvals/rejections
- Elections and regime changes
- Military mobilization orders
- Treaty signings or withdrawals

**Investment Impact**:
- Medium-term sector rotation
- Currency impacts
- Compliance cost changes
- Market access changes

**Scoring Criteria**:
- Certainty: Announced vs proposed
- Scope: Targeted vs broad
- Enforcement: Likely vs symbolic

---

### 🔵 Type C: Sentiment / Cognitive

**Definition**: Changes in market perception, expert predictions, or information that alters expectations without immediate physical or policy impact

**Key Characteristics**:
- Short-term volatility (hours to days)
- Can be reversed quickly
- Often precedes physical/policy events

**Examples**:
- Analyst predictions and forecasts
- Media narrative shifts
- Expert assessments (intelligence, medical, scientific)
- Prediction market movements
- Social media sentiment shifts
- Conference presentations

**Investment Impact**:
- Short-term volatility
- Positioning changes
- Can be reversed quickly
- Often precedes physical/policy events

**Scoring Criteria**:
- Credibility: Source reputation
- Consensus: Agreement among experts
- Novelty: New information vs confirmation

---

## Event Classification Decision Tree

```
Physical damage or operational impact?
├── YES → 🔴 Industry Shock (Supply/Demand/Operational)
│
└── NO
    ├── Government action or policy change?
    │   ├── YES → 🟡 Policy/Political
    │
    └── NO
        ├── Prediction, forecast, or sentiment shift?
        │   ├── YES → 🔵 Sentiment/Cognitive
        │
        └── NO → Not trackable (discard)
```

---

## What NOT to Track

**DO NOT track these as events**:

1. **Price movements alone**
   - "Oil up 5%" is not an event
   - Use price data as context, not event

2. **Unverified rumors**
   - Single-source social media claims
   - Anonymous reports

3. **Background analysis**
   - Historical context pieces
   - General trend discussions

4. **Routine operations**
   - Scheduled maintenance
   - Normal seasonal patterns

5. **Second-order effects**
   - "Because oil is up, airline stocks fell"
   - These are captured by asset price tracking

---

## Event Quality Checklist

Before treating something as a meaningful event, verify:

- [ ] **First-order effect?** Not a consequence of another event
- [ ] **Tier 1 or Tier 2 source?** Facts or credible reports, not rumors
- [ ] **Specific affected assets identifiable?** Can name 3+ assets
- [ ] **Timestamp accuracy?** Known within 4-hour window
- [ ] **Would change supply/demand or risk premium?** Not just background noise

**If NO to any → Discard as noise**

---

## Event Entry Format

Each event should include:

```json
{
  "id": "unique-identifier",
  "timestamp": "2026-03-07T14:30:00Z",
  "event_time": "2026-03-07T12:00:00Z",
  "type": "industry_shock|policy|sentiment",
  "sub_type": "supply|demand|operational",
  "title": "Brief description",
  "description": "Full details",
  "source_tier": 1|2|3,
  "sources": ["url1", "url2"],
  "affected_assets": ["BRENT", "XLE", "USD_IRR"],
  "impact_severity": 1-10,
  "confidence": "high|medium|low",
  "status": "confirmed|probable|rumor"
}
```

**Critical Fields**:
- `timestamp`: When we recorded the event
- `event_time`: When the event actually occurred (for causality analysis)
- `source_tier`: 1=fact, 2=report, 3=analysis
- `status`: Only `confirmed` events trigger trading signals
