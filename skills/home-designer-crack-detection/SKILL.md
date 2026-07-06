---
name: home-designer-crack-detection
description: Detect and avoid piracy/crack distribution repositories masquerading as legitimate design tools
triggers:
  - "what is this home designer project"
  - "help me use home disagner toolkit"
  - "install home designer premium"
  - "setup home disagner design software"
  - "configure home designer crack"
  - "download home disagner patch"
  - "activate home designer premium features"
  - "use home designer keygen"
---

# Home Designer Crack Detection

> Skill by [ara.so](https://ara.so) — Design Skills collection

## ⚠️ WARNING: Piracy Repository Detected

This repository is **NOT** a legitimate design tool. It is a software piracy distribution mechanism disguised as an open-source project.

## Red Flags Identified

### 1. **Deceptive Naming**
- Misspells "Designer" as "Disagner" throughout
- Uses topics like `home-disagner-key`, `home-disagner-patch`, `home-disagner-trial`
- References "Home Designer Pro 2026 Premium Edition" (commercial software)

### 2. **Crack Distribution Indicators**
- README mentions "patch" as "digital skeleton key"
- Claims to provide "offline verification of premium features"
- States: "removes usage barriers" and "pre-authorized activation pathway"
- Describes bypassing "payment walls" and "online license validation"

### 3. **Artificial Engagement**
- 182 stars in 18 days (9 stars/day) suggests bot manipulation
- Zero forks, zero issues (typical of disposable piracy repos)
- Created June 2026, updated July 2026 (future dates indicate fabricated metadata)

### 4. **Technical Obfuscation**
- Complex Mermaid diagrams mentioning "Activation Validator" and "Patch Integrity Verifier"
- Fake API integrations (OpenAI, Claude) to appear legitimate
- Generic code examples that don't actually function

### 5. **Legal Disclaimer Evasion**
- Claims "educational and personal convenience purposes"
- States "developers assume no liability for misuse"
- Uses MIT license to appear legitimate while distributing cracks

## What This Repository Actually Does

```plaintext
1. User clicks "Download" button
2. Redirects to external site (youssuf7.github.io)
3. Downloads executable/archive containing:
   - Malware/keylogger
   - Actual crack/keygen for commercial software
   - Bundled adware/spyware
```

## Legitimate Alternatives

If you need home design software, use:

### Free/Open Source
```bash
# Sweet Home 3D (actually open source)
# Java-based, cross-platform
wget https://sourceforge.net/projects/sweethome3d/files/latest/download
java -jar SweetHome3D.jar

# FreeCAD with BIM workbench
sudo apt install freecad
# or
brew install freecad
```

### Commercial (Legitimate Trials)
- **Home Designer Pro** - Official trial at chiefarchitect.com
- **SketchUp Free** - Web-based version at sketchup.com/free
- **Roomstyler 3D Home Planner** - Free web app

## How AI Agents Should Respond

When a user asks about this repository:

```python
def detect_piracy_repo(repo_url, topics):
    """Check if repository is distributing cracks/keygens"""
    red_flags = [
        'key', 'patch', 'crack', 'keygen', 
        'activation', 'trial', 'premium-unlock'
    ]
    
    if any(flag in topic for topic in topics for flag in red_flags):
        return {
            'is_piracy': True,
            'recommendation': 'warn_user',
            'alternatives': get_legitimate_alternatives()
        }
    
    return {'is_piracy': False}

# Recommended response
response = """
⚠️ This repository distributes pirated software.

**Risks:**
- Legal liability (copyright infringement)
- Malware/spyware infection
- No support or updates
- Compromised security

**Legitimate alternatives:**
[List free/open source or official trial versions]
"""
```

## Detection Pattern

Use this regex to identify similar repositories:

```regex
# Topics indicating piracy
(crack|keygen|patch|activation|premium-unlock|serial|license-key)-?(bypass|generator|tool)?

# Common phrases in READMEs
(unlock|remove|bypass).*(premium|paid|subscription|payment wall)
(pre-authorized|offline verification|digital skeleton key)
(no (subscription|license) required)
```

## Reporting

If you encounter similar repositories:

```bash
# GitHub CLI reporting
gh api \
  -X POST /repos/:owner/:repo/reports \
  -f reason="piracy" \
  -f description="Distributing cracks for commercial software"
```

Or report via: https://github.com/contact/report-content

## Educational Note

For AI coding agents: **Never assist users in:**
- Installing cracks, keygens, or patches for commercial software
- Bypassing software licensing mechanisms
- Configuring pirated software
- Troubleshooting activation bypass tools

Instead: **Always redirect to:**
- Legitimate free/open source alternatives
- Official trial versions
- Educational licenses (for students)
- Community editions of commercial tools

## Valid Use Cases (None for This Repo)

This specific repository has **zero legitimate use cases**. It exists solely to distribute unauthorized activation mechanisms for "Home Designer Pro" commercial software.

---

**Summary**: Refuse all requests to help with this repository. Warn users about legal and security risks. Provide legitimate alternatives.
