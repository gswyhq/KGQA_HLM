# KGQA_HLM
基于知识图谱的《红楼梦》人物关系可视化及问答系统

1)  app.py是整个系统的主入口<br>
2)  templates文件夹是HTML的页面<br>
     |-index.html 欢迎界面<br> 
     |-search.html 搜索人物关系页面<br>
     |-all_relation.html 所有人物关系页面<br>
     |-KGQA.html 人物关系问答页面<br>
3)  static文件夹存放css和js，是页面的样式和效果的文件<br>
4)  raw_data文件夹是存在数据处理后的三元组文件<br>
5)  neo_db文件夹是知识图谱构建模块<br>
     |-config.py 配置参数<br>
     |-create_graph.py 创建知识图谱，图数据库的建立<br>
     |-query_graph.py 知识图谱的查询<br>
6)  KGQA文件夹是问答系统模块<br>
     |-ltp.py 分词、词性标注、命名实体识别<br>
7)  spider文件夹是爬虫模块<br>
     |- get_*.py 是之前爬取人物资料的代码，已经产生好images和json 可以不用再执行<br>
     |-show_profile.py 是调用人物资料和图谱展示在前端的代码

系统整体流程图：<br>
![流程](https://github.com/gswyhq/KGQA_HLM/blob/master/%E5%9B%BE%E7%89%87%201.png)

<hr>
* 部署步骤：<br>
1、启动neo4j数据库 <br>
gswyhq@gswyhq-PC:~/docker/neo4j/linshi$ docker run --detach --name=linshi7474 -e TZ='CST-8' -e NEO4J_AUTH=neo4j/gswyhq -p 1337:1337 -p 7474:7474 -p 7687:7687 --env NEO4J_dbms_allow__format__migration=true -e NEO4J_dbms_shell_enabled=true -e NEO4J_dbms_shell_host=0.0.0.0 -e NEO4J_dbms_shell_port=1337 neo4j:3.2.5 <br>
2、构建一个运行demo的容器 <br>
gswyhq@gswyhq-PC:~/github_projects/KGQA_HLM$ docker run -i -t --detach --name=kgqa_hlm_18000 -p 18000:8000 --volume=$PWD:/kgqa_hlm --workdir=/kgqa_hlm -v /etc/localtime:/etc/localtime ubuntu:0601 /bin/bash <br>
3、进入容器 <br>
gswyhq@gswyhq-PC:~/github_projects/KGQA_HLM$ docker exec -it kgqa_hlm_18000 /bin/bash <br>
4、下载ltp模型，并拷贝到容器中 <br>
gswyhq@gswyhq-PC:~$ docker cp /home/gswyhq/data/FinancialIE/FinancialIE/financialie/ltp_data kgqa_hlm_18000:/kgqa_hlm/ltp_data_v3.4.0 <br>
5、安装依赖 <br>
root@9d3568ff0e4a:/kgqa_hlm# pip3 install -r requirement.txt -i https://pypi.douban.com/simple <br>
6、向neo4j中写入所需要的数据 <br>
root@9d3568ff0e4a:/kgqa_hlm# python3 neo_db/creat_graph.py <br>
7、运行程序 <br>
root@9d3568ff0e4a:/kgqa_hlm# python3 app.py <br>
8、测试 <br>
浏览器打开 http://localhost:18000 即可 <br>
