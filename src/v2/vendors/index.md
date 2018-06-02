---
title: 综述
type: vendors
order: 0
---

## 供应商一览


![](https://user-images.githubusercontent.com/2363295/40734379-daeb69bc-646a-11e8-8649-21e370c71179.png)


https://www.altencalsoftlabs.com/robotic-process-automation/



## 供应商优劣势分析


* 1.BLUEPRISM

优点	
	Good workflow/flowchart designer -  enables better traceability of automation with process and ease of maintenance.
	Business objects are reusable by design
	Supports distributed development. True enterprise level centralized Architecture.
	In built reporting module with lowest level of granularity and strong analytical engine.
	Detailed transaction log available
	Supports Role based access control, Active Directory (AD) integration
	Tool can get Control room data through query
	Centralized secured credential storage and HIPPA, PCI compliant security standard.
	Ability to provide BP components as web service - provide greater flexibility in designing enterprise level solution. 
	Runtime execution is fast, but slower than AA
	Good all-round functionality with a proven track record of deployment at many enterprises
	OCR support (Tesseract)
	
缺点	
	"Record and Play" feature not available
	Windows 10 not supported
	Limited browser support beyond IE
	Version control management tool not available
	Product Licence is costly. Need to have minimum licences.
	Windows based control room as opposed to web based control room from compatatiors.
	Mainly used for Backoffice (unassisted automation). RDA / assisted automation is supported through surface automation (double check) using events like button click event.
	
核心组件Blue Prism:	
Process studio	
Object studio	
Control room 	
System manager and Release manager	
Application Modeller

     



* 2.Pega Robotics / Openspan:


优点	
	Great RPA tool for both RDA and RPA. RDA is assisted automation where Agent an Bot work together. It requires a design of user interface.
	Integrated with Visual studio and great if you have DotNet/C# experience.
	Framework components like Logging,  runtime configuration (read dynamic values from  xml)  can be built as C# DLL / custom conrols and import it to openspan studio.
	Text adapter/Emulator for mainframe support
	TFS integration
	
	
缺点	
	Non Visual studio .Net developers find it difficult.
	Localized credential staorage in flat file.
	Inbuilt work queues not available 
	
核心组件PegaRobotics/Openspan	
	Robotic Desktop Automation (RDA) support by adding windows forms
	Adapters (Web, Desktop, Text and Citrix)
	Global Container, Activities and Interaction manager to pass data back and forth between projects
	Entry and Exit points         



* 3.Automation Anywhere:

优点	
	"Record and Play" feature available making development easy and faster
	Supports MODI, TOCR and external OCR Engines (Accuracy is very good)
	Built in Software version control (SVN) available 
	Centralized web based dash board for runtime Bot monitoring
	Good analytical feature plug-in (Bot Insight) for business and operational data
	Metabot feature available to integrate DLLs, create re-usable components and perform offline development
	Supports Role based access control
	Ease of deployment. Offers flexibility and ease of doing business
	Runtime execution is fast
	Desktop automation as well as enterprise capabilities
	Web based control room that allows to access it from mobile
	
缺点	
	Inbuilt work queues not available 
	AA has localized credential staorage in flat file. Credential vault is not secured for users
	Need minimum programming experience
	AA only has task level reporting capability
	AA has procedural approach towards exposing AA components.
	AA fails to create a complete Virtual workforce concept by not allowing to share same workstation with Agent/user.
	Exposing web service not supported in AA
	Limited features for data driven backend operations, compare to its competators. For ex, excel automation or file automation. 
	
核心组件 AA 	
	TaskBot - Basic Automation License, used to automate rule based transactional processes
	MetaBot - Advanced features for large deployments, Offline app automation and consuming objects.
	IQBot - For Cognitive RPA. 
	BotFarm - For cloud deployment
	BotInsight - For business and operational analytics

        
* 4.UiPath	

优点	
	Has robust workflow designer and recording feature
	Efficient image recognition 
	Feature available to record Citrix processes
	Business objects are re-usable (Sequences, flowcharts)
	Native support to OCR and various Cloud options (Average accuracy)
	In-built software version control (TFS, SVN)
	Supports Role based access control
	Elastic search / indexer server available for operational and historical data of the robots
	Multi-tenancy – you can separate control room into several areas e.g. per department or DEV and QA
	Supports both front and back office automation
	
缺点	
	Overall runtime performance is slow
	Requires 8 GB RAM 
	Java code not supported whereas VBA, C# are supported
	Able to extract PDF data but extracting tabular data from PDF is lengthy process
	
	
核心组件 UiPath:	
	UiPath robots - Enterprise ready Front and Back office robot
	UiPath Studio - 
	UiPath Orchestrator - Enables the Orchestration and management of thousands of robots from a single command centre




## 供应商产品宣介材料


## 供应商客户一览



## 培训教程和材料



## 供应商评价


 某互联网公司高管的话：请咨询公司实施，咨询公司最后剩下的作用只是出个报告。 ​​​​ 


 ……还是要把很多日常事务的核查、处理流程等自动化，不为什么，就因为我太讨厌重复劳动了，内存有限也懒得去记约会日程orz……让机器工作吧，我去看书了 ​ 


昨天到一家制造企业参观、调研，综控室把企业的运行设备、物料库存、实时在线员工等信息，林林总总显示了几大屏，各种数据表、曲线、饼图…领导来看，“工业大数据”一目了然，很震撼。所谓“流程智慧化、管控智能化、设备自动化、集成高效化”。

但企业的人私下说：现在只做到部分数据的实时采集和汇总显示，有些数据仍存在滞后问题，各系统和数据的关联集成还没完全做到，因此无法进行数据分析和优化。满屏幕的数据要创造实际的价值，还需要完善管理机制，提高数据的实时性、准确性，也需要建立完整的、集成的、合理的数据模型，才能实现流程优化、管控和设备统管、预维等价值目标。

有多少工业企业的大数据项目，现阶段停步于“几大屏”？而再往下进行，难度在哪？



关于财务自动化，有一个就是之前在人工智能里提过的，中文翻译为机器人流程自动化，感兴趣的可以关注下这个新兴领域，看这个机器人流程自动化是怎么逐渐发展和改变工作的。机器人流程自动化确实现在还有很多局限和各种bug，但是还是很看好这个领域的发展，对后台部门都可以产生很大的变革，而最先替代的就是财务，因为有严格的操作流程和标准化规范，机械重复较多，但是其实可以扩展到各种工作和部门。其实机器人自动化在欧美已经给后台部门带来较高的自动化率，但是在亚洲还很低，很看好人工智能在这个领域的发展，这块确实很有需求。


![default](https://user-images.githubusercontent.com/2363295/40172349-c1671406-5a00-11e8-88b5-cfc7f0574eaa.png)

 数字化转型的核心本质就是：数字模型为纽带，数字化技术为支撑，实现产品和商业流程的自动化和智能化。由卖产品到卖服务，通过对产品进行数字化的全生命周期管理，发掘新的商业模式，围绕产品生命周期的“数字化服务”即云服务成为企业的基本商业模式。 ​ 

 企业的智能化提升是路径而不是目的，前三步：流程标准化-管控信息化-人机自动化，智能化涵盖其中，主要是指体系中的运算、模拟、判断的能力。 ​ 

 自动化 算法 就是模拟人固有的操作流程，通用的将被自动化，特殊的还是需要人工处理。只是通过网页平面自动化扩大到机器人实体，从二维到三维空间。 ​ 


 跑了一下午数据最后报表崩溃了。气死了，回家吃饭！有时候真的想从这种毫无意义的数据整理的工作脱离出来，可是每天都忙的没时间怎么去想提升工作效率，哪些工作流程可以标准化和自动化的[挖鼻][挖鼻][挖鼻]陷入一种疲于奔命的工作状态只会让自己的工作效率越来越低[晕][晕][晕] ​ 




