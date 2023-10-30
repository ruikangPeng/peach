MozPeach 是 Mozilla Security 的 [Peach v2.7](http://www.peachfuzzer.com) 的一个分支。在我们的社区和合作伙伴的支持下，我们的目标是继续将 Peach 作为具有 Python 兼容性和新功能的开源产品提供。

我们的重点是可用性、速度和更少的依赖性。我们还开始了对 Python 3 的支持，替换了已弃用的 Python 依赖项，切换了 XML 后端，添加了新的配置系统，简化了代码等等。

### 设置

##### Prerequisites for Ubuntu
```
sudo apt-get --yes --quiet install libxml2-dev libxslt1-dev lib32z1-dev
```

##### General
```bash
pip install virtualenv
pip install virtualenvwrapper

git clone --depth 1 https://github.com/mozillasecurity/peach

cd peach
git clone --depth 1 https://github.com/mozillasecurity/fuzzdata

mkvirtualenv -r requirements.txt peach
```

或

```bash
workon peach
```

### Fundamentals
Peach 使用基于 XML 的 pits 作为配置文件。有两种类型的 pit，我们将在这里简要描述。

**Pit: Data Model**

数据模型 pit 是规范的 XML 描述，需要将任何类型的输入解析到内存中的 XML 树中。然后，Peach 使用该树生成模糊输出。

**Pit: Target**

Target pit 用于定义目标进程将如何变得模糊，如何监控其可疑行为以及如何处理结果。

是否将所有东西都放在一个 pit 中是可选的，但是不这样做将简化使用多个目标、不同主机和重用 pit 的过程。遵循数据模型/Target pit实践将允许跨项目重用数据模型 pit


### Examples

##### Run
```bash
./peach.py -pit Pits/<component>/<format>/<name>.xml -target Pits/Targets/firefox.xml -run Browser
```

**提示**: 你可以从命令行使用 -macros 为两个 pit 设置相关的配置值。

##### Debug
```bash
./peach.py -pit Pits/<component>/<format>/<name>.xml -1 -debug | less -R
```

**注意**: 这将显示解析过程的非常详细的输出。要仅查看每个元素的解析过程的结果，可以添加： "| grep Rating | less -R"


### 帮助菜单
```
% ./peach.py -h
usage: peach.py [-h] [-pit path] [-run name]
                [-analyzer ANALYZER [ANALYZER ...]] [-parser PARSER]
                [-target TARGET] [-macros MACROS [MACROS ...]] [-seed #]
                [-debug] [-new] [-1] [-range # #] [-test] [-count] [-skipto #]
                [-parallel # #] [-agent # #] [-logging #]
                [-check model samples] [-verbose] [-clean] [-version]

Peach Runtime

optional arguments:
  -h, --help            show this help message and exit
  -pit path             pit file
  -run name             run name
  -analyzer ANALYZER [ANALYZER ...]
                        load analyzer.
  -parser PARSER        use specific parser.
  -target TARGET        select a target pit.
  -macros MACROS [MACROS ...]
                        override configuration macros
  -seed #               seed
  -debug                turn on debugging. (default: False)
  -new                  use new relations.
  -1                    run single test case.
  -range # #            run range of test cases.
  -test                 validate pit file.
  -count                count test cases for deterministic strategies.
  -skipto #             skip to a test case number.
  -parallel # #         use parallelism.
  -agent # #            start agent.
  -logging #            verbosity level of logging
  -check model samples  validate a data model against a set of samples.
  -verbose              turn verbosity on. (default: False)
  -clean                remove python object files.
  -version              show program's version number and exit
```

### Resources

有助于根据文件格式的语法构建 pit 的资源：

    * http://www.sweetscape.com/010editor/templates/
    * http://www.synalysis.net/formats.xml
