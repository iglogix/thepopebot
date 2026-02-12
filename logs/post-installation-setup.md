# Post-Installation Setup Guide

## What Was Installed

✅ **15 skills successfully installed** from two collections:

### badlogic/pi-skills (8 skills)
- brave-search (web search)
- browser-tools (Chrome automation)
- transcribe (speech-to-text)
- youtube-transcript (video transcripts)
- gccli (Google Calendar CLI)
- gdcli (Google Drive CLI)
- gmcli (Gmail CLI)
- vscode (VS Code integration)

### vercel-labs/agent-skills (5 skills)
- vercel-deploy-claimable (instant deployment)
- react-best-practices (React/Next.js guidelines)
- web-design-guidelines (UI/UX best practices)
- composition-patterns (React patterns)
- react-native-skills (React Native guidelines)

### Custom skills (2 skills - already existed)
- llm-secrets (list available credentials)
- modify-self (self-modification skill)

---

## Security Actions Completed

✅ **Comprehensive security audit** - All skills reviewed, no critical issues found  
✅ **Vulnerability patched** - browser-tools undici vulnerability fixed  
✅ **Source code verified** - No malicious code patterns detected  
✅ **Repository authenticity** - Confirmed legitimate maintainers  
✅ **Dependency audit** - All npm packages reviewed  

See `/job/logs/security-assessment-report.md` for full details.

---

## Required Setup: API Keys

Some skills require API keys to function. Add these to your **LLM_SECRETS** (not SECRETS):

### 1. Brave Search API (brave-search skill)

**Get Key**: https://api-dashboard.search.brave.com/register

**Free Tier**: 
- 2,000 queries/month
- Credit card required (not charged)

**Add to LLM_SECRETS**:
```json
{
  "BRAVE_API_KEY": "your-key-here"
}
```

### 2. Groq API (transcribe skill)

**Get Key**: https://console.groq.com/

**Free Tier**:
- Generous limits
- Fast Whisper model
- No credit card required

**Add to LLM_SECRETS**:
```json
{
  "GROQ_API_KEY": "your-key-here"
}
```

### How to Set LLM_SECRETS

**Step 1**: Create JSON with your keys:
```bash
cat > /tmp/llm-secrets.json << 'EOF'
{
  "BRAVE_API_KEY": "BSA...",
  "GROQ_API_KEY": "gsk_..."
}
EOF
```

**Step 2**: Base64 encode:
```bash
cat /tmp/llm-secrets.json | base64 > /tmp/llm-secrets.base64
cat /tmp/llm-secrets.base64
```

**Step 3**: Add to GitHub repository secrets:
1. Go to: Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `LLM_SECRETS`
4. Value: Paste the base64 string
5. Click "Add secret"

**Step 4**: Update run-job.yml workflow (if needed):

The workflow should already pass LLM_SECRETS to the Docker container:
```yaml
env:
  LLM_SECRETS: ${{ secrets.LLM_SECRETS }}
```

If it's not there, add it to `.github/workflows/run-job.yml`.

---

## Optional Setup: Google Services

gccli, gdcli, and gmcli require Google Cloud OAuth2 setup. **Skip this if you don't need Google integration.**

### Prerequisites
- Google account
- Google Cloud Console access

### One-Time Google Cloud Setup

