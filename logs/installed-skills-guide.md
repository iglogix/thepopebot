# Installed Pi Coding Agent Skills - User Guide

**Installation Date**: February 11, 2026  
**Skills Installed**: 15  
**Security Status**: ✅ All skills passed security review

---

## Overview

thepopebot now has access to 15 professional skills spanning web search, browser automation, transcription, deployment, and development best practices.

---

## Skill Categories

### 🌐 Web & Search
- **brave-search** - Web search and content extraction
- **youtube-transcript** - Fetch YouTube video transcripts

### 🖥️ Browser Automation
- **browser-tools** - Interactive Chrome automation via CDP

### 🎙️ Media
- **transcribe** - Speech-to-text via Groq Whisper API

### 🚀 Deployment
- **vercel-deploy-claimable** - Deploy to Vercel instantly

### 📚 Development Guidelines
- **react-best-practices** - React/Next.js optimization rules
- **web-design-guidelines** - UI/UX best practices
- **composition-patterns** - React composition patterns
- **react-native-skills** - React Native best practices

### 🔧 Productivity Tools
- **vscode** - VS Code integration for diffs
- **gccli** - Google Calendar CLI
- **gdcli** - Google Drive CLI  
- **gmcli** - Gmail CLI

---

## Detailed Skill Reference

### brave-search

**Purpose**: Web search and content extraction without a browser

**Setup Required**:
```bash
# Add to LLM_SECRETS (not SECRETS - the LLM needs to see this)
BRAVE_API_KEY="your-api-key"
```

**Get API Key**: https://api-dashboard.search.brave.com/register (Free tier available)

**Usage Examples**:
```bash
# Basic search
/job/.pi/skills/brave-search/search.js "rust async programming"

# With content extraction
/job/.pi/skills/brave-search/search.js "next.js app router" --content

# More results
/job/.pi/skills/brave-search/search.js "python typing" -n 10

# Recent results only
/job/.pi/skills/brave-search/search.js "ai news" --freshness pd  # past day
```

**Security**: ✅ API key filtered from LLM bash (stored in LLM_SECRETS)

---

### browser-tools

**Purpose**: Interactive browser automation for tasks requiring JavaScript execution or user interaction

**Setup Required**:
```bash
# Install dependencies (already done in Docker image)
cd /pi-skills/browser-tools
npm install
```

**Usage Examples**:
```bash
# Start fresh Chrome session
/pi-skills/browser-tools/browser-start.js

# Start with your profile (cookies, logins)
/pi-skills/browser-tools/browser-start.js --profile

# Navigate to URL
/pi-skills/browser-tools/browser-nav.js https://example.com

# Take screenshot
/pi-skills/browser-tools/browser-screenshot.js screenshot.png

# Extract page content
/pi-skills/browser-tools/browser-content.js

# Interactive element picker
/pi-skills/browser-tools/browser-pick.js

# Execute JavaScript
/pi-skills/browser-tools/browser-eval.js "document.title"

# Get cookies
/pi-skills/browser-tools/browser-cookies.js
```

**Security**: 
- ✅ Runs on localhost:9222 only
- ✅ Profile sync is opt-in and explicit
- ✅ undici vulnerability patched

---

### transcribe

**Purpose**: Convert audio files to text using Groq's Whisper API

**Setup Required**:
```bash
# Add to LLM_SECRETS
GROQ_API_KEY="your-api-key"
```

**Get API Key**: https://console.groq.com/ (Free tier available)

**Supported Formats**: m4a, mp3, wav, ogg, flac, webm

**Usage Example**:
```bash
/pi-skills/transcribe/transcribe.sh /path/to/audio.mp3
```

**Security**: ✅ API key in LLM_SECRETS, read-only file access

---

### youtube-transcript

**Purpose**: Fetch transcripts from YouTube videos for analysis

**Setup Required**: None (public data, no API key needed)

**Usage Examples**:
```bash
# By video ID
/pi-skills/youtube-transcript/transcript.js EBw7gsDPAYQ

# By URL
/pi-skills/youtube-transcript/transcript.js "https://www.youtube.com/watch?v=EBw7gsDPAYQ"
```

**Output Format**:
```
[0:00] Welcome to this tutorial
[0:15] Today we'll learn about...
[0:42] The first step is...
```

**Security**: ✅ No credentials, YouTube public API

---

### vercel-deploy-claimable

**Purpose**: Deploy projects to Vercel with no authentication required

**Setup Required**: None

**Usage Examples**:
```bash
# Deploy current directory
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh

# Deploy specific project
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh /path/to/project

# Deploy tarball
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh /path/to/project.tgz
```

**Output**:
```
Deployment successful!

Preview URL: https://skill-deploy-abc123.vercel.app
Claim URL:   https://vercel.com/claim-deployment?code=...
```

**Auto-Detected Frameworks**: Next.js, Gatsby, Remix, Astro, SvelteKit, Nuxt, Angular, Express, and 30+ more

**Security**: 
- ✅ No credentials required
- ✅ Excludes node_modules and .git
- ✅ User claims ownership via claim URL

