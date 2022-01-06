install elastic-package

clone integrations repo
cd integrations/packages/k8s_cis
elastic-package build

create you own profile with elastic-package
in this profile, comment out "xpack.fleet.enabled: true" in the "kibana.config.default.yml" file.
why does this file take effect with kibana version >=8 ? idk. it seems like both config files are active in this case. but I'm not sure.

set up the env vars
general
eval "$(elastic-package stack shellinit)"
choose you image versions:
eval $(cat _dev/how_to_deploy.md)

later on, you'll have to build your own images for parts of this setup. remember that if you want them to run on kind cluster, you'll have to load them there after building them (kind load).

to start the stack:
elastic-package stack up -p your_profile

this starts with docker compose the follwing containers:
package registry
elastisearch
kibana
fleet-server agent
another agent

then. to later start an agent on a kind cluster:
first make sure you have a kind cluster :)
(inside k8s_cis) run
elastic-package service up
this will start an elastic agent on your kind cluster.
This happend since we have a _/dev/deploy/k8s dir in owr package. If you'd like to deploy more things add their yamls to this dir (see for exaple kubernetes integration).
If you'd like to build things add the build directory under "deploy". see examples in kubernetes package as well.

Now you have a full set up! rock on.
for now you'll ned our own agent image to make it work with our integration.
also check how tests are done for the kubernetes packages since we would like to do similar things.
