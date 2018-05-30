---
title: 【UIpath】专栏
type: examples
order: 55
---



## UiPath的一些Activities和Variables

From  ZHIHU https://www.zhihu.com/people/chen-min-min-62/activities

前言

        本来想写一篇关于我常用的activities、Variables的文章，但是拖拖拉拉的半年，终于鼓起了干劲写出来。在动笔之前我又查了一下百度，发现UiPath在中国还是没啥资料，有得都是一些英语生肉。在此我再次抛砖，分享一下我对UiPath的一些常用的activities，Variables需要注意的地方。
常用Activities

        Excel Application。如果企业做RPA的话，那么肯定需要用到Excel Application。因为Excel在很多企业中都是作为操作和储存数据的主要工具。无论是什么报表清单，UiPath都能操作。但是需要注意的地方有：

1. Excel worksheet尽量不要有merge的单元格（合并的单元格），不然会导致读出来的data table数据位置和预想中的不同。

2. 在读取已保护的workbook时，如果只是读取不写入的话，请把Excel application 里的autosave默认的钩去掉，否则会出错。

3. 在使用read range activity读取work sheet时，请注意读取的范围。如果不写入具体范围，则UiPath默认读取已使用过的单元格作为读取范围。如果在add header里打了勾，则默认读取范围的第一行作为header，同时第一行空白的单元格，UiPath会从左到右增加header为“column1， column2"以此类推。

4. 使用write range activity时，需要填入的是data table数据类型；而write cell activity则是string数据类型。而在使用这两个activites时，(range或者StartingCell)那里需要填写Excel的地址如"A1”“B2“之类。不能像vba那样写cells(row,column)。所以，要把数字转成列标，可以用chr()这个公式来转换。（图加例子）

        Assign。Assign这个应该是UiPath中最基本的一个activity了。我们通常用它来进行赋值。但是，其实它除了赋值以外，它还能做以下动作。

1. 初始化list, datatable, dictionary等的数据类型。 这是我初始化一个list来写的例子。test_List = new system.collections.generic.list(of variable)

2. 在赋值的同时进行运算和操作数据。比如split string，数学运算等。

3. 对data table进行一些select, sort的操作，请看以下例子

data_row[] = data_table.select([column_name] = *) （仅作select例子）

{

先用assign：data_table.DefaultView.Sort = （“[column_name]ASC")

再用assign：data_table = data_table.DefaultView.ToTable

}（仅作sort例子）

        Invoke workflow。这个activity是用来引用其他UiPath xaml文件时需要用到的。同时，invoke workflow还能进行参数的传递。对于一些重复使用的动作，我们可以将其打包成一个xaml文件，每次使用都直接引用，对整个project来说有一定的解耦作用。这个activity的重点在于参数的传递，亦就是Arguments。Arguments需要注意的点有：

1. 和普通的variables不同，Arguments多了一个direction的选项，就是in, out, in/out。这个方向是站在被invoke的xaml文件角度来看的。比如，我在A project使用了invoke workflow，加入了B project，需要将变量test从project A传入到 project B。那么在B project里新建Argument时，方向就是in了。

2. 方向in可以设置默认值，方向out不能。

        Connect。这个activity是用来连接data base的。具体怎样连接，这个activity有很好的向导，我在此就不再赘述。那么连接完以后怎样进行SQL进行增删改查，需要用到以下两个activities。

1. 用Execute non query进行“增删改”的动作。SQL语句写在sql那一格里，数据类型是string。写法就和普通的SQL一样。INSERT INTO table_name (column1, column2) VALUES (value1, value2)。大致就是这样，写进去就好。那么这个activity output出来的是一个int32类型的数，用来检查SQL执行的次数。

2. 用Execute query进行“查”的动作。同样的SQL语句写在sql那一格里，数据类型是string。写法就和普通的SQL一样。SELECT  * FROM table_name。例子大概就这样。这个activity output出来的是一个data table的数据类型。
常用的Variables

        Array。数组是比较常用的数据类型。数组的长度是固定的，但在UiPath中依然可以运用其他方法添加或减少数组的长度。

1. 使用assign activity减少数组长度。从左往右取截取数组长度：testArray = testArray.Take(your_length).ToArray()。从右往左截取数组长度：testArray = testArray.skip(testArray.take(TestArray.count - your_length).count).ToArray

2. activity增加数组长度。我暂时也没找到简单的法子。目前我是另外新建一个临时数组，增加其长度，再用for each把主数组的元素放到临时数组里。再用assign，把临时数组赋值给主数组。

