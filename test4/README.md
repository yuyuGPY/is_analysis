# 实验三 图书管理系统顺序图绘制

## 图书管理系统顺序图
### 1.借出图书
#### 源码
```flow js
@startuml
actor ": 图书管理员" as 图书管理员
participant ": 读者" as 读者
participant ": 馆藏目录" as 馆藏目录
participant ": 馆藏书籍资源" as 馆藏书籍资源
participant ": 借书记录" as 借书记录
skinparam sequenceParticipant underline
图书管理员 -> 读者:验证读者
activate 图书管理员
loop
activate 读者
deactivate 读者
图书管理员 -> 读者:取读者限额
activate 读者
opt 超出限额
读者-->图书管理员:取消借书操作
deactivate 读者
end
图书管理员 -> 馆藏目录:获取馆藏目录
activate 馆藏目录
馆藏目录 -> 馆藏书籍资源:查找书籍资源
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 馆藏目录
图书管理员 -> 借书记录:创建借书记录
activate 借书记录
deactivate 借书记录
图书管理员 -> 馆藏目录:借出资源
activate 馆藏目录
馆藏目录 -> 馆藏书籍资源:减少可借数量
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 馆藏目录
图书管理员 -> 读者:减少可借限额
activate 读者
deactivate 读者
end
图书管理员 -> 借书记录:打印借书清单
activate 借书记录
deactivate 借书记录
deactivate 图书管理员
@enduml
```
#### 顺序图
![借出图书顺序图](./借出图书.png)
#### 说明
读者借书时，读者将借阅书籍与读者卡交与管理员，管理员判断对书籍与读者分别进行验证，验证读者是否存在借书条件，
并判断读者借书限额，

之后查询书籍信息与品种，两种验证均通过的情况下，借书成功，减少库内书籍数量，减少读者可借书数，一次借书操作完成，

在借书操作循环完成后，打印读者借书清单。
### 2.归还图书与赔偿登记
#### 源码
```flow js
@startuml
actor ": 图书管理员" as 图书管理员
participant ": 读者" as 读者
participant ": 馆藏目录" as 馆藏目录
participant ": 馆藏书籍资源" as 馆藏书籍资源
participant ": 借书记录" as 借书记录
participant ": 逾期记录" as 逾期记录
participant ": 罚款细则" as 罚款细则
skinparam sequenceParticipant underline
图书管理员 -> 读者:验证读者信息
activate 读者
deactivate 读者
loop
图书管理员 -> 馆藏目录:读取馆藏目录
activate 图书管理员
activate 馆藏目录
馆藏目录 -> 馆藏书籍资源:获取书籍资源
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 馆藏目录
图书管理员 -> 借书记录:取借书记录
activate 借书记录
借书记录 --> 图书管理员:借书记录
deactivate 借书记录
图书管理员 -> 馆藏目录:归还资源
activate 馆藏目录
馆藏目录 -> 馆藏书籍资源:增加可借数量
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 馆藏目录
图书管理员 -> 借书记录:登记还书日期
activate 借书记录
deactivate 借书记录
opt 逾期
图书管理员 -> 逾期记录:登记逾期记录
activate 逾期记录
逾期记录->罚款细则:匹配罚款细则
activate 罚款细则
deactivate 罚款细则
逾期记录 --> 图书管理员:逾期信息
deactivate 逾期记录
end
图书管理员->读者:增加可借限额
activate 读者
deactivate 读者
end
图书管理员 -> 借书记录:打印还书清单
activate 借书记录
deactivate 借书记录
图书管理员 -> 逾期记录:打印逾期清单
activate 逾期记录
deactivate 逾期记录
deactivate 图书管理员
@enduml
```
#### 顺序图
![归还图书与赔偿登记顺序图](./归还图书与赔偿登记.png)
#### 说明
读者归还图书时，将需要归还的书籍和读者卡交给图书管理员，图书管理员首先对读者进行验证，验证是否读者是否可借还书，

通过验证后，依次读取馆藏目录与馆藏书籍资源，验证书籍与品种信息是否存在馆藏信息中，存在则继续进行归还处理，

图书管理员读取读者借书记录，查找读者是否正在借阅该图书，

如过借阅信息存在，则继续进行归还操作，增加书籍资源库存数目，登记借书记录还书日期，同时判断是否逾期，超过预期归还
时间，若超过，进行逾期记录登记，

