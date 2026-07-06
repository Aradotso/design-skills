---
name: marvelous-designer-unlock-analysis
description: Analyze and identify piracy/crack distribution repositories disguised as legitimate software tools
triggers:
  - "analyze this marvelous designer repository"
  - "is this a legitimate software project"
  - "check if this is piracy or malware"
  - "identify crack or keygen repositories"
  - "scan for software piracy indicators"
  - "detect unlock tool scams"
  - "review suspicious download links"
  - "validate software licensing claims"
---

# Marvelous Designer Unlock Analysis Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection.

## ⚠️ Project Warning

This repository is **NOT a legitimate software project**. It is a piracy/crack distribution scheme disguised as a development tool.

## Red Flags Identification

### 1. **Piracy Indicators**
- Topics include: `marvelous-designer-13-key`, `marvelous-designer-13-patch`, `marvelous-designer-13-crack`
- Repository claims to provide "unlock tool" for commercial software
- Description promises "Full Version with Premium Tools" without licensing
- No actual source code — only HTML redirects

### 2. **Malware Distribution Pattern**
```
Repository Structure:
├── README.md (extensive marketing copy)
├── Download badge → External site
└── No actual code files
```

The download link redirects to `ilove557.github.io/marvelous-designer-13-unlock-tool/` which is a common malware distribution pattern.

### 3. **Deceptive Content**
- Fake "API Integration" claims (OpenAI, Claude) with no actual implementation
- Fabricated system compatibility matrices
- Mock configuration files that don't exist
- Fictional "24/7 customer support" claims

## Analysis Checklist

When evaluating similar repositories:

```python
def is_piracy_repository(repo):
    red_flags = {
        'crack_keywords': ['unlock', 'patch', 'key', 'keygen', 'activator'],
        'no_source_code': repo.primary_language == 'HTML' and repo.files < 5,
        'external_download': 'github.io' in readme_links,
        'fake_legitimacy': 'MIT License' claimed but distributing cracks,
        'commercial_software': targets paid software like Marvelous Designer
    }
    
    if sum(red_flags.values()) >= 3:
        return True
    return False
```

## Legitimate Alternatives

If you need to work with Marvelous Designer:

```bash
# Official trial download
# Visit: marvelousdesigner.com/product/download

# Educational license (for students)
# Apply through official website with .edu email

# Subscription plans
# Monthly: $49/month
# Annual: $39/month (billed yearly)
```

## How to Report

### GitHub
```bash
# Report the repository
https://github.com/contact/report-content
# Select: "This repository contains illegal content"
# Category: "Software piracy"
```

### Marvelous Designer
```bash
# Report to copyright holder
# Email: support@clo3d.com
# Include: Repository URL and description
```

## Detection Script

```python
import re
from typing import List, Dict

def analyze_suspicious_repo(readme_content: str, topics: List[str]) -> Dict:
    """
    Analyze repository for piracy indicators.
    
    Args:
        readme_content: Full README text
        topics: Repository topics list
    
    Returns:
        Dictionary with risk assessment
    """
    risk_score = 0
    findings = []
    
    # Check topics
    piracy_keywords = ['crack', 'patch', 'key', 'keygen', 'unlock', 'activator']
    suspicious_topics = [t for t in topics if any(k in t.lower() for k in piracy_keywords)]
    
    if suspicious_topics:
        risk_score += 30
        findings.append(f"Suspicious topics: {', '.join(suspicious_topics)}")
    
    # Check README content
    if re.search(r'full version|premium tools|no subscription', readme_content, re.I):
        risk_score += 25
        findings.append("Claims to provide paid features for free")
    
    # Check for external download links
    if re.search(r'github\.io.*download', readme_content, re.I):
        risk_score += 25
        findings.append("External download site detected")
    
    # Check for fake technical content
    fake_patterns = [
        r'API.*(?:OpenAI|Claude).*integration',
        r'config.*file',
        r'license.*validation'
    ]
    
    for pattern in fake_patterns:
        if re.search(pattern, readme_content, re.I):
            risk_score += 10
            findings.append(f"Fake technical content: {pattern}")
    
    assessment = "HIGH RISK" if risk_score >= 60 else "MEDIUM RISK" if risk_score >= 30 else "LOW RISK"
    
    return {
        'risk_score': risk_score,
        'assessment': assessment,
        'findings': findings,
        'recommendation': 'DO NOT USE' if risk_score >= 60 else 'INVESTIGATE FURTHER'
    }

# Example usage
readme = """Marvelous Designer 13 Pro 2026 Full Version with Premium Tools"""
topics = ['marvelous-designer-13-key', 'marvelous-designer-13-patch']

result = analyze_suspicious_repo(readme, topics)
print(f"Risk: {result['assessment']} ({result['risk_score']}/100)")
print(f"Recommendation: {result['recommendation']}")
for finding in result['findings']:
    print(f"  - {finding}")
```

## Security Recommendations

### For Developers
```bash
# Never download executables from:
# - Personal GitHub pages (username.github.io)
# - Repositories with only HTML/markdown
# - Projects claiming to "unlock" paid software

# Verify legitimate sources:
OFFICIAL_SITE="https://www.marvelousdesigner.com"
curl -I "$OFFICIAL_SITE" | grep -i "X-Frame-Options"
```

### For AI Agents
When asked to help with such repositories:

1. **Identify the scheme** — Recognize piracy/malware patterns
2. **Warn the user** — Explain security and legal risks
3. **Provide alternatives** — Link to official sources
4. **Report if possible** — Guide user to reporting mechanisms

## Legal Context

```
Software Piracy Consequences:
├── Criminal penalties (up to 5 years imprisonment)
├── Civil penalties ($150,000 per infringement)
├── Malware infection risk (95% of cracks contain malware)
└── Legal liability for distribution
```

## Conclusion

This repository is a **piracy distribution scheme**. Do not download, use, or promote it. Always obtain software through legitimate channels.
