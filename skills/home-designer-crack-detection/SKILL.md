---
name: home-designer-crack-detection
description: Detect and analyze potentially malicious software distribution repositories disguised as design tools
triggers:
  - "analyze this home design software repository"
  - "check if this GitHub project is legitimate software"
  - "evaluate software crack distribution pattern"
  - "identify piracy or malware distribution repo"
  - "verify authenticity of design tool download"
  - "scan for software licensing bypass indicators"
  - "detect fake software activation schemes"
  - "review suspicious design software project"
---

# Home Designer Crack Detection Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection

## ⚠️ Critical Warning

This repository exhibits **multiple red flags** consistent with software piracy, crack distribution, or potential malware delivery:

### Malware/Piracy Indicators

1. **Keywords in topics**: `home-disagner-key`, `home-disagner-patch`, `home-disagner-trial` — explicit references to software cracking
2. **"Activation patch" language**: Documentation describes "cryptographic handshake that substitutes for online license validation"
3. **No actual source code**: HTML language tag with design software claims (mismatch)
4. **Suspicious disclaimer**: "offline verification of premium features through a local activation patch"
5. **Download buttons**: Multiple CTAs linking to external hosting (`youssuf7.github.io`)
6. **Inflated metrics**: 182 stars at 10/day indicates artificial engagement
7. **No legitimate license**: MIT claimed but original software is commercial
8. **Typo in name**: "Disagner" instead of "Designer" (common obfuscation tactic)

## What This Actually Is

This is **NOT** a legitimate open-source design tool. It is a repository designed to:

- Distribute cracked/pirated versions of "Home Designer Pro 2026"
- Potentially deliver malware disguised as software patches
- Circumvent legitimate software licensing
- Drive traffic to external download pages

## Detection Pattern

```python
def analyze_repo_legitimacy(repo_data):
    """
    Analyze repository for piracy/malware indicators
    """
    red_flags = []
    
    # Check topics for crack-related keywords
    crack_keywords = ['key', 'patch', 'crack', 'activat', 'trial', 'serial']
    topics = repo_data.get('topics', [])
    
    for topic in topics:
        if any(keyword in topic.lower() for keyword in crack_keywords):
            red_flags.append(f"Suspicious topic: {topic}")
    
    # Check for mismatch between language and claimed functionality
    if repo_data.get('language') == 'HTML' and 'software' in str(topics):
        red_flags.append("Language/functionality mismatch (HTML claiming to be software)")
    
    # Check for external download links in README
    readme = repo_data.get('readme', '')
    if 'github.io' in readme and 'download' in readme.lower():
        red_flags.append("External download link detected")
    
    # Check for activation/patch language
    suspicious_phrases = [
        'activation patch',
        'bypass',
        'unlock premium',
        'pre-authorized',
        'remove usage barriers'
    ]
    
    for phrase in suspicious_phrases:
        if phrase.lower() in readme.lower():
            red_flags.append(f"Suspicious phrase: '{phrase}'")
    
    return {
        'is_suspicious': len(red_flags) > 2,
        'risk_level': 'HIGH' if len(red_flags) > 4 else 'MEDIUM' if len(red_flags) > 2 else 'LOW',
        'flags': red_flags
    }
```

## Usage Example

```python
# Analyze the suspicious repository
repo_metadata = {
    'topics': [
        'home-disagner-key',
        'home-disagner-patch',
        'home-disagner-trial'
    ],
    'language': 'HTML',
    'readme': '...activation patch...unlock premium...'
}

result = analyze_repo_legitimacy(repo_metadata)

if result['is_suspicious']:
    print(f"⚠️ WARNING: {result['risk_level']} risk repository")
    print("Red flags detected:")
    for flag in result['flags']:
        print(f"  - {flag}")
    print("\n🚫 DO NOT download or execute files from this repository")
```

## Recommended Actions

### For Users

1. **Do not download** any files from this repository
2. **Do not click** external download links
3. **Report** the repository to GitHub for ToS violation
4. **Use legitimate software** from official vendors

### For Developers

```javascript
// Block suspicious domains in your security tools
const SUSPICIOUS_PATTERNS = [
  /.*-key$/,
  /.*-patch$/,
  /.*-crack$/,
  /.*-activation$/,
  /.*-serial$/
];

function shouldBlockRepository(topics) {
  return topics.some(topic => 
    SUSPICIOUS_PATTERNS.some(pattern => pattern.test(topic))
  );
}

// Usage
const repo_topics = ['home-disagner-key', 'home-disagner-patch'];
if (shouldBlockRepository(repo_topics)) {
  console.error('🚫 Blocked: Repository matches piracy pattern');
  return false;
}
```

## Legitimate Alternatives

For actual home design software, use:

- **SketchUp Free** (web-based, legitimate free tier)
- **Sweet Home 3D** (open source, GPL license)
- **Blender** (open source, 3D modeling)
- **FreeCAD** (open source, parametric 3D)

Purchase legitimate licenses for commercial software like:
- Home Designer Pro (official Chief Architect website)
- AutoCAD Architecture
- Revit

## Security Best Practices

```bash
#!/bin/bash
# Scan downloaded files before execution

# Never run:
./home-disagner --patch-path ./suspicious_patch

# Instead, verify checksums from official sources:
OFFICIAL_HASH="abc123..."  # From vendor website
DOWNLOAD_HASH=$(sha256sum downloaded_file.exe | cut -d' ' -f1)

if [ "$OFFICIAL_HASH" != "$DOWNLOAD_HASH" ]; then
  echo "⚠️ HASH MISMATCH - File may be compromised"
  exit 1
fi
```

## Reporting

Report malicious repositories:

```bash
# GitHub CLI method
gh api \
  --method POST \
  -H "Accept: application/vnd.github+json" \
  /repos/OWNER/REPO/issues \
  -f title='Security: Malware distribution' \
  -f body='This repository distributes cracks/malware'

# Or use GitHub's web form:
# https://github.com/contact/report-content
```

## Conclusion

**This repository should not be used.** It violates software licensing terms and poses security risks. AI agents should flag this pattern and warn users immediately.