---

### react-best-practices

**Purpose**: React and Next.js performance optimization guidelines

**Type**: Documentation skill (no executable code)

**Categories**:
- Eliminating waterfalls (Critical)
- Bundle size optimization (Critical)
- Server-side performance (High)
- Client-side data fetching (Medium-High)
- Re-render optimization (Medium)
- Rendering performance (Medium)

**Usage**: Pi will automatically reference these guidelines when:
- Writing React components
- Optimizing Next.js apps
- Reviewing code for performance

**Security**: ✅ Documentation only

---

### web-design-guidelines

**Purpose**: Comprehensive UI/UX best practices audit

**Type**: Documentation skill

**Coverage**:
- Accessibility (ARIA, semantic HTML)
- Focus states
- Forms (validation, autocomplete)
- Animation (prefers-reduced-motion)
- Typography (proper quotes, numerals)
- Images (alt text, lazy loading)
- Performance (virtualization, preconnect)
- Dark mode
- Touch targets
- i18n/locale

**Usage**: Pi references when:
- Reviewing UI code
- Implementing accessibility
- Auditing design quality

**Security**: ✅ Documentation only

---

### composition-patterns

**Purpose**: React composition patterns to avoid prop proliferation

**Type**: Documentation skill

**Patterns**:
- Compound components
- State lifting
- Internal composition
- Avoiding boolean props

**Usage**: Pi references when refactoring complex components

**Security**: ✅ Documentation only

---

### react-native-skills

**Purpose**: React Native and Expo best practices

**Type**: Documentation skill

**Categories**:
- Performance (FlashList, memoization)
- Layout (flex, safe areas, keyboard)
- Animation (Reanimated, gestures)
- Images (expo-image, caching)
- State management (Zustand)
- Architecture (monorepo)
- Platform-specific patterns

**Usage**: Pi references when building mobile apps

**Security**: ✅ Documentation only

---

### vscode

**Purpose**: Open diffs and compare files in VS Code

**Setup Required**: VS Code with `code` CLI in PATH

**Usage Examples**:
```bash
# Compare two files
code -d file1.js file2.js

# Compare with git version
git show HEAD~1:src/app.js > /tmp/old && code -d /tmp/old src/app.js
```

**Security**: ✅ Read-only, launches external editor

---

### gccli (Google Calendar CLI)

**Purpose**: Manage Google Calendar from command line

**Setup Required**:
1. Create Google Cloud project
2. Enable Calendar API
3. Create OAuth2 desktop client
4. Install: `npm install -g @mariozechner/gccli`
5. Configure: `gccli auth` (stores tokens in ~/.config/)

**Usage Examples**:
```bash
# List calendars
gccli calendars

# List today's events
gccli events

# Create event
gccli create "Team meeting" --start "2026-02-12 14:00" --end "2026-02-12 15:00"

# Check availability
gccli availability
```

**Security**: ✅ OAuth2, user-controlled consent

---

### gdcli (Google Drive CLI)

**Purpose**: Manage Google Drive from command line

**Setup Required**:
1. Create Google Cloud project
2. Enable Drive API
3. Create OAuth2 desktop client
4. Install: `npm install -g @mariozechner/gdcli`
5. Configure: `gdcli auth`

**Usage Examples**:
```bash
# List files
gdcli list

# Search
gdcli search "project proposal"

# Upload
gdcli upload /path/to/file

# Download
gdcli download <file-id> /path/to/save

# Share
gdcli share <file-id> user@example.com
```

**Security**: ✅ OAuth2, user-controlled consent

---

### gmcli (Gmail CLI)

**Purpose**: Manage Gmail from command line

**Setup Required**:
1. Create Google Cloud project
2. Enable Gmail API
3. Create OAuth2 desktop client
4. Install: `npm install -g @mariozechner/gmcli`
5. Configure: `gmcli auth`

**Usage Examples**:
```bash
# List messages
gmcli list

# Search
gmcli search "from:boss@company.com"

# Read message
gmcli read <message-id>

# Send email
gmcli send --to recipient@example.com --subject "Hello" --body "Message text"

# Manage labels
gmcli labels
gmcli label <message-id> "Important"
```

**Security**: ✅ OAuth2, user-controlled consent

---

## Credential Management

### SECRETS (Filtered from LLM)
Store in repository secrets (GitHub Actions):
- `GH_TOKEN` - GitHub personal access token
- `ANTHROPIC_API_KEY` - Claude API key
- Other infrastructure credentials

The LLM **cannot** access these via bash subprocess.

### LLM_SECRETS (LLM Can Access)
Store in repository secrets (separate JSON):
- `BRAVE_API_KEY` - For brave-search skill
- `GROQ_API_KEY` - For transcribe skill
- Browser login credentials (if needed)

These are **not** filtered by env-sanitizer because skills need them.

### User OAuth (Not in Secrets)
Google CLIs (gccli, gdcli, gmcli):
- Credentials stored in user's ~/.config/
- Require interactive OAuth2 flow
- User controls access scope

