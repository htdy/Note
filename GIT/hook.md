* post-receive sample

```
#!/bin/bash
while read oldrev newrev refname
do
    branch=$(git rev-parse --symbolic --abbrev-ref $refname)

    if [ "check" == "$branch" ]; then
        echo "goroot:$GOROOT"
        echo "gopath:$GOPATH"
        echo "default GIT_DIR: $GIT_DIR"
        unset GIT_DIR
        echo "unset GIT_DIR: $GIT_DIR"
        echo "GIT_PUSH_OPTION_0:$GIT_PUSH_OPTION_0"
        echo "old:$oldrev"
        echo "new:$newrev"
        cd /export/gopath/mgq/src/projectdir
        git status
        git clean -df
        git stash
        git reset HEAD --hard
        git checkout check
    fi
    if [ "build" == "$branch" ]; then
        echo "goroot:$GOROOT"
        echo "gopath:$GOPATH"
        echo "default GIT_DIR: $GIT_DIR"
        unset GIT_DIR
        echo "unset GIT_DIR: $GIT_DIR"
        echo "GIT_PUSH_OPTION_0:$GIT_PUSH_OPTION_0"
        echo "old:$oldrev"
        echo "new:$newrev"
        cd /export/gopath/mgq/src/projectdir
        git status
        git clean -df 
        git stash
        git reset HEAD --hard
        git checkout build 
        cd /export/gopath/mgq/src/projectdir && export GOROOT=/usr/lib/golang && export GOPATH=/export/gopath/mgq && make build
    fi  
    if [ "release" == "$branch" ]; then
        echo "goroot:$GOROOT"
        echo "gopath:$GOPATH"
        echo "default GIT_DIR: $GIT_DIR"
        unset GIT_DIR
        echo "unset GIT_DIR: $GIT_DIR"
        echo "GIT_PUSH_OPTION_0:$GIT_PUSH_OPTION_0"
        echo "old:$oldrev"
        echo "new:$newrev"
        cd /export/gopath/mgq/src/projectdir
        git status
        git clean -df
        git stash
        git reset HEAD --hard
        git checkout release
        cd /export/gopath/mgq/src/projectdir && export GOROOT=/usr/lib/golang && export GOPATH=/export/gopath/mgq && make build_release
    fi
    if [ "mgqdeploy" == "$branch" ]; then
        echo "goroot:$GOROOT"
        echo "gopath:$GOPATH"
        echo "default GIT_DIR: $GIT_DIR"
        unset GIT_DIR
        echo "unset GIT_DIR: $GIT_DIR"
        echo "GIT_PUSH_OPTION_0:$GIT_PUSH_OPTION_0"
        echo "old:$oldrev"
        echo "new:$newrev"
        cd /export/pgsql/gopath/mgq/src/projectdir
        /export/pgsql/mgq/bin/stop_taskmanager.sh
        cp -f /export/pgsql/gopath/mgq/src/projectdir/bin/task_manager /export/pgsql/mgq/bin/
        cp -f /export/pgsql/gopath/mgq/src/projectdir/output/jdcloud-rds-booter2-*.tar.gz /export/pgsql/mgq/nginx/
        /export/pgsql/mgq/bin/start_taskmanager.sh
    fi
done
```
