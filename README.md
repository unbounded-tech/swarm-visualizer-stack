# swarm-visualizer stack

before forking, create the following environment variables in Jenkins:

## `swarmVisualizerDomain`
Example: `swarm-visualizer.yourdomain.com`

Note: When adding a stack that uses a new domain name, it's recommened to configure the DNS before
deploying to avoid DNS caching issues on your local machine. This occurs when you attempt
to go to a domain before the DNS for it has been configured. The unconfigured resolution will
can get cached on your router or computer, which technically doesn't cause issues for anyone
but you, but it is annoying!