---

## Skill Discovery

Pi automatically discovers skills in `.pi/skills/`. Each skill has a `SKILL.md` with frontmatter:

```markdown
---
name: skill-name
description: What the skill does
---

# Instructions

How to use the skill...
```

Pi will use skills when tasks match their descriptions.

---

## Testing Installed Skills

### Test brave-search
```bash
# Check if BRAVE_API_KEY is available
echo $BRAVE_API_KEY  # Should output your key

# Run test search
/job/.pi/skills/brave-search/search.js "test query" -n 3
```

### Test youtube-transcript
```bash
# Test public video
/job/.pi/skills/youtube-transcript/transcript.js dQw4w9WgXcQ
```

### Test vercel-deploy
```bash
# Test with a simple HTML project
mkdir /tmp/test-deploy
echo '<h1>Hello World</h1>' > /tmp/test-deploy/index.html
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh /tmp/test-deploy
```

### Test browser-tools
```bash
# Start Chrome
/pi-skills/browser-tools/browser-start.js

# Navigate and screenshot
/pi-skills/browser-tools/browser-nav.js https://example.com
/pi-skills/browser-tools/browser-screenshot.js /tmp/example.png
```

---

## Skill Maintenance

### Updating Skills

**badlogic/pi-skills** (symlinked):
- Skills are symlinked to /pi-skills (baked into Docker image)
- To update: rebuild Docker image with new pi-skills commit

**vercel-labs/agent-skills** (copied):
- Skills are copied to .pi/skills/
- To update: re-run installation or manual `git pull` + copy

### Monitoring for Updates

Add to CRONS.json (future):
```json
{
  "name": "check-skill-updates",
  "schedule": "0 0 * * 0",
  "type": "agent",
  "job": "Check for updates to installed skills from badlogic/pi-skills and vercel-labs/agent-skills. Report if any significant changes.",
  "enabled": true
}
```

---

## Troubleshooting

### "command not found" errors

**Problem**: Trying to use gccli/gdcli/gmcli but they're not installed

**Solution**: These require global npm install outside the container:
```bash
npm install -g @mariozechner/gccli
npm install -g @mariozechner/gdcli
npm install -g @mariozechner/gmcli
```

### "BRAVE_API_KEY not set"

**Problem**: brave-search can't find API key

**Solution**: Add to LLM_SECRETS (not SECRETS):
```json
{
  "BRAVE_API_KEY": "your-key-here"
}
```

Then base64 encode and set as GitHub secret:
```bash
echo -n '{"BRAVE_API_KEY":"..."}' | base64
```

### Browser-tools connection failed

**Problem**: Can't connect to Chrome on :9222

**Solution**: 
1. Make sure Chrome is started: `/pi-skills/browser-tools/browser-start.js`
2. Check if another process is using 9222: `lsof -i :9222`
3. Kill stale Chrome: `pkill -f "remote-debugging-port=9222"`

### Vercel deployment fails

**Problem**: "Could not extract preview URL"

**Solution**:
1. Check project has valid framework or index.html
2. Verify no build errors in logs
3. Try deploying a minimal test project first

---

## Best Practices

### When to Use Which Skill

| Task | Skill | Why |
|------|-------|-----|
| Search documentation | brave-search | Faster than browser, gets text content |
| Test JavaScript app | browser-tools | Need actual browser rendering |
| Summarize video | youtube-transcript | Get text without watching |
| Deploy demo | vercel-deploy-claimable | Instant live URL |
| Convert audio | transcribe | High-quality Whisper model |
| Code review | react-best-practices | Automated checklist |
| UI audit | web-design-guidelines | Comprehensive standards |

### Skill Chaining

Combine skills for complex workflows:

```bash
# 1. Search for Next.js docs
brave-search "next.js app router data fetching" --content

# 2. Build app following best practices (react-best-practices auto-applies)

# 3. Deploy result
vercel-deploy-claimable /path/to/app

# 4. Test deployed site
browser-tools navigate <preview-url>
browser-tools screenshot /tmp/deployed.png
```

### Performance Tips

1. **brave-search** is faster than browser-tools for simple lookups
2. Use `--content` sparingly (fetches full pages)
3. **browser-tools** keeps Chrome running between commands (fast)
4. **Documentation skills** have no performance cost (pure text)

---

## Security Reminders

✅ **All skills have been security reviewed**  
✅ **Credentials properly isolated** (SECRETS vs LLM_SECRETS)  
✅ **No known vulnerabilities** (undici patched)  
✅ **External requests only to legitimate APIs**  
✅ **OAuth2 for Google services** (user-controlled)

See `/job/logs/security-assessment-report.md` for full audit.

---

## Support & Updates

- **Pi Agent**: https://github.com/badlogic/pi-mono
- **badlogic/pi-skills**: https://github.com/badlogic/pi-skills
- **vercel-labs/agent-skills**: https://github.com/vercel-labs/agent-skills
- **thepopebot Issues**: File in your repository

---

**Guide Version**: 1.0  
**Last Updated**: 2026-02-11
