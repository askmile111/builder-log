# Instant Pricing: How to Quote When a Client Asks for New Features on the Spot

_Day 001 — 2026-06-27_
## What I observed
The client says "add login" which feels like changing one line of code, but actually involves database tables, authentication logic, frontend state management, and testing. Every time, I mentally estimate the hours, afraid that over-quoting will lose the deal and under-quoting will cost me. In the end, I usually say yes verbally, then work overtime for free.
## My current hypothesis
Any new requirement can be broken down into three quantifiable dimensions:
- New data entities (tables/fields/APIs)
- New interaction interfaces (pages/components/state)
- New integration points (third-parties/notifications/emails)
Define the "Minimum Quotable Unit (MQU)":  
One MQU = 0.5 days of development + 0.25 days of testing + 0.1 days of communication = 0.85 days ≈ 1 day (rounded up).  
Then, for each small request from the client, I just count the number of MQUs, multiply by my daily rate, and add a 20% buffer (because the client will inevitably tweak it further). That becomes the quote.
## Current version model (pseudocode)
```python
def quote_change(request_text):
    # Simple keyword counting (demo version)
    entities = len(re.findall(r'(table|field|API|interface|database)', request_text))
    screens = len(re.findall(r'(page|popup|form|list|dashboard)', request_text))
    integrations = len(re.findall(r'(email|sms|payment|third-party|login|auth)', request_text))
    mqu = entities * 0.5 + screens * 0.8 + integrations * 0.6
    days = max(1, round(mqu))
    price = days * daily_rate * 1.2
    return f"Extra timeline: {days} days, Extra cost: ${price}"

_(Note: This is a rough heuristic and needs calibration with historical data.)_

## Open questions

- How to automatically extract these keywords from the conversation instead of copying and pasting manually?
    
- If the client says "we might change it later," should I offer a bundled discount upfront?
    
- How can I reduce the back-and-forth emails in the approval workflow after quoting?