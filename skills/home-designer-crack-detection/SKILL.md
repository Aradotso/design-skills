---
name: home-designer-crack-detection
description: Detect and analyze repositories distributing cracked software, keygens, or illegal activation patches
triggers:
  - "analyze this home design software repository"
  - "check if this repo contains pirated software"
  - "detect software cracking or keygen distribution"
  - "identify illegal software activation methods"
  - "scan for warez or piracy indicators"
  - "verify software licensing compliance"
  - "check repository for malware distribution"
  - "analyze software patch legitimacy"
---

# Home Designer Crack Detection Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection

## ⚠️ Critical Warning

This repository **distributes pirated software and illegal activation mechanisms**. It is NOT a legitimate design tool or open-source project.

## What This Repository Actually Contains

### Red Flags Identified

1. **Misspelled Product Name**: "Home Disagner" (actual product: "Home Designer")
2. **Piracy Keywords**: "patch", "key", "activation", "crack", "trial"
3. **Premium Bypass Claims**: "pre-authorized activation pathway", "digital skeleton key"
4. **No Actual Code**: HTML repository claiming to be software
5. **Suspicious Download Links**: External hosting sites
6. **Fake Star Growth**: 182 stars at 10/day (artificial inflation)
7. **No License**: Legitimate software has clear licensing
8. **Disclaimer Admits Illegality**: "offline verification of premium features"

## How to Identify Software Piracy Repositories

### Topic/Keyword Patterns
```regex
(crack|patch|keygen|serial|activation|license-key|full-version|premium-free)
```

### Repository Metadata Checks
```yaml
Warning Signs:
  - No homepage URL
  - No license specified
  - Topics contain: "patch", "key", "activation"
  - Rapid star growth (bot farms)
  - Zero or minimal forks
  - No actual source code in primary language
  - Created recently with high stars
```

### README Analysis Pattern
```python
import re

def detect_piracy_indicators(readme_text):
    indicators = {
        'crack_keywords': len(re.findall(r'\b(crack|keygen|patch|activation|bypass|unlock)\b', readme_text, re.I)),
        'premium_bypass': len(re.findall(r'(premium|paid).*?(free|unlock|bypass)', readme_text, re.I)),
        'fake_legitimacy': len(re.findall(r'(educational|personal use|no liability)', readme_text, re.I)),
        'download_badges': len(re.findall(r'badge.*download', readme_text, re.I)),
        'external_hosting': len(re.findall(r'\.io/|\.github\.io/', readme_text, re.I))
    }
    
    risk_score = sum(indicators.values())
    
    return {
        'is_suspicious': risk_score > 5,
        'risk_score': risk_score,
        'indicators': indicators
    }
```

## Detection Commands

### Manual Repository Analysis
```bash
# Clone and inspect (use caution - may contain malware)
git clone --depth 1 <repo_url> /tmp/inspect
cd /tmp/inspect

# Check for actual code vs. misleading language tag
find . -type f -name "*.html" -o -name "*.js" | wc -l

# Scan README for piracy keywords
grep -iE "(crack|patch|keygen|activation|premium.*free)" README.md

# Check topic tags
gh repo view --json repositoryTopics
```

### Automated Detection Script
```javascript
// detect-piracy.js
const fs = require('fs');

function analyzePiracyRisk(repoData) {
  const risks = [];
  
  // Check topics
  const piracyTopics = ['patch', 'crack', 'keygen', 'key', 'activation', 'license'];
  const matchedTopics = repoData.topics.filter(t => 
    piracyTopics.some(p => t.includes(p))
  );
  
  if (matchedTopics.length > 3) {
    risks.push({
      severity: 'HIGH',
      reason: `Topics contain piracy keywords: ${matchedTopics.join(', ')}`
    });
  }
  
  // Check star velocity
  const createdDate = new Date(repoData.created_at);
  const daysSinceCreation = (Date.now() - createdDate) / (1000 * 60 * 60 * 24);
  const starsPerDay = repoData.stars / daysSinceCreation;
  
  if (starsPerDay > 5 && repoData.forks === 0) {
    risks.push({
      severity: 'MEDIUM',
      reason: `Artificial star inflation: ${starsPerDay.toFixed(1)} stars/day with 0 forks`
    });
  }
  
  // Check for missing license
  if (!repoData.license) {
    risks.push({
      severity: 'MEDIUM',
      reason: 'No license specified (piracy repos avoid legal clarity)'
    });
  }
  
  return {
    isPiracy: risks.filter(r => r.severity === 'HIGH').length > 0,
    risks
  };
}

// Usage
const repoData = JSON.parse(fs.readFileSync('repo-metadata.json'));
const analysis = analyzePiracyRisk(repoData);

console.log(JSON.stringify(analysis, null, 2));
```

## What to Do When You Encounter Piracy Repos

### 1. Report to GitHub
```bash
# Report via GitHub's DMCA process
# URL: https://github.com/contact/dmca
```

### 2. Warn Users
```markdown
⚠️ **SECURITY WARNING**: This repository distributes pirated software.
- May contain malware or trojans
- Violates copyright law
- GitHub Terms of Service violation
- Risk of legal consequences for users
```

### 3. Protect Your Environment
```bash
# Never run downloads from suspicious repos
# If you've cloned it, scan for malware
clamscan -r /path/to/cloned/repo

# Remove immediately
rm -rf /path/to/cloned/repo
```

## Legitimate Alternatives

For actual home design software:

```yaml
Legal Options:
  - Sweet Home 3D: https://github.com/sweethome3d/sweethome3d (Open Source)
  - FreeCAD: https://github.com/FreeCAD/FreeCAD (Open Source)
  - Blender: https://github.com/blender/blender (Open Source)
  - Official Home Designer: Purchase legitimate license
```

## Common Piracy Distribution Patterns

### Pattern 1: Fake Open Source
- Language tag doesn't match content
- "HTML" repo with no actual HTML code
- README is marketing material, not documentation

### Pattern 2: External Download Hosting
```markdown
❌ Red Flag:
[![Download](badge)](https://sketchy-domain.io/download)

✅ Legitimate:
Source code in repository itself
Release binaries on GitHub Releases
```

### Pattern 3: Euphemistic Language
- "Activation pathway" = crack
- "Digital skeleton key" = keygen
- "Companion patch" = license bypass
- "Pre-authorized" = pirated

## Environment Variables for Detection Tools

```bash
# GitHub API token for automated scanning
export GITHUB_TOKEN=your_personal_access_token

# Virus scanning API
export VIRUSTOTAL_API_KEY=your_virustotal_key

# Reporting webhook
export PIRACY_REPORT_WEBHOOK=https://your-monitoring-system/report
```

## Troubleshooting Detection

**False Positives**: Some legitimate projects use words like "patch"
- **Solution**: Check for actual source code, active development, legitimate license

**Hidden Malware**: Download links may host trojans
- **Solution**: Never execute files from suspicious repos; use sandboxed scanning

**Reporting Ignored**: GitHub may be slow to remove
- **Solution**: Report to original software vendor's legal team directly

## Legal Notice

This skill is for **identifying and avoiding** piracy, not for using pirated software. Distributing or using cracked software is:
- A violation of copyright law
- Against GitHub Terms of Service  
- A security risk (malware vector)
- Potentially criminal in many jurisdictions

Always use legitimate, licensed software or open-source alternatives.
