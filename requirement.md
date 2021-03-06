Requirement
===========

* 基础架构
	* 集群拓扑
		* 支持5000-10000台规模级别的服务器集群
		* 支持多机房容灾
	* 私有云平台(IaaS)
		* 采用虚拟化技术，可提供多套不同硬件配置的实例
		* 支持虚机资源的自助管理，可快速申请、启用、停用、释放虚机资源
		* 建立内部域名服务，支持内部域名资源的自助助管理，可快速绑定、释放应用实例IP
		* 支持LB与路由设备的自助管理，可快速、灵活调整LB策略与路由规则
	* 基础组件与服务
		* 将基础组件通过API形式开放给业务人员，将基础组件实现与应用完全隔离，基础组件参照操作系统升级的方式可进行统一升级与快速更新
			* 架构与业务升级时间上分开、升级过程需双方共同关注、一旦有问题可立即回滚
			* （注：目前架构团队已开始着手实现，具体见phoenix项目）
		* 标准化的数据服务，提供统一的key-value pair的数据服务平台，使应用开发人员避开大数据存储、扩展、性能等复杂问题，提高研发效率

* 运维平台
	* 部署与发布
		* 自动化的应用部署与发布工具
			* 整个部署与发布支持一键式全自动，过程间无需人工介入
			* 支持部署发布过程的全程可跟踪，可监控，可追溯
			* 摆脱目前上线流程，靠发mail或qq通知运维的低效困境
		* 灰度发布与A/B测试
			* 支持同一应用的多版本同时在线运行
			* 支持不同版本通过流量比例、城市、分类、地域、用户（星级、是否登陆）等多个维度进行分流，可灵活方便快速调整分流策略
			* 支持UI层面（如同一功能的不同UI展示）或复杂业务逻辑（如同一功能的不同页面处理逻辑）的A/B测试，可方便建立、配置、使用、跟踪、评估测试。
	* 安全
		* 防攻击平台：可防范分布式（上万散列IP）DOS攻击或页面攻击，为应用提供安全保障
		* 应用防火墙的封杀与解封，通过工具实现24小时监控管理
		* 支持盗库、恶意扫描等行为的入侵检测与防御
	* 监控
		* 统一的业务监控平台，可针对不同的业务监控需求进行灵活配置，提供及时的预警功能与初步故障定位、诊断功能
			* 业务开发人员不用穿梭于各种监控工具间，而可以一目了然地看到所有关心的监控数据，提高监控效率
			* 监控的内容需要包含机器运行健康状态，关键业务指标，以及吞吐量，响应时间等性能指标
			* 以log为基础，实现统一的监控、预警与显示；以dashboard形式，方便从广度和深度上，多维度地了解系统运行情况
			* 以统一的方式，对监控、预警进行订阅与推送
			* 通过同比、环比等各种统计数据，通过观察并设置合理阈值以实现预警
		* 能及时发现业务容量瓶颈，并支持预警功能
		* 支持应用日志的统一实时查看	   
	* 故障恢复
		* 支持对复现的已知故障进行系统自动故障隔离与恢复
		* 针对严重异常导致系统雪崩引发P0故障，通过流控、多机房切换等技术，使系统能快速从P0故障中恢复
	* 作业
		* 统一的作业部署与调度平台
			* （注：目前架构已有两套作业调度系统，一套针对单机作业，一套针对hadoop作业，今后两套系统会整合统一起来，就目前，新建作业完全可以使用现有作业调度系统进行部署与调度）	    

