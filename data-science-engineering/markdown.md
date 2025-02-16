# 数据科学工程

Yanshuo Chu

[https://yanshuo.site](https://yanshuo.site)

[<i class="fa-brands fa-github fa-2xs"></i>](https://github.com/dustincys/)
[<i class="fa-brands fa-twitter fa-2xs"></i>](https://twitter.com/dustin13316197)
[<i class="fa-brands fa-weibo fa-2xs"></i>](https://www.weibo.com/chuyanshuo)

02/17/2025



## 如何高效、快速完成项目
![run](https://raw.githubusercontent.com/dustincys/slides/images/134233agtww9t4sl9wg5ut.jpg)


### 故事是怎么形成的
![idea](https://raw.githubusercontent.com/dustincys/slides/images/1342287r3v4r7aalavrcor.png)



## 数据科学常见工程问题
![刀耕火种](https://raw.githubusercontent.com/dustincys/slides/refs/heads/images/daogenghuozhong.jpeg)


### “刀耕火种”式数据分析
- 没有数据和代码的管理和备份（耕地不可重复利用）
- 单机（session）、逐行“复制粘贴”式计算（石器时代的耕作工具）



## 数据备份
<img src="https://raw.githubusercontent.com/dustincys/slides/refs/heads/images/data_backup.png"  style="height: 300px !important;" >

数据科学杂谈之四--数据存档：
https://yanshuo.site/cn/2022/07/datascience/


### 数据备份内容

- 数据校验码
- 数据信息说明
  - 数据来源
  - 合作者提供的数据信息
  - 数据分析者发现的数据信息
  - ...


### 数据校验
ranger的配置文件rc.conf中添加：
```bash
map ,md5 console shell -f find . -type f ! -iname "*md5sum.txt" | xargs -n 1 md5sum > md5sum.txt
map ,cmd5 shell -f md5sum -c md5sum.txt > check_md5sum.txt && cat check_md5sum.txt | awk '{s[$2]++};END{for(i in s) print i,s[i]}' > report_md5sum.txt && wc -l md5sum.txt >> report_md5sum.txt && wc -l check_md5sum.txt >> report_md5sum.txt
map ,vmd5 shell -f cat check_md5sum.txt | awk '{s[$2]++};END{for(i in s) print i,s[i]}' > report_md5sum.txt && wc -l md5sum.txt >> report_md5sum.txt && wc -l check_md5sum.txt >> report_md5sum.txt
```



## 代码备份
<img src="https://raw.githubusercontent.com/dustincys/slides/refs/heads/master/code_backup.png" style="height: 300px !important;" >

数据科学杂谈之六--自动备份：
https://yanshuo.site/cn/2022/12/datascience2/


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



## 工程架构
<img src="https://raw.githubusercontent.com/dustincys/cn/assets/scheme_of_path.png" style="height: 300px !important;" >

数据科学杂谈之一--工程架构：
https://yanshuo.site/cn/2020/10/datascience/


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
    - software
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



## 迭代反馈
<img src="https://raw.githubusercontent.com/dustincys/slides/refs/heads/images/feedback.png" style="height: 500px !important;" >


### 此处举一个反例
<img src="https://raw.githubusercontent.com/dustincys/slides/refs/heads/images/spatialinfercnv.png" style="height: 500px !important;" >



## 规模化管理工具

![奇异博士的传送门](https://raw.githubusercontent.com/dustincys/cn/assets/doctorstrange-drstrange.gif)

数据科学杂谈之五--奇异博士的传送门：
https://yanshuo.site/cn/2022/11/datascience/



## 其他

- 数据科学杂谈之十二--现场恢复 https://yanshuo.site/cn/2024/05/restorescene/
- 数据科学杂谈之十一--瞬时捕捉 https://yanshuo.site/cn/2024/05/datascience/
- 数据科学杂谈之十--算力分配 https://yanshuo.site/cn/2023/08/computing-power/
- 数据科学杂谈之八--日程管理 https://yanshuo.site/cn/2023/04/datascience_agenda/



## 推荐主要生产力工具
- [tmux](https://github.com/tmux/tmux/wiki)
  - 多任务/窗口/工程管理
  - 现场恢复
- [ranger](https://github.com/ranger/ranger)
  - 管理文件
  - 将命令映射为快捷键
- [emacs](https://www.gnu.org/software/emacs/)
  - 可在终端运行的编辑器
  - 具有RELP的编辑器



## 推荐主要生产力工具
- 环境管理
  - [conda](https://docs.conda.io/en/latest/)
  - [docker](https://www.docker.com/)
  - [singularity](https://docs.sylabs.io/guides/2.6/user-guide/singularity_and_docker.html)
- 流程管理
  - [nextflow](https://www.nextflow.io/)
  - [snakemake](https://snakemake.readthedocs.io/en/stable/)
- 版本管理
  - [git](https://git-scm.com/)



## 推荐主要生产力工具

- 并行/异步
  - [LSF](https://www.ibm.com/support/pages/what-lsf)
  - 动态监控
    - [MLFlow tracking](https://www.mlflow.org/)


### 之一：monocle3的参数寻优

#### 提交job
```bash
PROJECT_FOLDER=/path/to/folder
DATA_FOLDER=${PROJECT_FOLDER}/data
RESULT_FOLDER=${PROJECT_FOLDER}/result
CODE_FOLDER=${PROJECT_FOLDER}/code
PIPELINE_FOLDER=${CODE_FOLDER}/pipeline
SRC_FOLDER=${CODE_FOLDER}/src
KNOWLEDGE_FOLDER=${PROJECT_FOLDER}/knowledge
PIPELINE_NAME=6_Neutrophil_trajectory__monocle3
PIPELINE_PATH_NAME=6_Neutrophil_trajectory/monocle3
PROJECT_NAME=$(basename ${PROJECT_FOLDER})

OutDir=$RESULT_FOLDER/$PIPELINE_PATH_NAME
if [ ! -d $OutDir ]; then
    mkdir -p $OutDir
fi

runR="Rscript --no-save "

es=( $(seq 0.1 0.1 0.3) )
ms=( $(seq 5 5 10) )
gs=( $(seq 0.1 0.1 0.3) )

JOBFOLDER=$OutDir
SEURAT_OBJ=/path/to/Neutrophil.rds
START_CLUSTER=6

for tempe in ${es[@]}; do
    for tempm in ${ms[@]}; do
        for tempg in ${gs[@]}; do
            JOBNAME=monocle3_${tempe}_${tempm}_${tempg}
            if [ -f ${JOBFOLDER}/${JOBNAME}.o.txt ] || [ -f ${JOBFOLDER}/${JOBNAME}.e.txt ]; then
                rm ${JOBFOLDER}/${JOBNAME}.*.txt -f
            fi
            bsub \
                -J ${JOBNAME} \
                -o ${JOBFOLDER}/${JOBNAME}.o.txt \
                -e ${JOBFOLDER}/${JOBNAME}.e.txt \
                -cwd ${JOBFOLDER} \
                -q e40short \
                -W 2:00 \
                -n 1 \
                -M 50 \
                -R rusage[mem=50] \
                -B \
                -N \
                -u ychu2@mdanderson.org \
                /bin/bash -c "
                    source ~/.bashrc; ssh -fNT -L 5196:127.0.0.1:5196 user@host;
                    module load R/4.0.3;
                    ${runR} /path/to/trajectory.r \
                        --mbl ${tempm} \
                        --gdr ${tempg} \
                        --edr ${tempe} \
                        --out_dir ${JOBFOLDER} \
                        --in_seurat_obj ${SEURAT_OBJ} \
                        --start_cluster ${START_CLUSTER};
                    exit"
        done
    done
done
```


#### r代码
```R
#'--------------------------------------------------------------
#' filename : trajectory.r
#' Date : 2020-12-03
#' contributor : Yanshuo Chu
#' function: trajectory
#'--------------------------------------------------------------

print('<==== trajectory ====>')

suppressMessages({
    library(optparse)
    library(Seurat)
    library(SeuratWrappers)
    library(monocle3)
    library(tidyverse)
    library(Matrix)
    library(ggplot2)
    library(mlflow)
})

option_list <- list(
  make_option(c("-o", "--out_dir"),
    type = "character",
    help = "output folder"),
  make_option(c("-i", "--in_seurat_obj"),
    type = "character",
    help = "input seurat obj"),
  make_option(c("-s", "--start_cluster"),
    type = "integer",
    default = 6,
    help = "start_cluster"),
  make_option(c("-e", "--edr"),
    type = "double",
    default = 1.0,
    help = "euclidean_distance_ratio",
    metavar = "double"
  ),
  make_option(c("-m", "--mbl"),
    type = "integer",
    default = 10,
    help = "minimal_branch_len",
    metavar = "integer"
  ),
  make_option(c("-g", "--gdr"),
    type = "double",
    default = 1 / 3.0,
    help = "geodesic_distance_ratio",
    metavar = "double"
  )
)

opt_parser <- OptionParser(option_list = option_list)
opt <- parse_args(opt_parser)

seurat_obj <- readRDS(opt$in_seurat_obj)

exp_name <- paste0(opt$start_cluster, "_",
             opt$edr, "_",
             opt$mbl, "_",
             opt$gdr)

mlflow_set_tracking_uri(uri = "http://127.0.0.1:5196")
mlflow_set_experiment(experiment_name = "monocle3_neutrophil")

with(mlflow_start_run(), {
  mlflow_log_param("in_seurat_obj", opt$in_seurat_obj)
  mlflow_log_param("start_cluster", opt$start_cluster)
  mlflow_log_param("edr", opt$edr)
  mlflow_log_param("mbl", opt$mbl)
  mlflow_log_param("gdr", opt$gdr)

  figure_path <- file.path(opt$out_dir,
                          "monocle3_mlflow",
                          paste0("e_", opt$edr),
                          paste0("m_", opt$mbl),
                          paste0("g_", opt$gdr))
  if (!dir.exists(figure_path)) {
      dir.create(figure_path, recursive = TRUE)
  }
  setwd(figure_path)

  print("Begin generate monocle obj")
  monocle_cds <- as.cell_data_set(seurat_obj)
  monocle_cds <- cluster_cells(cds = monocle_cds, reduction_method = "UMAP")
  print("Begin learn graph")
  monocle_cds <- learn_graph(
    monocle_cds,
    learn_graph_control = list(
      euclidean_distance_ratio = opt$edr,
      minimal_branch_len = opt$mbl,
      geodesic_distance_ratio = opt$gdr
    )
  )

  start_cells <- Cells(seurat_obj)[
    seurat_obj$seurat_clusters == opt$start_cluster
  ]
  monocle_cds <- order_cells(monocle_cds,
    reduction_method = "UMAP",
    root_cells = start_cells
  )

  ptime <- pseudotime(monocle_cds, reduction_method = "UMAP")
  ptime_sd <- sd(ptime)
  mlflow_log_metric("ptime_sd", ptime_sd)
})
```


### 之二：bulk-RNAseq分析

![screenshort](https://raw.githubusercontent.com/dustincys/slides/images/Screenshot%202023-04-18%20at%2011.35.25.png)



## 谢谢