List。List比Array好用太多了。长度不固定，增减都很简单。而且UiPath有特定的activities来支持List。也不要纠结List占用的内存比Array多，RPA项目的数据量一般不大。大数据处理请求助于其他更给力的语言或者工具。

1. 新建了变量后，使用list前需要用assign初始化（文章assign部分有提及）

2. 增减查List里的元素，可以使用 add to collection, remove from collection, exists in collection 这3个activities。

        Data table。对于data table的读取，很大一部分是对这个data table进行切片。

1. 如果读取其中一个元素，可以用 get row item 这个activities。但是如果想用assign的方法来动态抓取data table里的元素时，可以写成: test_table.rows(index).item(column_name)（前提是这个data table有header，如果没有header，也可以用index。

2. Data table行循环时，可以用 for each row 这个activity。那么在for each row里抓取其中一个元素，则可以在assign里写成 get_value = row.item(column_name)

3.Data table列循环时，可以用for each这个activity。在in 写 test_table.columns， 在 TypeArgument里选system.data.datacolumn

4.在用get row item 去抓取data table的某个元素时，如果目标元素是空白的，那么output出来的是nothing。如果用assign去抓取data table的某个元素，目标元素是空白的，那么output出来的则是""。注意这个地方，我在这里栽了好多次。

以上是读取data table内容时需要注意的地方。下面是新建data table的一些用法。

5. 新建一个新的data table，有两种方法。前提都是先新建好一个data table 变量。方法一：用 Build Data Table这个activity。这个方法的好处是你可以直接画好需要的行和列，很直观，就是创建的时候会浪费很多时间。方法二：用assign这个activity。上面我刚讲过可以用test_table = new system.data.datable 来新建data table，方便快捷。

6. 对于data table增加行列，可以用add data row 和 add data column这两个activities。虽然add data column里的Properties项目比较多，但也不难，只要确定好column在哪个table ，column name，column要求的数据类型就好，一般不报错。而add data row则是受限于data column。如果数据类型不对，或者在ArrayRow里写的数组长度超过column数目，则会在运行中报错。

7. data table写入数据，可以是在add data row里写入的，也可以用Build Data Table时同时写入。

Dictionary。字典的实用性无需叙言。

1. 新建Dictionary 先创建变量。方法一：用assign activity。 test_Dictionary = new system.collections.generic.dictionary(of Tkey, Tvalue)。方法二：用Build Dictionary来创建。本人倾向于用方法一。

2. 录入数据时，只能用add to dictionary。为避免在运行中出现重复key的情况。所以一般在会在add to dictionary前加 key Exists in Dictionary 和 If，用来判断是否有重复key。

3. 读取Dictionary，可以用for each循环，也可以像切片一样，用assign只抓一个key或者value。抓key的值时：test_key = test_Dictionary.keys(index)。抓value时：test_value = test_Dictionary.item(key)

4. 删除某条目，可以用 remove from Dictionary。

5. 如果担心内存占用太多，可以在用完dictionary之后，用clear dictionary。
后记

本文并不能涵盖UiPath中所有的Activities和Variables，我只是挑选其中一些出来讲讲特点和怎样用。而且，本文并不是手把手教学手册，有一些基本的内容我就当作大家都懂的前置知识而没写出来。如果发现自己的UiPath没有文中的activities，请在UiPath中按ctrl + p，安装完所有的包，那么肯定可以看到文中的activities的。希望大家看完以后会有所收获。





## 如何高效学习Uipath Studio？
网上找了下并没有找到相关资源。。。求助各位大神，谢谢！


陈敏敏
陈敏敏
我是男的= =

不知道题主是想因为工作需要还是出于自己的兴趣而想学习UiPath。如果是出于自己的兴趣而学习，建议还是学学Python之类的比较好。如果是出于工作需求，那就继续看下去吧。

UiPath标榜的是让编程人员能在无编程基础的情况下做编程。这样的好处是让有自动化需求而无专业编程人员的企业可以组织到员工学习使用。看上去学习成本很低，不需要记得各种语法，只需要学会按照流程把各个功能框连接起来，就可以操纵鼠标键盘和各种程序。功能框的功能必须细分到每一个鼠标单击的动作和键盘按下什么键。就像你在教导一个什么都不会的机器人，去像人一样操作电脑。如果觉得不知道怎么下手开始编UiPath，它还有录制的功能，把你的动作都录制下来，然后自动生成流程。这个录制功能就像Excel的录制宏。看上去一切都很完美。

