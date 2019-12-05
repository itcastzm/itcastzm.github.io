# Git 操作

## 清理已合并分支
```shell
git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d
```
## 同步清理后的远程分支
```shell
git pull --prune
```
## 输出维护者信息
```shell
# https://github.com/standard/standard/blob/master/bin/update-authors.sh
git log --reverse --format='%aN (%aE)' | perl -we '
BEGIN {
  %seen = (), @authors = ();
}
while (<>) {
  next if $seen{$_};
  $seen{$_} = push @authors, "- ", $_;
}
END {
  print "# Authors\n\n";
  print "#### Ordered by first contribution.\n\n";
  print @authors, "\n";
}
'
```