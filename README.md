可以使用 `docker build -t rk42745417/2025cloud:{tag} .` 指令進行打包，其中 `{tag}` 為自定義 tag。

接著使用 `docker run rk42745417/2025cloud:{tag}` 運行 image。

# 自動化邏輯
目前採用 `.github/docker-publish.yml` 內的 Github Action 進行自動化的 build 和 push。Github action 會在以下的事件進行操作：
+ 有新的 push 至 master 或 push 新的 semver tag
+ 有新的 PR 要 merge 到 master
+ 每天的 2:24 UTC。

會先 build image 為 `rk42745417/2025cloud:{tag}`，其中 `{tag}` 有幾種可能：
+ `master`：如果 push 到 master，或是 dispatch
+ `nightly`：如果是每天的 scheduled event
+ `v*.*.*`：如果是 push 了 semver tag
+ `pr-*`：如果是發 PR 至 master

如果不是 PR 觸發，則接著就會把該 image push 至 Docker hub。


