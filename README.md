# 1.准备工作
## 从ROM中导出Narc
使用[tinke](https://github.com/pleonex/tinke)读取*.nds导出a/0/0/2、a/0/0/3和a/0/2/3这三个Narc文件
其中：

a/0/0/2 系统文本集

a/0/0/3 剧情文本集

a/0/2/3 字体集（字库）

## Narc解包
### 示例
*此处仅为使用方法的示例*

為了方便後續操作，建议将上述通过tinke提取出来的文件重命名，以日版(JP)的Narc文件为例：

a/0/0/2提取出来的2 重命名为 B(JP)2

a/0/0/3提取出来的3 重命名为 B(JP)3

a/0/2/3提取出来的3 重命名为 a023

将这些Narc文件与*.py放在同级目录下，在控制台Command Line(CMD)中按

ExtractNarc.py <narc文件名> <处理类型：输入text（文本）或file（其它）>执行语句：

```
>> python ExtractNarc.py B(JP)2 text

>> python ExtractNarc.py B(JP)3 text

>> python ExtractNarc.py a023 file
```
由此就创建了：

B(JP)2_extr、B(JP)3_extr和a023_extr这三个分别对应B(JP)2、B(JP)3和a023的子文件夹。

B(JP)2_extr、B(JP)3_extr内应当有：解包后按照<>编号命名的文件块（无后缀）和对应的解密后的文本*.txt

a023_extr内应当有：解包后按照编号命名的文件块（无后缀）

# 2.文本处理
*整体流程：*

拆文本→翻译（修改）文本→做码表→改字库→文本和字库都导入narc打包→tinke导入对应New

## 拆文本组合为原译文对照
→直接执行CMP_Div_main.py脚本，按照提示制作对照文本。

### 示例
依然以 1.准备工作 中的示例文件命名为例：
```
>> python CMP_Div_main.py

>> 请输入当前目录下已利用ExtractNarc.py导出的文本对应的源Narc文件名：B(JP)3

>> 请选择操作：CMP
```
由此就会在B(JP)3_extr\目录下创建名为B3_CMP的文件夹，其中包含0和1两个分块的原译文对照文本。
要修改的是0和1两个文件夹中的文本。

### 翻译示例
以.\B(JP)3_extr\B3_CMP\0\0_CMP_B3-23.txt中的第一段文本0_0I为例：

0_0I

\------------------------------

はくぶつかんの　そのおくで

ちょうせんしゃを　まつ`<F>`

ポケモンジム……`<F>`



なんだか　ふんいき　あるっすよね`<PAGE>`



というわけで

これを　さしあげるっす！`<PAGE>`



\------------------------------

はくぶつかんの　そのおくで

ちょうせんしゃを　まつ`<F>`

ポケモンジム……`<F>`



なんだか　ふんいき　あるっすよね`<PAGE>`



というわけで

これを　さしあげるっす！`<PAGE>`



\------------------------------

上面的部分是原文，下面的部分是译文，因此直接修改翻译下面的部分即可：

0_0I

\------------------------------

はくぶつかんの　そのおくで

ちょうせんしゃを　まつ`<F>`

ポケモンジム……`<F>`



なんだか　ふんいき　あるっすよね`<PAGE>`



というわけで

これを　さしあげるっす！`<PAGE>`



\------------------------------

博物馆里面是

等待着挑战者的宝可梦道馆……`<F>`



感觉真有气氛啊。`<PAGE>`



哦对了，

这个给你！`<PAGE>`



\------------------------------
#### 控制符
`<>`中包括的是控制符，翻译时建议尽量按照原文的格式摆放。

不过对于`<PAGE>`和`<F>`，翻译者可以酌情考虑变更。

在游戏中的效果：

`<PAGE>`的作用是：显示翻页的运动箭头▼，并且按A后整个对话框翻一页。也就是按A后，`<PAGE>`后的文段直接覆盖`<PAGE>`所在的文段。

以上面的翻译为例：“感觉真有气氛啊。`<PAGE>`”按A，对话框的两行字直接换成“哦对了，

这个给你！`<PAGE>`”。

`<F>`的作用是：按A后，`<F>`后面的一段文字把`<F>`所在的这段文字顶到上面去。

以上面的翻译为例：“等待着挑战者的宝可梦道馆……`<F>`”按A，

“
感觉真有气氛啊。<PAGE>”往上滑动，顶去“等待着挑战者的宝可梦道馆……<F>”。

## 文本导入打包流程

### 组合文本
→直接执行CMP_Div_main.py脚本按照提示组合文本。

#### 示例
依然以 1.准备工作 中的示例文件命名为例：
```
>> python CMP_Div_main.py

>> 请输入当前目录下已利用ExtractNarc.py导出的文本对应的源Narc文件名：B(JP)3

>> 请选择操作：Restore

>> 请输入拟定创造的新Narc文件名：B(CH)3
```
组合后的文本就在新创建的B(CH)2_extr/目录下，并且已经创建好了待写入的对应文本块。

### 导入文本到对应Narc分片中
→用InjectTXTNarc.py批量导入分片。
#### 示例
依然以 1.准备工作 中的示例文件命名为例：

```
>> python .\InjectTXTNarc.py B(CH)3
```

待程式正确执行后，这样就将B(CH)3_extr\中的所有*txt导入对应的Narc分片中了。

### 打包Narc
→用MakeNarc.py打包文本Narc生成对应的New_Narc
#### 示例：
依然以 1.准备工作 中的示例文件命名为例：
```
>> python .\MakeNarc.py B(CH)3
```
这样就得到了打包后的名为New_B(CH)3的Narc.

# 3.字库制作
## 做码表的流程
→直接执行CodeList_main.py按照提示制作新码表。

### 示例
依然以 1.准备工作 中的示例文件命名为例：

```
>> python .\CodeList_main.py

>> 请输入Nftr对应的原Narc文件名：a023

>> 是否已有完整的原始码表？输入Y为是，N为否：N

>> 已在程式脚本同目录下生成3个字库对应的全码表
a023-0_Total.txt、
a023-1_Total.txt、
a023-2_Total.txt，
这3个码表的内容应当完全相同。

>> 确认无误后，请任选一个码表作为原始码表（输入0，1，2中的任意一个数）：0

>> 请输入翻译后的文本*.txt所在的文件夹路径：B(CH)2_extr

已在程式脚本同目录下生成了名为CHS.TBL的中文编码表，专用于制作中文字库制作。

>> 确认无误后，请按任意键继续后续操作：\n
已在程式脚本同目录下生成了名为NewCodeList.txt的新码表。
全操作完毕，请按任意键结束...
```
中途会对应生成全文涉及到的中文码表CHS.TBL，用于后续的中文字库制作。

制作的全新码表固定名称为NewCodeList.txt，仅执行中文编码区域的覆盖。

## 修改重做字库调用流程
### 制作中文字库
用MakeFonts_main.py参照CHS.TBL做中文字库。
#### 示例
依然以 1.准备工作 中的示例文件命名为例：
```
>> python .\MakeFonts_main.py CHS.TBL SIMSUN2.TTC a023
```
如上执行后，会在与脚本同级的目录下得到三个中文字库：

a023-0-chs

a023-1-chs

a023-2-chs

为了方便后续操作，请不要擅自移动它们。

### 修改字符宽度表
→用Change三件套修改Width、Bitmap和CodeList，直接执行ChangeBCW_main.py按提示操作。
#### 示例
依然以 1.准备工作 中的示例文件命名为例：
```
>> python .\ChangeBCW_main.py

>> 请输入已经导出的Nftr的原Narc文件名：a023

>> 请输入码表路径：NewCodeList.txt
```
正确执行上述后，在a023_extr\下会生成_new的所有需要的新字库文件。

### 导入Nftr导入Narc
→InjectNftr.py将新字库文件导入各分片。

→MakeNarc.py打包字库。
#### 示例
依然以 1.准备工作 中的示例文件命名为例：
```
>> python .\InjectNftr.py a023

>> python .\MakeNarc.py a023
```
这样就在脚本的同级目录下得到了打包后的新字库New_a023的Narc.

# 将新Narc导入ROM
使用[tinke](https://github.com/pleonex/tinke)读取*.nds将a/0/0/2、a/0/0/3和a/0/2/3这三个Narc文件对应替换后保存。

以 1.准备工作 中的示例文件命名为例：

将a/0/0/2替换为New_B(CH)2，将a/0/0/3替换为New_B(CH)3，将a/0/2/3替换为New_a023.

黑/白版本的文本和字库是相同的，所以直接用相同的新Narc对应黑/白的ROM替换即可。

至此你完成了《宝可梦：黑/白》的文本汉化工作。

至于游戏中的图片汉化，还需要另做操作，此处不作赘述。
