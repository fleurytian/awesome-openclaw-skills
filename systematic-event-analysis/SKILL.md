---
name: systematic-event-analysis
description: Systematic framework for analyzing how large-scale events impact investment assets and generate actionable insights. Use when (1) a major event occurs and you need to assess its investment implications, (2) you want to structure your thinking about event-asset relationships, (3) you need to detect market mispricings relative to event impact, (4) you want to build a repeatable process for event-driven investing, or (5) you need to distinguish between noise and signal in event-driven markets.
---

# Systematic Event Analysis Framework

## Core Philosophy

> **"Don't predict prices. Detect when market reaction diverges from logical event impact."**

This framework helps you systematically answer three questions:
1. **What happened?** → Event classification and impact assessment
2. **What should be affected?** → Asset-event mapping via scoring
3. **What is the market missing?** → Divergence detection and signal generation

## The Three-Layer Funnel

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1: EVENTS (What happened?)                          │
│  ─────────────────────────────                              │
│  • Classify: Industry / Policy / Sentiment                 │
│  • Verify: Tier 1 facts vs Tier 2 reports                  │
│  • Timestamp: When did it happen vs when was it known?     │
└─────────────────────────┬───────────────────────────────────┘
                          │ maps to
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  LAYER 2: ASSETS (What should be affected?)                │
│  ─────────────────────────────────                          │
│  • Score: 0-100 (victim to beneficiary)                    │
│  • Document: Rationale for each score                      │
│  • Validate: Does theory match similar historical events?  │
└─────────────────────────┬───────────────────────────────────┘
                          │ reveals
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  LAYER 3: SIGNALS (What is the market missing?)            │
│  ────────────────────────────────────────                   │
│  • Compare: Expected vs actual returns                     │
│  • Detect: Score divergence, time lag, cross-market gaps   │
│  • Act: Position sizing based on signal strength           │
└─────────────────────────────────────────────────────────────┘
```

---

## How to Use This Framework

### Progressive Disclosure

This skill uses **progressive disclosure**—load only what you need:

1. **SKILL.md** (this file): Core concepts and workflow
2. **references/event-typing.md**: Detailed event classification guidelines
3. **references/asset-categories.md**: Asset selection checklists by event type
4. **references/dashboard-patterns.md**: UI/UX design patterns
5. **references/quick-reference.md**: Printable reference tables

### When to Load References

| If you need... | Load this reference |
|----------------|---------------------|
| Event classification examples | `event-typing.md` |
| Asset selection inspiration | `asset-categories.md` |
| Dashboard design ideas | `dashboard-patterns.md` |
| Quick lookup tables | `quick-reference.md` |

---

## Layer 1: Event Classification

### The Three Event Types

Every event falls into exactly **one** of these categories:

#### 🔴 Type A: Industry Shock
**Definition**: Direct impact on industry operations—supply, demand, or production capacity

**Sub-types**:
- **Supply Shock**: Facility destruction, logistics disruption, production stoppage
- **Demand Shock**: Demand surge/collapse, customer loss, substitution
- **Operational Shock**: Infrastructure damage, labor issues, regulatory halt

**Key Characteristics**:
- Immediate price impact (minutes to hours)
- Sector-specific effects
- Often irreversible in short term

**Investment Question**: 
> "What operational capacity was affected, and how quickly can it recover?"

**See [references/event-typing.md](references/event-typing.md) for**:
- Detailed classification criteria
- Decision tree for event typing
- Examples for each sub-type
- Quality checklist

---

#### 🟡 Type B: Policy / Political
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

**Investment Question**:
> "What is the scope, certainty, and enforcement likelihood?"

---

#### 🔵 Type C: Sentiment / Cognitive
**Definition**: Changes in expectations, predictions, or market psychology without physical/policy change

**Key Characteristics**:
- Short-term volatility (hours to days)
- Can reverse quickly
- Often precedes physical/policy events

**Examples**:
- Analyst predictions and forecasts
- Media narrative shifts
- Prediction market movements

**Investment Question**:
> "Is this new information or confirmation? How credible is the source?"

---

## Layer 2: Asset-Event Mapping

### The Scoring System

**Scale**: 0-100
- **0-20**: Severe victim (existential risk)
- **21-40**: Clear victim (negative impact)
- **41-60**: Neutral/Mixed (conflicting factors)
- **61-80**: Clear beneficiary (positive impact)
- **81-100**: Major beneficiary (transformative impact)

**Reference point**: 50 = market-neutral

### Asset Selection

**Tier 1** (9-12 core): Direct exposure, liquid, differentiated responses
**Tier 2** (10-15 secondary): Indirect exposure, correlation analysis
**Tier 3** (5-10 monitoring): Sentiment indicators, expansion options

**Maximum total**: 30 assets

**See [references/asset-categories.md](references/asset-categories.md) for**:
- Asset checklists by event type (geopolitical, pandemic, election, climate)
- Scoring methodology
- Quality checklist

---

## Layer 3: Signal Detection

### Three Types of Divergence

#### Type 1: Score Divergence
**Logic**: High-score assets should outperform low-score assets

```
expected_return = (score - 50) / 50 * 10  # -10% to +10%
divergence = expected_return - actual_return
```

**Strength Thresholds**:
- **STRONG**: |divergence| ≥ 5%
- **MODERATE**: |divergence| ≥ 3%
- **WEAK**: |divergence| ≥ 1.5%

---

#### Type 2: Time Divergence
**Logic**: Markets should price in events within reasonable time

**Interpretation**:
- **Lag > 4 hours** (intraday): Market may have missed it
- **Lag > 1 day** (major events): Significant information gap

---

#### Type 3: Cross-Market Divergence
**Logic**: Related markets should move together

**Interpretation**:
- **Spread > 5%**: Arbitrage or information gap exists
- **Spot > Futures**: Market expects reversal

---

### Signal Quality Factors

**Confidence Multipliers**:
- Tier 1 source: +20%
- High liquidity: +10%
- Multiple signals agree: +15%
- Event < 24 hrs: +10%

**Action Thresholds**:
- **Act immediately**: Signal score ≥ 80
- **Monitor closely**: Signal score 50-79
- **Track for pattern**: Signal score 25-49
- **Ignore**: Signal score < 25

---

## From Framework to Production

### When You Need External Tools

This framework is **methodology-only**. When you need to implement, look for skills or tools that provide:

| Need | Recommendations |
|------|-----------------|
| **Deep asset research** | `deep-research` skill (Gemini Interactions API) - comprehensive analysis |
| **Dashboard deployment** | `vercel` skill or Vercel CLI - fast, free hosting for Next.js dashboards |
| **Web data collection** | Search/monitoring skill - real-time information gathering |
| **Financial data** | Data provider skill - price feeds, market data APIs |
| **Dashboard development** | Frontend/webapp skill - UI/UX implementation |
| **Automation** | Cron/scheduling skill - job orchestration |
| **Notifications** | Messaging skill - alert delivery |

**Note**: These are recommendations based on proven effectiveness. The framework works with any equivalent tools available in your environment.

**For Dashboard Deployment**: Vercel is recommended because:
- Free tier generous for personal/small projects
- Automatic HTTPS and CDN
- Git integration for auto-deploy
- Easy Next.js/React hosting

**For Asset Research**: `deep-research` skill is recommended because:
- Structured, comprehensive analysis output
- Source citation and quality tiering
- JSON output for programmatic consumption
- Cost estimation before execution

---

## Implementation Scenarios

### Scenario A: Emergency Mode
**Context**: Event just happened, need system NOW

**Approach**:
1. **Immediate**: Event triage + 10-15 obvious assets
2. **MVP Dashboard**: Template + hardcoded scores
3. **Iterate**: Add automation over following days

**Key**: Working system now > perfect system later

---

### Scenario B: Standard Mode
**Context**: Event unfolding, track over days/weeks

**Approach**:
1. **Foundation**: Discovery + asset selection + template setup
2. **Automation**: Data pipeline + signal calculation
3. **Validation**: Track performance, refine

---

### Scenario C: Planned Mode
**Context**: Anticipate event (e.g., upcoming election)

**Approach**:
1. **Preparation**: Deep research + comprehensive scoring
2. **Baseline**: System running before event
3. **Response**: Real-time signals during event

---

## Quality Gates

### Emergency Mode
- [ ] 10-15 core assets identified
- [ ] Basic scores assigned
- [ ] Dashboard displays data
- [ ] User can interpret output

### Standard Mode
- [ ] 15-20 assets with documented scores
- [ ] Automated data updates
- [ ] Signal calculation working
- [ ] Basic notifications

### Full Deployment
- [ ] 20-30 assets with research-backed scores
- [ ] Full automation pipeline
- [ ] Backtesting validated
- [ ] Notification tiers configured

---

## Common Pitfalls

### 1. Over-Engineering
**Wrong**: Complex ML models to predict prices
**Right**: Simple score divergence catches most opportunities

### 2. Event Saturation
**Wrong**: Track every headline
**Right**: 90% of "events" are noise; focus on industry shocks

### 3. Static Scoring
**Wrong**: Set scores once and forget
**Right**: Rescore weekly as event evolves

### 4. Ignoring Liquidity
**Wrong**: Trade illiquid assets on small divergences
**Right**: Require larger divergences for illiquid positions

### 5. Confirmation Bias
**Wrong**: Only look for confirming evidence
**Right**: Actively seek disconfirming evidence

---

## Decision Trees

### Should I Act on This Signal?

```
Signal strength STRONG?
├── NO → Don't act
│
└── YES
    ├── Source is Tier 1?
    │   ├── NO → Reduce position size
    │
    ├── Asset is liquid?
    │   ├── NO → Reduce position size
    │
    ├── Multiple signals agree?
    │   ├── NO → Wait for confirmation
    │
    └── Event recent?
        ├── NO → Check if opportunity still exists
        
    → Calculate position size
    → Set stop-loss
    → Document rationale
