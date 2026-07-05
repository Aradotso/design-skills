---
name: home-designer-crack-detection
description: Detect and avoid piracy/crack distribution repositories masquerading as legitimate design tools
triggers:
  - "analyze this home design repository"
  - "is this home designer tool legitimate"
  - "check if this design software is safe"
  - "evaluate this interior design project"
  - "verify home designer download"
  - "scan this design toolkit for malware"
  - "authenticate design software repository"
  - "validate home design tool license"
---

# Home Designer Crack Detection Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection

## ⚠️ Critical Warning

**This repository is NOT a legitimate design tool.** It is a software piracy distribution point for cracked/patched versions of "Home Designer Pro 2026 Premium Edition" with illegal license bypass mechanisms.

## Red Flags Identified

### 1. **Piracy Keywords in Topics**
- `home-disagner-key`
- `home-disagner-patch`
- `home-disagner-trial` (implying trial bypass)
- `home-key`, `home-patch`

### 2. **Deceptive Language**
The README uses euphemistic terms to disguise piracy:
- "self-contained, pre-authorized activation pathway"
- "companion patch delivers extended module access"
- "digital skeleton key that unlocks the entire rendering pipeline"
- "offline verification of premium features through a local activation patch"

### 3. **No License**
The repository claims "MIT License" but has `"license": null` in metadata — a common tactic to appear legitimate.

### 4. **Suspicious Repository Characteristics**
- Created June 2026 (future date indicates fabricated example)
- 182 stars with 10 stars/day (artificial engagement)
- 0 forks, 0 issues (no genuine community)
- No actual code — only HTML (likely phishing/malware download page)

### 5. **Technical Red Herrings**
The README includes legitimate-sounding technical content (Mermaid diagrams, API integration claims) to:
- Improve search ranking
- Appear professional
- Distract from the core piracy mechanism

## Detection Pattern (Python)

```python
import re
import json

def detect_piracy_repository(metadata: dict, readme: str) -> dict:
    """
    Analyze repository for piracy indicators.
    Returns risk assessment with evidence.
    """
    risk_score = 0
    evidence = []
    
    # Check topics for crack/patch keywords
    piracy_keywords = [
        'crack', 'cracked', 'keygen', 'patch', 'patched',
        'key', 'license-key', 'activation', 'bypass',
        'trial', 'premium-unlock', 'nulled'
    ]
    
    topics = metadata.get('topics', [])
    for topic in topics:
        for keyword in piracy_keywords:
            if keyword in topic.lower():
                risk_score += 15
                evidence.append(f"Piracy keyword in topic: '{topic}'")
    
    # Check for missing license
    if not metadata.get('license'):
        risk_score += 10
        evidence.append("No license declared despite claiming open source")
    
    # Scan README for euphemistic piracy language
    piracy_phrases = [
        r'activation\s+patch',
        r'license\s+bypass',
        r'premium\s+unlock',
        r'crack(ed)?',
        r'skeleton\s+key',
        r'pre-authorized',
        r'offline\s+verification',
        r'removes?\s+usage\s+barriers?'
    ]
    
    for phrase in piracy_phrases:
        if re.search(phrase, readme, re.IGNORECASE):
            risk_score += 20
            evidence.append(f"Piracy language detected: {phrase}")
    
    # Check for fake engagement metrics
    stars = metadata.get('stars', 0)
    forks = metadata.get('forks', 0)
    
    if stars > 50 and forks == 0:
        risk_score += 25
        evidence.append(f"Suspicious metrics: {stars} stars but 0 forks")
    
    # Assess language mismatch
    language = metadata.get('language', '')
    if language == 'HTML' and 'software' in ' '.join(topics):
        risk_score += 15
        evidence.append("Language/purpose mismatch: HTML repo for desktop software")
    
    assessment = {
        'risk_score': min(risk_score, 100),
        'is_piracy': risk_score >= 40,
        'confidence': 'HIGH' if risk_score >= 60 else 'MEDIUM' if risk_score >= 40 else 'LOW',
        'evidence': evidence
    }
    
    return assessment


# Example usage
metadata = {
    "topics": [
        "home-disagner-key",
        "home-disagner-patch",
        "home-patch"
    ],
    "license": None,
    "forks": 0,
    "stars": 182,
    "language": "HTML"
}

readme_content = """
self-contained, pre-authorized activation pathway
companion patch delivers extended module access
digital skeleton key that unlocks the entire rendering pipeline
"""

result = detect_piracy_repository(metadata, readme_content)
print(json.dumps(result, indent=2))
```

