# 数据科学工程

Yanshuo Chu

[https://yanshuo.site](https://yanshuo.site)

[<i class="fa-brands fa-github fa-2xs"></i>](https://github.com/dustincys/)
[<i class="fa-brands fa-twitter fa-2xs"></i>](https://twitter.com/dustin13316197)
[<i class="fa-brands fa-weibo fa-2xs"></i>](https://www.weibo.com/chuyanshuo)

04/17/2023



## 数据科学常见工程问题

- 工程烂尾
  - 备份烂尾
  - 场地烂尾
- 算力不足
  - 硬件利用率低
  - 软件无效内耗
- 思维僵化
  - 自由软件“伪自由”
  - 舒适区不“舒服”



## 软件工具推荐
- [tmux](https://github.com/tmux/tmux/wiki)
  - 多任务/窗口/工程管理
  - 现场恢复
- [ranger](https://github.com/ranger/ranger)
  - 管理文件
  - 将命令映射为快捷键
- [emacs](https://www.gnu.org/software/emacs/)
  - 可在终端运行的编辑器
  - 具有RELP的编辑器



## 数据备份

- 数据压缩包
- 数据校验码
- 数据信息说明
  - 数据来源
  - 合作者提供的数据信息
  - 数据分析者发现的数据信息
  - ...

详细参考： [数据科学杂谈之四--数据存档](https://yanshuo.site/cn/2022/07/datascience/)


### 压缩
```bash
tar –czf jpg.tar.gz *.jpg
```


### 数据校验码
ranger的配置文件rc.conf中添加：
```bash
map ,md5 console shell -f find . -type f ! -iname "*md5sum.txt" | xargs -n 1 md5sum > md5sum.txt
map ,cmd5 shell -f md5sum -c md5sum.txt > check_md5sum.txt && cat check_md5sum.txt | awk '{s[$2]++};END{for(i in s) print i,s[i]}' > report_md5sum.txt && wc -l md5sum.txt >> report_md5sum.txt && wc -l check_md5sum.txt >> report_md5sum.txt
map ,vmd5 shell -f cat check_md5sum.txt | awk '{s[$2]++};END{for(i in s) print i,s[i]}' > report_md5sum.txt && wc -l md5sum.txt >> report_md5sum.txt && wc -l check_md5sum.txt >> report_md5sum.txt
```



## 代码备份

- 可回溯历史
- 可全文检索
- 可版本管理

详细参考：[数据科学杂谈之六--自动备份](https://yanshuo.site/cn/2022/12/datascience2/)


### 服务器端备份脚本
`backupAllCode.sh`:
```bash
##!/usr/bin/env bash
rm -rf /path/to/CodeBackup
rsync -rav \
      --include="*/" \
      --include='*.R' \
      --include='*.r' \
      --include='*.sh' \
      --include='*.py' \
      --include='*.md' \
      --include='*.org' \
      --exclude='*' \
      --exclude=".RD" \
      --exclude=".RH" \
      --prune-empty-dirs \
      /path/to/project/ \
      /path/to/CodeBackup/ && \
    zip -r /path/to/configs.zip /path/to/configs &&
    cd /path/to/CodeBackup/ &&
    git add --all &&
    git commit -m $(date '+%m-%d-%Y') &&
    git push origin master
```


### 本地定时自动触发脚本
1. 运行
```bash
crontab -e
```
2. 添加
```bash
0 15 * * 3 /path/to/backupServer.sh
```
3. `backupServer.sh`文件内容：
```bash
##!/user/bin/env bash
ssh your.server.domain "/path/to/backupAllCode.sh" && \
scp your.server.domain:/path/to/cnofigs.zip /path/to/local/folder
```



## 文件夹结构

- 树型结构
- 共有/私有文件

详细参考： [数据科学杂谈之一--工程架构](https://yanshuo.site/cn/2020/10/datascience/)


### 树型文件夹结构举例
```
- project1
  - code
    - pipeline
      - public
      - private
        - dataset 1
        - dataset 2
    - src
      - public
      - private
  - data
    - dataset 1
    - dataset 2
  - knowledge
    - public
    - private
      - dataset 1
      - dataset 2
  - info
  - result
      - dataset 1
      - dataset 2
```


### 自动化生成工程文件夹结构
1. 在任意位置（最好${HOME}/templates）手动创建以上文件夹（除了dataset）
2. 创建一个脚本`newproject.sh`:

```bash
##!/usr/bin/env bash
PROJECT_NAME=$1
cp -r /path/to/templates/project ./${PROJECT_NAME} && \
cd ./${PROJECT_NAME} && \
git init
```



## 环境结构

- 环境管理
  - [conda](https://docs.conda.io/en/latest/)
  - [docker](https://www.docker.com/)
  - [singularity](https://docs.sylabs.io/guides/2.6/user-guide/singularity_and_docker.html)
- 流程管理
  - [nextflow](https://www.nextflow.io/)
  - [snakemake](https://snakemake.readthedocs.io/en/stable/)
- 版本管理
  - [git](https://git-scm.com/)
  - [SVN](https://subversion.apache.org/)



## 充分利用算力

- 并行/异步
  - [LSF](https://www.ibm.com/support/pages/what-lsf)
  - 动态监控
    - [MLFlow tracking](https://www.mlflow.org/)



## 自动化/智能化操作流程

- 自动生成任务
- [自动备份](https://yanshuo.site/cn/2022/12/datascience2/)
- [智能跳转](https://yanshuo.site/cn/2022/11/datascience/)
- ..



## 开放思维

- 充分使用自由软件
- 架构优于细节



## 谢谢
