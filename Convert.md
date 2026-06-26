### Goal:
the project contains a migration from wix web site to our own custom web site 
1. analyize the existing code 
2. run it to ensure its functional
3. check if the send email is functional
4. research and recommend on hosting the web site with the server , assume I have aws account but i have open minded to ANY platform in 2026 that is easy to set up quickly and allows fast productivity for low-traffic componets 

### Instructions :
1. Create 1-3 agents to perform the task and reach the goal

- Code Scan Agent : The first agent to run , maps the code , understand what the previous VibeCode tool did , read the old Plan.md and Readme.md and maps the project structure.  uses claude-4-8-sonnet[1m]

- Research Engine : looks for trends on how to host such web site and recommend on 
approaches for 2026. it can use brave search or tavily mcp for the research.
it needs to recommend which SaaS platform to use or whether to host it on my private AWS account . assume low traffic for this web site.  
prefer Free Tier SaaS platform like Base 44 and similiar offerings. uses claude-4-8-opus, runs in planning mode.



- QA Agent : ensure the changes we made meet the goal of converting the existing wix web site , ensure the site is running locally and that its functionaliy is working and can be work also from mobile devices and on many browser . may use Playwright mcp.
need to confirm whether the vibe coding agent made support for email sending , and if not -> do not add it .uses claude-4-8-sonnet[1m]

## Agent Team Prompts - Do's and Don'ts

| ✓ DO | ✗ DON'T |
|------|---------|
| Own specific files | Share same file |
| Define output | Vague deliverables |
| Name recipients | Assume the plan |
| Give full context | No history given |

## Agent mode instructions
1. use tmux mode
2. use different color for each different agent
3. once the agent finishes - stop it or close it . dont let it run /spin with idle job
use claude-4-6-sonnet[1m] for all agents , except the research which can go with claude-4-8-opus

