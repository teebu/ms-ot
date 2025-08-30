# Copilot Setup:

1. create purview api plugin
   -> setup purview endpoint with credentials (req: client credentials and purview endpoint)
2. create onetrust pii data plugin
   -> setup endpoint and api (req: token from onetrust) credentials
3. create onetrust pii agent (make it available to everyone)
   -> go to agent list, deploy and enable timer

# Notebook:

1. deploy notebook (packager?)
   -> create cron job - what should the end time be? how does publisher figure it out / update it?.