逾期记录登记完成后增加读者可借书限额，一次归还操作完成，根据读者是否需要归还多本书进行循环，循环完成后打印还书清单与逾期清单
### 3.续借图书
#### 源码
```flow js
@startuml
actor ": 图书管理员" as 图书管理员
participant ": 读者" as 读者
participant ": 馆藏目录" as 馆藏目录
participant ": 馆藏书籍资源" as 馆藏书籍资源
participant ": 借书记录" as 借书记录
skinparam sequenceParticipant underline
activate 图书管理员
图书管理员->读者:验证读者信息
activate 读者
deactivate 读者
loop
opt
图书管理员->借书记录:取读者该图书可续借次数
activate 借书记录
借书记录-->图书管理员:是否可续借
deactivate 借书记录
end
图书管理员->借书记录:输入图书信息查询借书记录
activate 借书记录
借书记录->馆藏书籍资源:取书籍信息
activate 馆藏书籍资源
馆藏书籍资源-->借书记录:读者当前正借阅书籍信息
deactivate 馆藏书籍资源
借书记录-->图书管理员:借书记录
deactivate 借书记录
图书管理员->借书记录:更新预计归还时间
activate 借书记录
deactivate 借书记录
end
图书管理员->借书记录:打印续借清单
activate 借书记录
deactivate 借书记录
deactivate 图书管理员
@enduml
```
#### 顺序图
![续借图书顺序图](./续借图书.png)
#### 说明
续借图书时读者提供书籍信息与读者卡给管理员，管理员验证读者信息，验证通过后继续续借操作

图书管理员根据读者和书籍信息查询借书记录中该书的已续借次数，判断是否可续借

可续借的情况下依次查询图书借书记录与书籍信息

更新还书预计时间，一次续借图书操作完成

续借操作完全完成后，打印续借清单
### 4.维护书目
#### 源码
```flow js
@startuml
actor ": 图书管理员" as 图书管理员
participant ": 馆藏书籍资源" as 馆藏书籍资源
participant ": 图书品种" as 图书品种
skinparam sequenceParticipant underline
activate 图书管理员
图书管理员->图书管理员:验证权限
loop
图书管理员->馆藏书籍资源:查询书籍信息
activate 馆藏书籍资源
馆藏书籍资源->图书品种:图书品种信息查询
activate 图书品种
图书品种-->馆藏书籍资源:图书品种信息
deactivate 图书品种
馆藏书籍资源-->图书管理员:书籍信息
deactivate 馆藏书籍资源
图书管理员->馆藏书籍资源:维护书籍信息
activate 馆藏书籍资源
图书管理员->馆藏书籍资源:添加或修改书籍信息
馆藏书籍资源->图书品种:图书品种信息修改或添加
activate 图书品种
deactivate 图书品种
deactivate 馆藏书籍资源
end
图书管理员->馆藏书籍资源:打印维护书籍清单
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 图书管理员
@enduml
```
#### 顺序图
![维护书目顺序图](./维护书目.png)
#### 说明
维护书目时图书管理员需要先验证图书管理员权限，判断图书管理员是否可维护书目，

通过权限验证后查询馆藏书籍资源信息，并查看品种信息，

若需要添加或修改，则分为馆藏书籍资源信息和书籍品种信息两种，若不需要，一次维护操作结束

维护操作完全完成后，打印维护书目清单
### 5.维护读者信息
#### 源码
```flow js
@startuml
actor ": 图书管理员" as 图书管理员
participant ": 读者" as 读者
skinparam sequenceParticipant underline
activate 图书管理员
图书管理员->图书管理员:验证权限
loop
图书管理员->读者:查询读者信息
activate 读者
读者-->图书管理员:读者信息
deactivate 读者
图书管理员->读者:维护读者信息
activate 读者
deactivate 读者
end
图书管理员->读者:打印维护读者清单
activate 读者
deactivate 读者
deactivate 图书管理员
@enduml
```
#### 顺序图
![维护读者信息顺序图](./维护读者信息.png)
#### 说明
维护读者信息时图书管理员需要先验证图书管理员权限，判断图书管理员是否可维护读者信息，

通过权限验证后查询读者信息，

若需要维护，则进行维护操作，若不需要，一次维护操作结束

