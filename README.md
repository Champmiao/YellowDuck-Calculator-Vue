🍉 本项目打包文件存在BUG，若查看请下载依赖运行，抱歉 🍉



<img src="https://github.com/Champmiao/YellowDuck-Calculator-Vue/blob/master/%E6%95%88%E6%9E%9C%E5%9B%BE.png" style="zoom: 50%;" />



###  面试作业：

> #### 使用Vue框架编写计算器:
>
> - 已实现除0、异常输入、多种非法输入等的错误提示。
> - 浮点型数字计算存在的问题，目前尚未实现解决方案，原因是当前计算器并非简易计算器（只针对四则运算或专门进行浮点型数字计算），内部计算实现依赖于JS函数 eval(),该函数可对内部字符串进行计算，鉴于目前可输入的表达式种类繁多（包含特殊符号），从中专门识别出浮点数计算有难度，易导致错误，短期内还不能实现功能。
> - 经查阅，浮点型数字计算的解决方案大致有两种: 第一种是将浮点数先转换成整数运算，之后再转回浮点数; 第二种是专门引入浮点数计算js库如 decimal.js、bignumber.js等，精度很好但多为专用计算器。



### 简介：

> #### 由Vue.js实现的中文提示计算器
>
> - 由Vue.js实现的中文提示计算器
> - 特点：中文提示(显示屏左侧)、常见错误针对性提示、界面美观。

> #### 人性化功能：
>
> - 若用户第一次按错运算符，无需像普通计算器按退格或清空键，直接按新运算符即可自动更改。
> - 若用户输入表达式取反次数较多，为避免显示过多"()",先计算结果后取反。

> #### 针对非法输入有一般性错误提示和针对性错误提示两种：
>
> - 针对性错误提示(开发时间过短，仅提供两种以示思维)："无法对负数计算平方根"、"除数不为0"
> - 一般性错误提示:"您输入有误请重新审查"



### 运行方法：

>-  install dependencies
>
>**npm install**
>
>- serve with hot reload at localhost:8080
>
>**npm run dev**



#### **🐕  感谢您的阅读 🐕**
