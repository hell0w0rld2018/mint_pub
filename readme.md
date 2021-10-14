# 说明文档
## 声明
- 该程序使用时需要私钥，本人承诺该程序不会上传私钥信息，欢迎大家抓包分析。同时也承诺只收取约定好的报酬，不会暗中转账。
- 如果无法接受请直接删除该程序，不要浪费时间和口舌。
- 使用该程序时请保护好私钥文件，不要泄露，任何时候都不要复制到聊天工具上。
- 该程序只能加快mint速度，无任何附加功能。
- 使用该程序为自愿原则，造成的任何损失概不负责。
- 文档中有叹号的位置请务必注意！！！

## 使用范围
- solana上所有基于candy machine作为mint工具的项目都可使用。
- 目前绝大部分项目都是基于candy machine，也有一些例外，例如Solarmy 2D&3D使用了raydium的drop zone（抽奖性质），RogueSharkTank使用了fair launch（也是抽奖性质），甚至还有些是项目方手动mint再分发的。

## 使用费用
- 每成功mint一个nft，按照nft的价格和手续费比例收取报酬。mint失败不支付费用。例如nft的mint价格为1 sol，手续费为10%，mint成功后会自动转账给作者0.1 sol。
- 请保证钱包余额大于mint的金额+solana手续费+作者报酬。
- 第一版手续费为10%，后续可能会调整。同时后续会推出高性能版本（目前版本控制了速度），支付更高比例的手续费可以使用。手续费在程序运行时可以看到。

## 快速开始
- 私钥复制到key.txt
- config.ini里配置好candy_machine地址
- 启动mint_pub.exe，输入数量，输入y开始mint
- 结束

# 以下内容为详细文档，可以忽略，但建议阅读

## candy machine
- candy machine是metaplex的一个项目，简单说来就是项目方把图片、属性等信息放到一个链上地址，用户在网页上点mint键时，会和这个链上地址交互，完成mint。
- 上述仅为简单描述，实际远远比这复杂，更多信息和candy machine源码请参考metaplex的github：https://github.com/metaplex-foundation/metaplex。
- candy machine本身没有对单用户的mint数量做限制，没有密码，也没有白名单机制，这些限制全都在网页上。
- 该程序直接和candy machine链上地址交互，完成mint，绕过网页卡顿和所有限制。

## 准备工作
### 创建私钥文件key.txt
- 首次使用需要从钱包里导出私钥。
- Phantom在右下角齿轮，Export Private Key。Solflare在左上角账户头像，Export Private Key。其他钱包请自行寻找。
- 私钥的格式有两种，一种是数字字母（ogR1wg4AVTNeVkDTwQfcfgrn4P98CZxxxxxx），另一种是数组（[208,118,164,180,64,93,xxx]），该程序均支持。不要复制到聊天工具上问私钥格式对不对！！！
- 私钥复制到key.txt里，保存关闭。

## 使用方法
### 修改配置文件config.ini
- endpoint处为网络节点，https://api.mainnet-beta.solana.com是solona公共的主链节点，一般不做修改。此处可以改成测试链节点https://api.devnet.solana.com，有条件可以自建节点。
- candy_machine处为链上地址。
### candy machine链上地址获取（难点）
- 大部分情况下可以通过mint网页获取，js信息里有。获取时注意下是不是主链节点，很多项目方到最后都仍然放的是测试链信息，其实就是没准备好，导致项目推迟。
- 有时mint页面会隐藏，但mint开始前一般都开放了。
- 作者有更多方法可以获取，可以一起交流。
- 注意：请一定要确认链上地址为正确地址，否则可能导致损失！！！

## mint
- 启动程序mint_pub.exe。正常情况下会一直输打印信息到“mint数量：”，如果报错请根据错误提示修改。正常情况如下

> 程序版本：1.0.0，有效时间：2021-11-01 00:00:00
> 
> 链上节点：https://api.mainnet-beta.solana.com
> 
> 链上时间：2021-10-09 13:11:44
> 
> 钱包地址：xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
> 
> 钱包余额：5.275128 sol
> 
> 链上地址（candy machine）（请务必核实信息是否正确）：XXXXXXXXXXXXXXXXXXXXXXX
> 
> 
> 
> 项目内容读取成功，内容如下（请务必核实信息是否正确）
> 
> 项目名称（第一个）：NAME #1
> 
> 项目链接（第一个）：https://arweave.net/XXXXXXXXXXXXXXXXXXXXXXXXXXXX
> 
> 项目缩写：SYMBOL
> 
> 总共数量：2000
> 
> 剩余数量：2000
> 
> 每枚价格：1 sol
> 
> 开始时间：2021-11-01 00:00:00
> 
> 报酬比例：10%
> 
> 每次mint成功作者报酬：0.1 sol
> 
> mint数量:

- 输入数量，按回车
- 提示“输入y按回车后开始：”，输入“y”，按回车，程序开始mint。输入其他字母则中断。
- 顺利的话，后面会提示“第n个nft地址：xxxxxxxxxxxxxxxxxx” “第n个nft成功mint”，直到输入的数量。之后程序自动退出。
- mint过程增加了自动重试，因此可以早几秒启动，等mint时间到了会正常mint。但如果是其他错误则可能需要关闭程序重新调整，例如已经mint完，余额不足等。常见错误可参考下节。
- 注：该程序启用了预处理功能，大部分报错并没有真正发送指令，不用担心手续费被消耗。

## mint过程中常见错误
### 错误内容中有 custom program error: 0x137'
- 已经mint完
### 错误内容中有 custom program error: 0x138'
- 还未到mint时间
### 错误内容中有 Insufficient
- 钱包余额不足
### 错误内容中有 Attempt to debit an account but found no record of a prior credit
- 钱包余额为0