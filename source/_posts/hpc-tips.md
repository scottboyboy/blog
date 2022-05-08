---
title: hpc-tips
date: 2017-03-19 22:53:17
tags: [script]
---

最近遇到了这么一件事···
## 想要复制N份qbs脚本

`N = 10`

	    $ example for copy conten
	    #!/bin/bash
	    echo "start"
	    for i in {1..10}
	    do
	    cat > script.$i.pbs <<EOF
	    #!/bin/bash
	    #PBS -N test
	    #PBS -l nodes=1:ppn=1
	    #PBS -q AP100_queue
	    #PBS -j oe
	    cd  \$PBS_O_WORKDIR
	    proc=\$(cat \$PBS_NODEFILE | wc -l)
	    ./test_work_1.o >> qwer.log
	    EOF
	    done
	    echo "end"


被复制的内容是
<!-- more -->

		#!/bin/bash
	    #PBS -N test
	    #PBS -l nodes=1:ppn=1
	    #PBS -q AP100_queue
	    #PBS -j oe
	    cd  \$PBS_O_WORKDIR
	    proc=\$(cat \$PBS_NODEFILE | wc -l)
	    ./test_work_1.o >> qwer.log

- shell脚本的 ```#!/bin/bash``` 不可缺少
- 待复制的里面有 `$`符号，一定注意，要将 `\` 加在其前。具体参见shell编程  `=_=`
	
## 执行N份qbs脚本

- 脚本名称为`script.1.pbs`等等

		for i in {1..10}; do qsub script.$i.pbs ;done

*图片参考`失败`，具体以实物为准*


**参考文档1**  [Tutorial - Submitting a job using qsub](https://wikis.nyu.edu/display/NYUHPC/Tutorial+-+Submitting+a+job+using+qsub)

**待更**

