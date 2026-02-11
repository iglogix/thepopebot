Set up an automated cron job to run the Financial Advisor daily market research at 8:30am EST on weekdays (Monday-Friday). The job should:

1. Add a new cron entry to `operating_system/CRONS.json` that:
   - Runs at 8:30am EST on weekdays using cron schedule `30 8 * * 1-5`
   - Uses type "agent" 
   - References the financial advisor task: "Read the file at operating_system/FINANCIAL_ADVISOR/FINANCIAL_ADVISOR.md and complete the tasks described there."
   - Is enabled by default

2. Ensure the cron job integrates properly with the existing CRONS.json structure

3. Test that the cron schedule is correct for weekday market open times

The financial advisor will perform comprehensive pre-market research using multiple Brave Search queries and generate a structured daily report at `operating_system/FINANCIAL_ADVISOR/FINANCIAL_REPORT.md`.