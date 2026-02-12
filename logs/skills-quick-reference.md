# Pi Skills - Quick Reference Card

## 🔍 Search & Content

### brave-search
```bash
/job/.pi/skills/brave-search/search.js "query" [-n 10] [--content] [--freshness pd]
```
**Requires**: `BRAVE_API_KEY` in LLM_SECRETS

### youtube-transcript  
```bash
/job/.pi/skills/youtube-transcript/transcript.js <video-id-or-url>
```
**No credentials needed**

---

## 🌐 Browser Automation

### browser-tools
```bash
# Start
/pi-skills/browser-tools/browser-start.js [--profile]

# Navigate
/pi-skills/browser-tools/browser-nav.js <url>

# Screenshot
/pi-skills/browser-tools/browser-screenshot.js <output.png>

# Content
/pi-skills/browser-tools/browser-content.js

# Eval JS
/pi-skills/browser-tools/browser-eval.js "document.title"

# Cookies
/pi-skills/browser-tools/browser-cookies.js

# Pick element
/pi-skills/browser-tools/browser-pick.js
```
**No credentials needed** (localhost only)

---

## 🎙️ Audio

### transcribe
```bash
/pi-skills/transcribe/transcribe.sh <audio-file>
```
**Requires**: `GROQ_API_KEY` in LLM_SECRETS  
**Formats**: m4a, mp3, wav, ogg, flac, webm

---

## 🚀 Deployment

### vercel-deploy-claimable
```bash
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh [project-dir]
```
**Returns**: Preview URL + Claim URL  
**No credentials needed**

---

## 📚 Documentation Skills (Auto-Applied)

### react-best-practices
40+ React/Next.js performance rules

### web-design-guidelines
100+ UI/UX/accessibility best practices

### composition-patterns
React composition patterns

### react-native-skills
React Native/Expo guidelines

---

## 🔧 Google Services (Require OAuth Setup)

### gccli (Calendar)
```bash
gccli calendars
gccli events
gccli create "Event" --start "2026-02-12 14:00"
```

### gdcli (Drive)
```bash
gdcli list
gdcli search "query"
gdcli upload <file>
gdcli download <file-id>
```

### gmcli (Gmail)
```bash
gmcli list
gmcli search "from:user@example.com"
gmcli send --to user@example.com --subject "Hi"
```

**Setup**: `npm install -g @mariozechner/<cli>` then `<cli> auth`

---

## 🛠️ Editor Integration

### vscode
```bash
code -d <file1> <file2>  # Diff two files
```
**Requires**: VS Code with `code` CLI in PATH

---

## 🔑 Credential Types

### SECRETS (LLM Cannot Access)
- GH_TOKEN
- ANTHROPIC_API_KEY
- Infrastructure credentials

### LLM_SECRETS (LLM Can Access)
- BRAVE_API_KEY
- GROQ_API_KEY
- Browser login credentials

### User OAuth (Stored in ~/.config/)
- Google Calendar
- Google Drive
- Gmail

---

## 📊 Skill Status

| Skill | Type | Credentials | Status |
|-------|------|-------------|--------|
| brave-search | Executable | LLM_SECRETS | ✅ Ready |
| browser-tools | Executable | None | ✅ Ready |
| transcribe | Executable | LLM_SECRETS | ✅ Ready |
| youtube-transcript | Executable | None | ✅ Ready |
| vercel-deploy-claimable | Executable | None | ✅ Ready |
| vscode | Executable | None | ✅ Ready |
| gccli | Requires install | User OAuth | ⚠️ Manual setup |
| gdcli | Requires install | User OAuth | ⚠️ Manual setup |
| gmcli | Requires install | User OAuth | ⚠️ Manual setup |
| react-best-practices | Documentation | None | ✅ Ready |
| web-design-guidelines | Documentation | None | ✅ Ready |
| composition-patterns | Documentation | None | ✅ Ready |
| react-native-skills | Documentation | None | ✅ Ready |

---

## 🧪 Quick Tests

```bash
# Test search
/job/.pi/skills/brave-search/search.js "test" -n 3

# Test transcript
/job/.pi/skills/youtube-transcript/transcript.js dQw4w9WgXcQ

# Test browser
/pi-skills/browser-tools/browser-start.js
/pi-skills/browser-tools/browser-nav.js https://example.com
/pi-skills/browser-tools/browser-screenshot.js /tmp/test.png

# Test deploy
mkdir /tmp/test && echo '<h1>Hi</h1>' > /tmp/test/index.html
bash /job/.pi/skills/vercel-deploy-claimable/scripts/deploy.sh /tmp/test
```

---

**Security**: All skills passed security audit ✅  
**Docs**: See `/job/logs/installed-skills-guide.md`  
**Audit**: See `/job/logs/security-assessment-report.md`
