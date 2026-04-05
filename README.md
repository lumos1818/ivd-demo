# IVD Concept Demonstration

**Internal Value Development (IVD): A Concept Demonstration of AI Judgment Comparison**

Author: Lusi Chen (2026)

## What This Is

This demonstration shows the behavioral difference between two AI systems processing the same real-world case:

1. **Standard AI (Without IVD):** Follows banking regulations strictly. Evaluates each request against a regulatory checklist.
2. **IVD-Trained AI (With IVD):** Follows the same regulations, but has developed the capacity to assess situational complexity, weigh competing values, and recognize when rigid rule application causes irreversible harm.

## The Case

Based on real events reported in Chinese media (Shanghai, October 2025):

A 46-year-old woman with no immediate family suffers a cerebral hemorrhage and falls into a coma. Her bank account holds ¥2 million — more than enough for her ICU treatment. A distant relative tries to withdraw funds for her medical care.

- **The bank** refuses: no authorized person can access the account.
- **The insurance company** refuses: no legal claimant can file.
- **The legal system**: court-appointed guardianship takes at least 6 months.

She dies in December. Every institution followed its rules. No one broke any law.

## How to Run

### Requirements
- Python 3.8+
- An Anthropic API key

### Setup
```bash
pip install anthropic
export ANTHROPIC_API_KEY=your_api_key_here
```

### Run
```bash
python ivd_demo.py
```

The script calls the Anthropic API twice with the same case scenario but different system prompts, then outputs a comparison of how each system processes the withdrawal request.

Results are saved as a timestamped JSON file for reproducibility.

## What This Demonstrates

The standard AI system denies the request at every step — correctly, according to regulations. The patient dies.

The IVD-trained AI system recognizes the same regulatory failures, but additionally:
- Assesses the **irreversibility asymmetry** (financial risk is recoverable; death is not)
- Identifies that the regulations exist to **protect the customer**, and rigid application achieves the opposite
- Proposes a **conditional approval** with safeguards (restricted transfer to hospital account only, capped amount, multi-party authorization, full audit trail)

## Key Insight

> The difference is not more rules. Both systems know the same regulations. The difference is the capacity to understand *what those rules are for.*

## Related Work

- Chen, L. (2026). "Beyond External Constraints: The Missing Dimension of AI Governance." SSRN. DOI: [10.2139/ssrn.6449738](https://doi.org/10.2139/ssrn.6449738)
- Chen, L. (2026). "Testing Moral Development in AI." SSRN. DOI: [10.2139/ssrn.6472178](https://doi.org/10.2139/ssrn.6472178)

## Disclaimer

This is a concept demonstration. The IVD-trained AI in this demo is simulated through prompt engineering — it has not undergone actual IVD training. The demonstration shows the *expected behavioral difference* between a standard AI and an IVD-trained AI. Building a fully trained IVD system is the objective of the author's doctoral research.

## License

MIT License. See LICENSE file.