但是！你以为用UiPath不需要编程的知识就大错特错了！UiPath本质上就是还是编程，哪里需要循环哪里需要判断一个都不能少，而且如果编制的时候逻辑不清，流程线乱拉，UiPath随时给你卡死，还不知道report回来。还有，各种变量的类型也是编制者的噩梦，变量的类型可以细分到一个长长的列表，而不单单是string之流。

好吧，先不吐槽。其实回答这个问题我更多的是想吐槽这个工具，因为我在用着……

下面是一些使用UiPath的建议，谈不上什么教程。

第一，建议有一定编程基础的人员去学UiPath。

第二，不要把一个项目定得太大，UiPath不稳定，跑的时间越长，卡死的机率越高。

第三，定义变量时，变量名称要清晰，变量类型要准确，变量作用范围要选好。

第四，流程图最好不要摊大饼式地铺开，最好用sequence和workflow组合一系列的动作。workflow之下还能再有workflow的。

第五，能click element就不要click image。

第六，常用write line检测变量

大概就是这些，想到什么就说什么，乱糟糟的。我猜题主是刚接触UiPath吧？以上只是我在使用UiPath时的一些感想，我认为最重要还是要自己亲手做项目，这样才是最高效的学习方法。然后你就会是这个表情了(´･_･`)


============分割线=====================

三个月后的一点小更新，希望对阅读本答案的人有一些帮助。

对于第五的修改更正：Click image其实也是要认element的，所以有时候click element和click image可以用在同一个地方。click image实用经验：在工作中我遇到过某些银行网站对于web element的标签做了特殊处理。每次刷新网站，对于同一个按钮，UiPath都认到不同的element。这个时候用click image，同时在selector中把特殊处理字段用*号代替，就能绕开这个特殊处理。

对于第六的额外建议：请不要不要write line太多在output bar，UiPath会卡死的。如果实在需要输出大量文本数据检查，可以用write txt file，生成一个txt文件进行检查。也可以生成一个data table写到Excel里面。

UiPath不稳定，用过的人都会吐槽。我认为原因有以下几个。

第一个原因是如果编写的时候考虑到多种情况时，整个流程会因此增大好多，臃肿不堪的话，UiPath在try catch的过程中会浪费大量时间，而效率在工作中也是很重要的。因此鉴于效率的问题，编程人员不把小几率的事件写上去，因而在遇上小概率事件时会出错。

第二个原因是，本身UiPath这款软件的平台也不甚完善，我们会发现很多不能理解的bug（可能是在下能力有限）。所以只能绕过这个功能，选用其他备用方案。

第三个原因是，运行环境杂乱。如果在运行UiPath的机子会有各种弹窗，各种提醒的话。那么UiPath在识别element的时候会被这些不经意的弹窗所打断。所以最好有一台特定的干净的机子来跑UiPath的机器人。

第四个原因是，开发中对于开发项目的工作流程理解不透。我觉得这个是最大的原因。之所以要开发机器人代替人手，是因为这些人手重复工作让机器人代替会更加有效率，使人力成本降低。但是这不意味着这些人手工作就不繁复的。重复不代表不繁复（拗口）。在理解项目时，必须仔细地看清楚人手操作的动作，认真地理顺人手操作背后的工作逻辑，大胆地提出新的工作逻辑，耐心地与相关工作人员了解情况和互动。这样子写出来的机器人，稳定性肯定提高不少。

目前工作中总结到的原因有以上四个。但是多思考第四个原因，可以让你少了很多维护的烦恼～


谢谢阅读：）

持续更新中。。。。
编辑于 2017-07-16


Christine
Christine
INTJ/Robotics/怒刷日语中....

UiPath官方网站就有自学资源呀:

UiPath Studio Guide: https://studio.uipath.com/v2016.2/docs/introduction

UiPath Academy: https://academy.uipath.com

UiPath自己做的在线课程,最近在刷的就是这个. 免费注册,十分良心. 内容例子什么的都很好,还带中文字幕,就是画面太渣了. 现在只有foundation和SAP.

他们家还有论坛让新手/老手交流经验: UiPath Community Forum - Robotic Process Automation

不知道高效学习是要多高效, UiPath挺好上手的,网上有教程,activity也很多,跟着教程看应该没什么问题. 相比之下,Automation Anywhere培训是要钱的! (两天2000刀!) Blue Prism功能虽强大,界面实在太简陋,什么都要自己设,UiPath很好了好嘛! 如果对RPA有其他问题可以私信我 :) 

发布于 2017-09-08




黑风岭的牛魔王
黑风岭的牛魔王
程序猿

闲得蛋疼，特意上来知乎想看看有没吐嘈这个软件的，也算给新人或者对这个东西有想法的人提个醒。 我前身是程序猿，因为烦了写代码的公司无一例外的要加班，加上身子骨确实不太好，怕过劳死，然后找了个相对轻松的公司；以下是我对这个产品的吐嘈，完全的吐嘈，无赞美： 

1，UiPath公司对外宣称无编程基础的人可以傻瓜式编程，这个是个天坑。UiPath是基于 .Net 开发的集成工具，即是原来的程序猿是通过代码来写程序，但这个是通过画画图，拉拉线，让实现了简单的程序，听上去很美，但其实不然。1）没有程序基础的人就算是画画图，拉拉线，也有可能做错；2）本身UiPath也支持类似变量，循环等编程的基础要素，不懂得这些，你开发的UiPath也是很难运行，可以实现的功能不多；3）也是我怨念最大的，就是像我们专业程序猿转过来的，你写的所有所有东西，都依照了UiPath本身的规范，但最后，你会发现，还是会有各种意外情况，而这些东西，你根本无法避免，因为你本身编写的，只是一个xaml文件，但执行的东西是平台的，你看不见也修改不了。所以结局就是：傻逼，卧了个槽；

2，UiPath平台的本身，我用过的几个版本中，基本无法实现兼容，哪怕向下兼容也不行，即是如果你原来写的东西是8.0版的，你转到8.1去，那极大可能你是要重写的；

3，UiPath这个东西，不稳定，是相当的不稳定，今天运行的好好的robot，到明天，就可能会运行不了或者结果不对。你永远无法知道你哪一步出了错，有没办法让他稳定下来；

如果想实现自动化的，我建议，还是python + shell脚本来吧。UiPath，就是一个蠢B。


补充一点：

本人一年前在这个贴子吐嘈了uipath这个平台，一年后，有些改观的东西，作个补充说明：

1，现行的uipath版本，比本人一年前或者更久远的版本，在稳定性上有了很大的提高，目前实现了7*24运行的robot大概有15个左右，基本达到实际业务部门的要求，离200个尚有比较大的距离；

2，按本人了解，国内目前rpa相对比较热门，但成型的公司并不采用uipath或者bp或者aa这样的平台，而是自行建造。主要在于平台license及数据保护的角度考虑；

3，uipath的开发，本质就是.net开发。.net开发，目前只能说不太受国内市场欢迎，不是这个工具不好，而且社区不发达，建造成本高（你不知道你究竟要给微软多少钱）

4，本人一年前推销过python + shell的rpa开发搭配，一年后，我决定否定了这个，包括我现在带的rpa开发团队，统一摒弃了python的开发标准。不是python不好，而是Python相对uipath来说，开发成本还是太高。这个成本主要体现在部署及维护成本，uipath的开发，要求快速，简单，维护升级成本低，而python开发的rpa，对实际开发人员与维护人员，要求的技术标准并不低，这样的人力成本，是背离了rpa的本质的。



## Uipath Studio初步认识



一、引子
偶然的机会，公司财务部实施一个机器人项目，我负责跟踪并学习一下机器人。
于是乎，在前后两个月里，我除了忙手头的工作外，就在跟踪这个财务机器人实施项目(具体名字不说了)，原来用的就是一个叫做Uipath的东东。经过一个多月时间的忙前忙后，既要一同参与用户需求调研、跟踪状况，又要学习大致看懂理解Uipath自动化代码及时提出问题点，以免请来的专业人士开发的东西和用户真正想要的需求不一致，我的职责就没担当好，在两位非常优秀的Uipath技术顾问(这两位小伙子Uipath水平挺牛的，人也聪明)努力帮助下，最后成功上线。从我的角度看，上线后达到预期效果，原来四十人要做的工作，大部分可以机器人做，只需要几个人扫尾一下小部分机器人没处理到的工作即可。在这个项目过程中，我从对Uipath完全都没听说过开始，慢慢地阅读学习它的流程图式代码，有什么不懂得问题就问两位牛人，他们也很乐意分享他们的知识，知无不言，在此也表示感谢！
在实施完该项目后，在接下来的两个月里，我试着开发实施了另一个应用Uipath的机器人小项目，对认识什么是Uipath才有点感觉，同时也使我感觉到它还许多我没认识的强大灵活的地方。由于关于Uipath相关的国内学习资料少得很(或许是我孤陋寡闻没接触到)，几乎没有找到，知乎上只有寥寥几篇Uipath介绍的文章贴，那么，我既然接触了一下这个东西，我就觉得应该分享一下我的认识，但并不含有替Uipath Studio公司宣传的成分。

二、认识Uipath Studio
1．什么是机器人，机器人是自动执行工作的机器装置，既可以接受人类指挥，又可以运行预先编排的程序。更高级的可以根据人工智能技术制定的原则纲领行动，智能化是未来发展方向。关于机器人更详细的介绍、分类、应用等知识在网上搜索一下自行脑补，限于篇幅在此不做赘述。

2．提出一个概念名词，RPA(Robotic Process Automation)，中文翻译全称叫做机器人流程自动化，是基于计算机编程以及基于规则的软件，通过执行重复的基于规则的任务来将手工活动进行自动化的一种技术。据悉RPA在国外在2000年左右兴起，目前这个市场已经相对成熟，从国外对RPA技术定义来看有几个显著特点：
a.处理高度可重复任务：通过软件编程语言实现的机器人可以处理重复的人工任务；
b.基于明确的规则操作：流程必须有明确的、可被数字化的触发指令和输入，流程不得出现无法提前定义的例外情况；
c.以外挂的形式部署在客户现有系统上：基于规则在用户界面进行自动化操作，非侵入式模式不影响原有IT基础架构；
d.模拟用户手工操作及交互：机器人可以执行用户的日常基本操作，例如：鼠标点击、键盘输入、复制/粘贴等一系列日常电脑操作。
说到这里，大家可能就明白了，RPA就是最纯粹的自动化形式，主要是针对企业现有信息系统提供的外挂自动化软件，没有改变现有流程，只是机器替代人工来执行高度可重复的工作，提高了效率，节省了人工，将人从简单、重复的工作中解放出来，去做更多有高附加值的工作。
当前，RPA在国内推进得如荼如火，尤其是在财务领域的业务，财务四大中已有三家已面向客户推出RPA会计机器人业务解决方案，另外一家也宣布拟将IBM的”沃森(Watson)”认知计算应用到其审计服务中，让其能够分析更多的数据，从而能够更加深入地了解客户的财务和业务运营状况。那么，认真分析业务、人工效率成本后，合理采用RPA能给企业带来价值是毋庸置疑的。
3．人工智能技术，人工智能（Artificial Intelligence），英文缩写AI，人工方法在机器上(计算机)实现的智能，智能包含的能力：感知能力、记忆和思维能力、学习和自适应能力、行为能力。它是研究开发用于模拟、延伸和扩展人的智能的一门新的技术科学。现在比较有名的例子是Alpha Go围棋、无人驾驶车、最近又有Boston Dynamics出了个后空翻机器人等。其实人工智能领域历经60年的发展，经历了从爆发到寒冬再到野蛮生长的历程，近期随着云计算、大数据技术的支撑，伴随着人机交互、机器学习、模式识别等人工智能技术的提升，人工智能成为这一技术时代的新趋势，将来发展高度智能的机器人对人类来说也是一个很有趣的课题。

从以上2、3的说明并比较就能看出，AI与RPA完全不同，AI的应用场景更广阔，应用范围更大，应用影响更深远。AI结合了机器学习和深度学习等技术，能够不断重新修正自己的计算模型，不断改善自己的计算、反应能力。AI具有自我学习能力，因此它可以在学习过程中不断适应、改变，甚至还能够直接掌握用户的行为习惯。反之，RPA则仅仅能完成人类预先要求做的事，且是一遍又一遍的反复做。RPA是不改变原有的系统和流程，只是把需要人工操作的部分变成机器代替人来操作，而随着AI技术在企业应用场景，尤其是财务场景的应用，并且结合云计算、大数据、移动等相关的技术，企业原有的流程本身会发生变革，会变的更加高效简洁自动化、智能化。但是，在社会化商业发展，流程自动化、数据资产化、组织灵动化、管理智能化还未完成之前，在企业原有的流程本身还未发生变革，变得智能化之前，RPA应用则是企业高效简洁自动化发展道路上的良好选择。
4．Uipath Studio是什么，
4.1.上面说了这么多机器人、机器人自动化和人工智能的事情，那Uipath和它们有什么关系呢？它到底是机器人还是人工智能？Uipath Studio其实是一种RPA软件（事实上，RPA软件还有其它如Automation anywhere、blue prism等。由于我只接触过Uipath，所以本文只介绍Uipath）、强调自动化。是当前人气较高的RPA软件之一(刚刚还获得CognitionX组织大赛的企业人工智能使用奖 和人工智能使用杰出成就奖两项大奖)。UiPath Studio创始人和首席执行官丹尼尔-丹尼斯(Daniel Dines)表示：“RPA能够帮助组织机构更好、更快、更可靠地开展业务。它还能让雇员从重复枯燥的工作中解放出来。这一技术正在改变我们对工作的认知以及我们开展工作的方式。未来，那些现在学习掌握这一技术的人士将会走在前沿。” UiPath Studio能够提供完整的软件平台，帮助组织机构高效地进行业务流程自动化，具有快速配置、外挂非侵入性、快速回报等优点。
4.2.作为技术者的角度认识，Uipath Studio是一个应用集成完整解决方案和自动化的第三方应用，一个最重要的概念是自动化project。一个project是业务流程的图形表示，它通过给你自定义设置执行顺序和关系的完全控制，在Uipath Studio中也称之为活动，来驱动你自动的以规则为基础的过程。每个活动都包括一个小动作，如单击按钮、读取文件或写入日志面板。支持项目的主要类型是：
Sequences – 适合线形流程，使你能够顺序地从一个活动到另一个活动，而没有弄乱你的project；
Flowchart – 适合更复杂的业务逻辑，使你能够通过多个分支逻辑操作集成决策并以更多样的方式连接活动；
State Machines – 适用于非常大的项目；它们在执行过程中使用有限数量的状态，这些状态由条件(转换)或活动触发。

它是一款易用的开发工具，入门似乎较简单，uipath提供业界最直观、功能丰富的自动化开发环境。高度视觉流程设计师配备员工快速和容易地配置机器人工作流程。只需将活动拖放到工作流中，或者使用记录器使其运行。这种独特的功能，记录你平时的日常工作和自动重播。
它提供了丰富的可扩展性，是开放的，可扩展的，允许您自动化复杂的过程，否则无法覆盖。内置模板操作的库使自动化成为一种舒适而有效的体验。为了使其完整，用户可以完全自由地设计自己的自定义操作。对于你自定义的复杂功能可以nupkg结尾的文件包的形式嵌入你的自动化project中。
作为一个初级使用者，从网上也看到一些Uipath使用者的体会，有一点也是非常赞同的，无编程基础的人要做好一个Uipath程序我想是非常困难的，即使你对各种拖拽图都非常熟练使用，但涉及到一些业务判断需要的程序语句，如：需要使用String.IsNullOrEmpty()判断某项是否为空；或一段复杂的自定义过程，如：需要嵌入一段自定义的C#编写的.nupkg包；或定义一变量，如：定义一个List类型存放excel表各行数据的变量，你若不熟悉C#、VB之类的.net编程，你的无编程基础也能干好的信心将受到打击。虽然在使用Uipath过程中我也有吐槽的点，但想想还是等自己认识深刻一些才有发言权吧。
关于安装及使用，我们可以在其官网(http://www.uipath.com)申请下载UiPath（点击Start Trial），下载后是UiPathStudio.msi安装文件，按提示安装即可，由于它是基于.net的技术，安装不成功可能之一会提示需要netframworkX.X框架之类的，可以下载一个netframworkX.X安装一下。UiPath这款软件安装以后，当第一次打开UiPath Studio时，会出现注册窗口，用户可以选择开始开始试用-Start Trial（评估用）、激活许可- Activate License或购买许可-Purchase License。如果用户想试用Studio，点击开始试用-Start Trial ，只能试用12个星期。如果用户已经拥有许可，点击激活许可-Activate License。
就先谈到这儿吧。
4.3. 本文结束语，作为一个企业/部门/项目管理者，咨询人员，想引进RPA机器人，通过上面的了解，或许能给您带来一些有益参考。作为一个技术者，只了解上面的知识是远远不行的，对于具体的入门开发者的帮助，我想如果有时间出一个学习笔记的系列方能有点作用，篇幅限制技术细节在此是说不清楚的，对于有兴趣者如果想学习该技术，官网以及官网的论坛是一个比较好的途径(因为中文资料少)：
https://studio.uipath.com
https://forum.uipath.com
另外，还有一个开放的RPA培训网络平台(http://academy.uipath.com)，任何人都可以加入并获得专业的免费Uipath RPA认证。

参考资料：《RPA会计机器人来临，未来会计会如何发展》 jobs
2017年11月

版权所有，转载或引用请联系作者并注明出处。