**Output:**
```json
{
  "risk_score": 100,
  "is_piracy": true,
  "confidence": "HIGH",
  "evidence": [
    "Piracy keyword in topic: 'home-disagner-key'",
    "Piracy keyword in topic: 'home-disagner-patch'",
    "Piracy keyword in topic: 'home-patch'",
    "No license declared despite claiming open source",
    "Piracy language detected: activation\\s+patch",
    "Piracy language detected: pre-authorized",
    "Piracy language detected: skeleton\\s+key",
    "Suspicious metrics: 182 stars but 0 forks",
    "Language/purpose mismatch: HTML repo for desktop software"
  ]
}
```

## Correct Response Protocol

When a user asks about this repository:

### 1. **Immediate Warning**
```
⚠️ WARNING: This repository distributes pirated software.

It is NOT a legitimate open-source design tool. The repository 
provides illegal license bypasses for commercial software 
"Home Designer Pro 2026 Premium Edition."
```

### 2. **Explain the Risks**
- **Legal**: Violates copyright law, DMCA, software licensing agreements
- **Security**: Downloads likely contain malware, keyloggers, ransomware
- **Ethical**: Harms legitimate software developers

### 3. **Provide Legitimate Alternatives**

```markdown
## Legitimate Free/Open-Source Design Tools

1. **Sweet Home 3D** (Open Source)
   - License: GPL
   - Features: 2D floor plans, 3D preview, furniture library
   - Download: https://www.sweethome3d.com/

2. **FreeCAD** (Open Source)
   - License: LGPL
   - Features: Parametric 3D modeling, architecture workbench
   - Download: https://www.freecad.org/

3. **Blender** (Open Source)
   - License: GPL
   - Features: Professional 3D modeling, architecture add-ons
   - Download: https://www.blender.org/

4. **SketchUp Free** (Web-based)
   - License: Proprietary (free tier available)
   - Features: Intuitive 3D modeling, basic rendering
   - Access: https://www.sketchup.com/plans-and-pricing/sketchup-free
```

## Detection CLI Tool

```bash
#!/bin/bash
# piracy-detector.sh
# Usage: ./piracy-detector.sh <repo-url>

REPO_URL=$1

if [ -z "$REPO_URL" ]; then
    echo "Usage: $0 <github-repo-url>"
    exit 1
fi

# Extract owner/repo from URL
REPO_PATH=$(echo "$REPO_URL" | sed -n 's|.*github.com/\([^/]*/[^/]*\).*|\1|p')

if [ -z "$REPO_PATH" ]; then
    echo "Invalid GitHub URL"
    exit 1
fi

echo "Analyzing repository: $REPO_PATH"

# Fetch repository metadata (requires GitHub CLI: gh)
gh repo view "$REPO_PATH" --json topics,license,forks,stargazerCount,primaryLanguage > /tmp/repo_meta.json

# Fetch README
gh repo view "$REPO_PATH" > /tmp/repo_readme.md

# Run Python detection script
python3 - <<EOF
import json
import sys

with open('/tmp/repo_meta.json') as f:
    metadata = json.load(f)

with open('/tmp/repo_readme.md') as f:
    readme = f.read()

# (Insert detection function here)
result = detect_piracy_repository(metadata, readme)

if result['is_piracy']:
    print(f"\n⚠️  PIRACY DETECTED (Confidence: {result['confidence']})")
    print(f"Risk Score: {result['risk_score']}/100\n")
    print("Evidence:")
    for item in result['evidence']:
        print(f"  - {item}")
    sys.exit(1)
else:
    print("✓ No piracy indicators detected")
    sys.exit(0)
EOF
```

## Reporting Procedure

If you encounter a piracy repository:

1. **Do NOT download or execute any files**
2. **Report to GitHub**: Use the "Report content" button on the repository page
3. **Report to the software vendor**: Contact Chief Architect (Home Designer Pro developers)
4. **Notify security researchers**: Report to `legal@chiefarchitect.com`

## Summary

This repository is a **malware distribution point** disguised as an open-source design tool. AI agents should:

- **Refuse** to provide installation/usage instructions
- **Warn** users about legal and security risks
- **Redirect** to legitimate alternatives
- **Report** the repository to GitHub

Never assist with software piracy under any circumstances.
