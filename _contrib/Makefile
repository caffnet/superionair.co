# Place this one level up in the dir tree
# Modify for your environment
_HOME=`pwd`
GIT_REPO_HOME=/opt/superionair.co/jekyll-incorporated
HTDOCS_HOME=/opt/superionair.co/htdocs

all:
	cd $(GIT_REPO_HOME) && git pull && jekyll build --trace --source $(GIT_REPO_HOME) --destination $(HTDOCS_HOME)
	cd $(_HOME)
	chown -R nginx:nginx $(HTDOCS_HOME)
