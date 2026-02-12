Perform a comprehensive security assessment of available Pi coding agent skills, then install all safe and compatible skills. Complete these tasks:

1. **Security Analysis**: 
   - Review source code of major skill collections (badlogic/pi-skills, vercel-labs/agent-skills)
   - Check for suspicious code execution patterns, network requests, file system access
   - Verify repository authenticity, maintenance status, and contributor reputation
   - Analyze any executable scripts for potential security risks
   - Document any concerns or red flags found

2. **Dependency Assessment**:
   - Review all npm packages and dependencies required by skills
   - Check for known vulnerabilities using npm audit or similar tools
   - Verify that global package installations are from trusted sources

3. **Sandboxing Verification**:
   - Confirm that thepopebot's env-sanitizer extension properly filters sensitive environment variables
   - Test that skills can't access credentials they shouldn't have access to
   - Verify the Docker container isolation is effective

4. **Safe Installation Process**:
   - If security assessment passes, install all compatible skills:
     - badlogic/pi-skills collection (brave-search, browser-tools, gccli, gdcli, gmcli, transcribe, vscode, youtube-transcript, subagent)
     - vercel-labs/agent-skills collection (react-best-practices, web-design-guidelines, vercel-deploy-claimable)
   - Use project-level installation (.pi/skills/) rather than global where possible
   - Set up proper configuration and test basic functionality

5. **Documentation**:
   - Create a security assessment report listing what was reviewed and findings
   - Document all installed skills with their capabilities and any security considerations
   - Create usage guidelines for skills that require special handling

**Security-First Approach**: If ANY skill collection shows significant security concerns, skip that collection and document why. Only install skills that pass security review.