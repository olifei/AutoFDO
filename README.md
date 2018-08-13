# Auto Feedback Directed Optimizer
### 什么是AutoFDO？
AutoFDO使用基于profile的采样来驱动反馈的优化。  
AutoFDO使用**perf**工具来收集采样。有一个独立的工具可以用来将perf.data转换成gcov格式。一个独立的环节是将处理过的profile数据来注解循环区块的计数，以此来预测分支概率。  
### 阶段
在AutoFDO中有三个阶段：
#### 阶段一：从profile数据中读取profile
从gcov文件中可以读取到以下信息：
* 函数名称和文件名称
* 源文件级别的profile
* 模块级别的profile，从模块到模块的对应
阶段一仅仅是读取文件，并没有处理文件。  
#### 阶段二：处理profile来构造内部数据结构（hashmap）
这一步需要在第一步（tree parsing）之后进行，因为需要用到从函数名到调试名的map关系。以下hashmap用来存储profile：
* function_htab：从function_name到entry_bb count的映射
* stack_htab：从inline stack到sample count的映射
* bfd_name_htab：从function name到debug name （bfd_name）
* module_htab：从module name到aux-module names的映射
#### 阶段三：注解控制流图
AutoFDO在整个控制流图中使用了独立的pass来：
* 注解基本的循环快计数
* 估计预测分支

reference: [source](https://gcc.gnu.org/wiki/AutoFDO)