维护操作完全完成后，打印维护读者信息清单
### 6.查询书目
#### 源码
```flow js
@startuml
actor 读者
actor ": 读者" as 读者
participant ": 馆藏目录" as 馆藏目录
participant ": 馆藏书籍资源" as 馆藏书籍资源
skinparam sequenceParticipant underline
activate 读者
读者->馆藏目录:取馆藏目录
activate 馆藏目录
馆藏目录->馆藏图书资源:查询图书资源信息
activate 馆藏图书资源
馆藏图书资源-->读者:返回图书资源信息
deactivate 馆藏图书资源
deactivate 馆藏目录
deactivate 读者
@enduml
```
#### 顺序图
![查询书目顺序图](./查询书目.png)
#### 说明
读者进行查询书目操作时，先取馆藏目录，根据馆藏目录选择馆藏书籍资源，并查询相关信息，不管是否查询到书籍信息都需要返回给读者
### 7.查询借阅情况
#### 源码
```flow js
@startuml
actor ": 读者" as 读者
participant ": 借书记录" as 借书记录
participant ": 逾期记录" as 逾期记录
skinparam sequenceParticipant underline
读者->读者:验证读者信息
读者->借书记录:取借书记录
activate 借书记录
借书记录->逾期记录:取逾期记录
activate 逾期记录
逾期记录-->借书记录:返回逾期记录
deactivate 逾期记录
借书记录-->读者:返回借书、归还、逾期记录
deactivate 借书记录
@enduml
```
#### 顺序图
![查询借阅情况顺序图](./查询借阅情况.png)
#### 说明
读者进行查询借阅情况操作时，先验证读者信息，

验证通过后查询读者借书记录，并通过借书记录查询其相关逾期记录，并向读者返回借书、归还、逾期记录
### 8.预定图书
#### 源码
```flow js
@startuml
actor ": 读者" as 读者
participant ": 预定记录" as 预定记录
participant ": 馆藏目录" as 馆藏目录
participant ": 馆藏书籍资源" as 馆藏书籍资源
skinparam sequenceParticipant underline
activate 读者
读者->读者:验证读者信息
loop
读者->馆藏目录:取馆藏目录
activate 馆藏目录
馆藏目录->馆藏书籍资源:查询书目
activate 馆藏书籍资源
deactivate 馆藏书籍资源
deactivate 馆藏目录
读者->预定记录:查询可预定数目
activate 预定记录
预定记录-->读者:可预定数目大于0
opt
预定记录-->读者:可预定数目为0
end
读者->预定记录:查询已预定书籍信息
预定记录-->读者:未预定当前需要预定图书
opt
预定记录-->读者:已预定当前需要预定图书
end
读者->预定记录:登记预定记录
deactivate 预定记录
end
读者->预定记录:打印预定清单
activate 预定记录
deactivate 预定记录
@enduml
```
#### 顺序图
![预定图书顺序图](./预定图书.png)
#### 说明
读者预定图书操作执行前先验证读者信息，验证通过后继续进行预定图书操作，

读者查询当前需要预定的图书信息，通过馆藏目录与馆藏书籍资源查询，查询后可进行预定操作

读者预定前查询读者可预定数目，判断是否可预定图书，若可预定图书，继续进行预定操作

读者查询预定记录查看已预定书籍信息，判断当前预定书籍是否已预定，未预定则继续进行预定操作

读者填写登记预定记录，一次预定操作完成

预定操作全部完成后，打印预定清单
### 9.取消预定
#### 源码
```flow js
@startuml
actor ": 读者" as 读者
participant ": 预定记录" as 预定记录
skinparam sequenceParticipant underline
activate 读者
读者->读者:验证读者信息
loop
读者->预定记录:查看预定记录
activate 预定记录
预定记录-->读者:未查询到预定记录
opt
预定记录-->读者:未查询到预定记录
end
读者->预定记录:删除预定记录
deactivate 预定记录
end
读者->预定记录:打印取消预定清单
activate 预定记录
deactivate 预定记录
deactivate 读者
@enduml
```
#### 顺序图
![取消预定顺序图](./取消预定.png)
#### 说明
读者预定图书操作执行前先验证读者信息，验证通过后继续进行取消预定图书操作，

读者查询当前的预定记录，查询到后进行下一步操作

读者删除预定记录，一次取消预定操作完成

取消预定操作全部完成后，打印取消预定清单