## 运行

###启动Elasticsearch

当你解压好了归档文件之后，Elasticsearch 已经准备好运行了。按照下面的操作，在前台(foregroud)启动 Elasticsearch：

```
cd elasticsearch-<version>
./bin/elasticsearch
```

- 如果你想把 Elasticsearch 作为一个守护进程在后台运行，那么可以在后面添加参数 `-d` 。
- **如果你是在 Windows 上面运行 Elasticseach，你应该运行 `./bin/elasticsearch.bat` 而不是 `./bin/elasticsearch` 。** 
- **此时就能够通过开发程序访问到Elasticsearch的数据库了。**

稍等片刻，显示started，打开浏览器，输入 [http://localhost:9200](http://localhost:9200/) ，或者直接在命令行中输入`curl localhost:9200`。注意windows下的curl命令已经去掉了单引号了。 

按下 Ctrl + C，Elastic 就会停止运行。 

### 操作
J以下两种工具，个人认为各有侧重，但认为都是可视化的工具。
####Head
ES集群状态查看、索引数据查看、ES DSL实现（增、删、改、查操作）
安装目录下运行`grunt server`
输入 [http://localhost:9100](http://localhost:9100/) 
####kibana
除了支持各种数据的可视化之外，最重要的是：支持Dev Tool进行RESTFUL API增删改查操作。
安装目录下运行`./bin/kibana.bat`

[http://localhost:5601](http://localhost:5601/) 

