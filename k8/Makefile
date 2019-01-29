#Authored by Phillip Bailey
.PHONY: all virtualbox aws vagrant_up  vagrant_clean
.SILENT:

DISTRO  = k8

all:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST)   | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

virtualbox: vagrant_clean vagrant_stop_all ## build virtualbox image
	rm $(DISTRO)-x64-virtualbox.box 2>/dev/null; true
	packer build -only=virtualbox-iso template.json

vagrant_up: ## starts the vagrant box
	vagrant up

vagrant_clean: ## stops and removes vagrant box
	vagrant destroy -f 2>/dev/null; true
	vagrant box remove file://$(DISTRO)-x64-virtualbox.box 2>/dev/null; true
	vagrant box list

vagrant_stop_all: ## stop all Vagrant machines
	vagrant global-status | grep virtualbox | cut -c 1-9 | while read line; do echo $$line; vagrant halt $$line; done;

vagrant_status: ## outputs status Vagrant environments for this user
	vagrant global-status