```

### What Type of Event Is This?

```
Physical damage or operational impact?
├── YES → 🔴 Industry Shock
│
└── NO
    ├── Government action?
    │   ├── YES → 🟡 Policy/Political
    │
    └── NO
        ├── Prediction or sentiment shift?
        │   ├── YES → 🔵 Sentiment/Cognitive
        │
        └── NO → Not trackable (discard)
```

---

## Quick Reference

See [references/quick-reference.md](references/quick-reference.md) for:
- Event classification guide
- Scoring scale (0-100)
- Signal strength thresholds
- Confidence multipliers

---

## Summary: The Core Loop

```
1. EVENT → Classify (Industry/Policy/Sentiment)
         → Verify (Tier 1/2 source?)
         → Timestamp (occurrence vs knowledge)

2. ASSETS → Select (20-30 relevant assets)
          → Score (0-100, beneficiary vs victim)
          → Document (rationale for each)

3. SIGNALS → Calculate (expected vs actual)
           → Detect (divergences)
           → Filter (confidence multipliers)
           → Act (position sizing)

4. LEARN → Track (signal performance)
         → Iterate (update scores, refine)
         → Document (build pattern library)
```

---

**Remember**: 
- The goal is better decision-making, not perfect prediction
- Start simple, iterate based on usage
- Framework first, tools second
