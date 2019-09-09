# git reset --hard 变基回到历史版本

## 操作流程

```
# 执行完下面命令后，【本地】代码就回滚到该历史版本
$ git reset --hard 3f75c3b455c4bc2184d5696939252c7b022f5928
HEAD is now at 3f75c3b4 闪租支付失败结果页

# status 查看，没有什么要提交的，并且还落后服务端若干个 commits 哦
$ git status
On branch 20190827_wwm_flashrent_pay
Your branch is behind 'origin/20190827_wwm_flashrent_pay' by 55 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean

# log 查看也是一样
$ git log --oneline
3f75c3b4 (HEAD -> 20190827_wwm_flashrent_pay) 闪租支付失败结果页
b7a1fcbe 🚀 vuex的mapActions

# 一开始用错了远程分支
$ git push --force origin master
Everything up-to-date

# 终于成功了，不要理会 merge request 到 master 的提示
$ git push --force origin 20190827_wwm_flashrent_pay
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: To create a merge request for 20190827_wwm_flashrent_pay, visit:
remote:   https://git.hzfapi.com/huifenqi/fe-huizhaofang/merge_requests/new?merge_request%5Bsource_branch%5D=20190827_wwm_flashrent_pay
remote:
To git.hzfapi.com:huifenqi/fe-huizhaofang.git
 + a5a21bf4...3f75c3b4 20190827_wwm_flashrent_pay -> 20190827_wwm_flashrent_pay (forced update)
```

操作成功后，本地和服务端都恢复到更早的版本，后续的 commits 历史也丢失了。完成！