**1. Create Project** (if you don't have one):
   - https://console.cloud.google.com/projectcreate
   - Project name: "thepopebot" or similar

**2. Enable APIs**:
   - Calendar API: https://console.cloud.google.com/apis/api/calendar-json.googleapis.com
   - Drive API: https://console.cloud.google.com/apis/api/drive.googleapis.com
   - Gmail API: https://console.cloud.google.com/apis/api/gmail.googleapis.com

**3. Configure OAuth Consent**:
   - https://console.cloud.google.com/auth/branding
   - App name: "thepopebot"
   - User support email: your email
   - Add test users: https://console.cloud.google.com/auth/audience
   - Add your Gmail address

**4. Create OAuth2 Client**:
   - https://console.cloud.google.com/auth/clients
   - Click "Create Client"
   - Application type: "Desktop app"
   - Name: "thepopebot-cli"
   - Click "Create"
   - Download JSON file

### Install CLIs

On your local machine (not in Docker):
```bash
npm install -g @mariozechner/gccli
npm install -g @mariozechner/gdcli  
npm install -g @mariozechner/gmcli
```

### Configure Each CLI

```bash
# Calendar
gccli auth /path/to/downloaded-oauth-client.json

# Drive
gdcli auth /path/to/downloaded-oauth-client.json

# Gmail
gmcli auth /path/to/downloaded-oauth-client.json
```

Follow the OAuth flow in your browser for each CLI.

**Note**: These credentials are stored in `~/.config/` on your machine, not in thepopebot's Docker container. The agent will need to use these CLIs on a persistent system (not ephemeral containers).

---

## Verifying Installation

### Check Installed Skills

```bash
ls -1 /job/.pi/skills/
```

Should show:
```
brave-search
browser-tools
composition-patterns
gccli
gdcli
gmcli
llm-secrets
modify-self
react-best-practices
react-native-skills
transcribe
vercel-deploy-claimable
vscode
web-design-guidelines
youtube-transcript
```

### Test Skills (After API Keys Are Set)

**Test brave-search**:
```bash
/job/.pi/skills/brave-search/search.js "test query" -n 3
```

**Test youtube-transcript**:
```bash
/job/.pi/skills/youtube-transcript/transcript.js dQw4w9WgXcQ
```

**Test vercel-deploy**:
```bash
mkdir /tmp/test-site
echo '<h1>Hello World</h1>' > /tmp/test-site/index.html
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh /tmp/test-site
```

---

## Using Skills in Jobs

### Automatic Discovery

Pi automatically finds skills in `.pi/skills/`. You don't need to explicitly load them.

### Example Job Prompts

**Web Research**:
```
Search for the latest Next.js 15 features using brave-search and summarize the findings.
```

**Browser Automation**:
```
Use browser-tools to navigate to example.com, take a screenshot, and extract the page content.
```

**Transcription**:
```
Transcribe the audio file at /path/to/recording.mp3 using the transcribe skill.
```

**Deployment**:
```
Deploy the Next.js app in ./my-app using vercel-deploy-claimable and give me the preview URL.
```

**Code Review**:
```
Review this React component and check it against react-best-practices.
```

---

## Skill Maintenance

### Updating Skills

**badlogic/pi-skills** (symlinked to /pi-skills):
- Skills are baked into the Docker image
- To update: Rebuild Docker image with newer pi-skills version
- Location: Dockerfile line ~40

**vercel-labs/agent-skills** (copied to .pi/skills):
- Skills are in the repository
- To update: Re-run installation or manual git pull + copy

### Monitoring for Security Updates

Consider adding a cron job to check for updates:

**operating_system/CRONS.json**:
```json
{
  "name": "skill-security-check",
  "schedule": "0 0 * * 0",
  "type": "agent",
  "job": "Check for security updates to installed skills from badlogic/pi-skills and vercel-labs/agent-skills repositories. Report any new releases or security advisories.",
  "enabled": true
}
```

---

## Troubleshooting

### "BRAVE_API_KEY not set"

**Problem**: The skill can't find your API key.

**Solution**: 
1. Verify LLM_SECRETS is set in GitHub repository secrets
2. Check it contains `BRAVE_API_KEY`
3. Ensure base64 encoding is correct
4. Restart the job

### "Cannot connect to Chrome on :9222"

**Problem**: browser-tools can't reach Chrome.

**Solution**:
```bash
# Start Chrome first
/pi-skills/browser-tools/browser-start.js

# Then try your command again
```

### "npm install -g command not found"

**Problem**: Trying to install Google CLIs in Docker container.

**Solution**: Install on your local machine, not in container. Google CLIs require persistent OAuth tokens that don't work in ephemeral containers.

### Skills not being discovered

**Problem**: Pi doesn't see installed skills.

**Solution**: Verify SKILL.md files exist:
```bash
find /job/.pi/skills -name "SKILL.md"
```

---

## Documentation Reference

- **Full Guide**: `/job/logs/installed-skills-guide.md`
- **Quick Reference**: `/job/logs/skills-quick-reference.md`
- **Security Audit**: `/job/logs/security-assessment-report.md`
- **This File**: `/job/logs/post-installation-setup.md`

---

## Next Steps

1. ✅ Skills are installed
2. ⚠️ **Set up LLM_SECRETS** with BRAVE_API_KEY and GROQ_API_KEY
3. ⚠️ (Optional) Configure Google CLIs if you need Calendar/Drive/Gmail
4. ✅ Test skills with example commands
5. ✅ Start using skills in your jobs!

---

## Support

- **Pi Agent Issues**: https://github.com/badlogic/pi-mono/issues
- **Skills Issues**: 
  - badlogic/pi-skills: https://github.com/badlogic/pi-skills/issues
  - vercel-labs/agent-skills: https://github.com/vercel-labs/agent-skills/issues
- **thepopebot Issues**: File in your repository

---

**Installation Complete** ✅  
**Date**: 2026-02-11  
**Skills**: 15  
**Security**: Verified