* 开发平台
	* 测试环境
		* 可模拟线上压力的性能测试仿真平台
			* 属于压力测试范畴，通过压测可有效获得系统安全边际
			* 在夜间机器空闲的时候做模拟压测，提高资源利用率
			* 集思广益的各种实现方式：
				* 方法1：选取某一应用集群中的一台机器，对其进行流量逐级加压，并实时跟踪其吞吐量及性能指标，找出安全边际（评：成本低，仿真度高，风险高）
				* 方法3：在测试环境中，建立性能测试脚本，通过对不同版本应用的性能测试数据做纵向对比，在已知旧版本应用安全边际的情况下，推算出新版本的安全边际（评：成本中，仿真度低，风险低）
				* 方法3：建立benchmark的请求序列（通过截取线上请求访问获得），并在测试环境中通过性能测试工具模拟序列请求来达到压测目标（评：成本中，仿真度中，风险低）
				* 方法4：流量镜像方式，实时复制线上请求到测试环境中进行压力测试（评：成本高，仿真度高，风险低）
				* 方法5：通过灰度发布方式，直接将新程序放在线上运行，逐步加压，遇到问题立即切走流量（评：成本低，仿真度高，风险高）
		* 内测环境，即公司员工或部分用户可通过官方域名直接访问的测试环境
		* 同一测试环境中支持应用多套版本的配置，便于同一应用的多分支测试
	* 可测试性
		* 基础组件提供方便应用开发进行单元测试的测试框架
	* 代码质量
		* code review工具，易于网上交互，review过程可记录、可追溯
	* 代码安全
		* code安全静态检查工具，方便自动搜索代码中的安全隐患，提高安全审计效率
		* 安全性代码基础框架，应用人员使用此框架开发，可方便规避诸多安全风险
		* 自动化安全测试脚本，通过黑盒方式对应用做安全入侵检测
	* 开源平台
		* 提供一个开源的平台，将架构及各类通用组件或工具在团队内部进行开源，共同维护优化，共享技术积累
			* （注：目前架构团队已搭建一套内部开源平台，http://code.dp/，并逐步将一些架构产品通过此平台分享给整个技术团队）
	* 统一入口
		* 提供一个统一的工具门户导航，在导航网站上可以方便地找到所有我们目前在使用的各种工具与平台。
	* 性能调优
		* 提供一套可以方便进行性能参数设置，并查看性能调优效果的管理工具

* 研发流程
	* 项目管理
		* 结合业务团队研发管理的思路，制定统一、灵活的项目管理规范，并定制与之配套的管理工具
	* 项目质量
		* 核心代码（如基础组件或service），必须经过其他人的review，否则无法合并
		* 核心代码采用分支开发，分支提交后自动进行CI，CI如果失败或新增代码没有覆盖CI，分支无法被合并
		* 制定安全编码规范，并根据规范制定审核工具，可自动扫描代码，全面保证代码的安全性要求
		* 项目仓库会集成相关质量检测工具对项目质量进行自动检测，会及时反馈给相关项目团队，促使项目质量的优化
		* 提供一套基于历史调优经验的、具有可操作性的DP Web性能调优指南，方便业务人员对应用进行参数调优
	* 研发效率
		* 测试效率
			* 明确测试环境的owner机制与权限分配，如beta或CI统一由QA负责，本地或alpha统一由业务开发负责，减少交叉环境操作引发环境可测性的降低
			* 必须保证利用线上稳定版本部署的测试环境的稳定性，避免因不稳定导致测试效率的降低
			* 明确测试数据的管理流程，包括测试数据的生成，初始化，重用和数据同步的机制等
		* 发布效率
			* 制定清晰、完整、可执行的发布流程，通过自助式发布工具，实现应用的规范、快速、自主发布
			* 支持业务与基础组件间相互独立的发布过程

* 数据平台
	* 数据源
		* hippo与ga的差异化定位，hippo作为ga的有效补充，在实时计算与数据挖掘领域可发挥其长处
	* 数据查询
		* adhoc查询
			* 提供基于hive的web查询系统，使业务人员可更方便地通过类SQL语言在hadoop上进行数据分析与查询
				* （注：目前架构团队已经在构建中，12月开始邀请一小部分用户试用，经过反馈改进，再逐步推广）



