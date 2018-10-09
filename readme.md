# GamePlay数据类 #
　　简单说明一下,在GamePlay数据类中,有许多数据都是要由外部输入的,到时会有一个全局的 “字典/列表/队列” 等来管理所有数据。

　　对于一个由外部输入的数据,在定义属性时,使用注释 "<==" 来说明,对于在游戏中依据某些数据动态生成的数据,则不使用此注释. 

## GamePlay数据类中的Model类和Mono类 ##
在我设计的GamePlay游戏逻辑类中，有Model类和Mono类这两种，其中Model类是相当于Java里面的JavaBean那种感觉，主要就是用来存储从外部导入进来的游戏数据，比如技能、物品、人物数据等。而Mono类对应的是Unity里面的MonoBehaviour类，主要用来管理游戏进程的。

举个例子，对于投射物，有projectileModel和projectileMono类，其中Model类是用来存储从外部导入的投射物数据，比如在外部数据库编辑好的某个投射物，他的射速、射线弧度、投射物到达目标地点后产生的屏幕特效、投射物与敌人碰撞后产生的屏幕特效等。

而对于Mono类，则用来管理一个具体的游戏里面的投射物的生命周期，在Mono类中，管理投射物造成的伤害计算、向目标飞过去的逻辑等等。

每个Mono类中都有一个对应的Model类充当他的属性。

### 为什么要引入看起来变扭的Model类和Mono类 ###
因为平常都是从游戏外部通过 sqlite、csv、json 来导入数据进Unity的，然后导入的数据需要用一个新建的对象来保存它，但是MonoBehavior对象是不能随意创建的，但是游戏逻辑的部分又关乎MonoBehavior类，就是挂载到游戏对象上的类必须继承自MonoBehavior，所以只好在中间新增一个Model类来充当外部数据与MonoBehavior类的中介者。

### Model类和Mono类应该遵循怎样的设计原则 ###
一般来说，Model类只需要存储从外部导入的游戏数据就好了，相当于一个中间的存储容器.

但是，我觉得稍微漂亮一点的写法是把一些关乎游戏数据计算的方法也写在Model类中,比如:伤害计算公式等等.

关于一些非数值计算的游戏逻辑,写在Mono类中,比如音效、动画的播放.
## 技能设置描述 ##

所有技能都在外处使用编辑器进行编辑，以（.json\\ .csv\\ .xml\\ .sqllit）的方式进行保存。
所有技能都会有特效动画的产生，特效动画还可分为立即释放型特效，投射型特效等，特效动画由外部进行指定，类型为GameObject，暂定以JSON的形式保存一个技能，其动画特效（释放时敌方产生的特效及我方产生的特效）是一个字符串类型，该字符串指定了在Resource/Prefabs/文件夹下的粒子特效预制体的名称。

## 技能 ##
### ***基类 BaseSkill*** ###
1. 属性
	 1. 技能名称 ： skillName : string  :  <==
	 2. 技能图标 ： Icon : string  :  <==
	 3. 技能描述 ： description : string  :  <==
2. 方法
	1. Excute() : Damage 执行该技能效果伤害效果
	2. pass
### ***主动技能类 ActiveSkill < BaseSkill*** ###
1. 属性
	 1. 要消耗的魔法值 ： mp:int  :  <==
	 2. 基础伤害 ： baseDamage:Damage  :  <==
	 3. 附加伤害 ： plusDamage:Damage  :  <==
	 4. 热键 ： keyCode:KeyCode  :  <==
	 5. 技能CD时间 ： coolDown:float  :  <==
	 6. 施法距离 ： spellDistance:float  :  <==
	 7. 最后一次施法的时间 ： finalSpellTime:float
2. 方法
	1. Execute() : Damage 计算伤害
### ***被动技能类 PassiveSkill < BaseSkill*** ###
1. 属性
	 1. pass
2. 方法
	1. pass
### ***指向型技能类 PointingSkill < ActiveSkill*** ###
1. 属性
	 1. pass
2. 方法
	1. pass	
### ***无指向型技能类 NoPointingSkill < ActiveSkill*** ###
1. 属性
	 1. pass
2. 方法
	1. pass	
	
## 单位 ##
需要明确的是，每一个单位都在外部由编辑器提前设定。
### 普通单位Character ###
1. 属性
	1. 血量 HP：int  :  <==
	2. 魔法值 Mp：int  :  <==
	3. 最大血量 maxHp：int  :  <==
	4. 最大魔法值 maxMp：int  :  <==
	5. 名称 name：string  :  <==
	6. 攻击距离 attackDistance：float  :  <== 
	7. 该单位拥有的所有技能 baseSkills：List<BaseSkill>  :  <==
	8. 该单位拥有的所有主动技能 activeSkills ： List< ActiveSkill >
	9. 该单位拥有的所有被动技能 passiveSkills : List< PassiveSkill > 
	10. 攻击力 attack ： int  :  <==
	11. 攻击类型 attackType ： AttackType  :  <==
	12. 攻击间隔（以秒为单位，每经过一段攻击间隔，便进行攻击一次） attackSpeed : float  :  <==
	12. 防御力 defense ： int  :  <==
	13. 防御类型 defenseType ： DefenseType  :  <==
	14. 移动速度 movingSpeed ： int  :  <==
	15. 转身速度 turningSpeed ： int  :  <==
	16. 投射物 projectile ： Projectile  :  <==
	17. 等级 level ： int  :  <==
	18. 回血速度 restoreHpSpeed : float  :  <==
	19. 回魔速度 restoreMpSpeed : float  :  <==
	20. 是否可被攻击（无敌） canBeAttacked : boolean  :  <==
	21. 单位类型 unitType : UnitType  :  <==
	22. 单位阵营 unityFaction : UnitFaction  :  <==
	
---
### 英雄单位 Hero < Character ###
1. 属性
	1. 力量 forcePower : int  :  <==
	2. 敏捷 agilePower : int  :  <==
	3. 智力 intelligencePower : int  :  <==
	4. 技能点 skillPoint : int  :  <==
	5. 经验值 exp : int  :  <==
	
---
### 特殊单位 投射物Projectile < Characte[canBeAttacked=false] ###
投射物是一个比较特殊的单位，在一些技能和单位的远程攻击里面出现，投射物一般有一个目标位置。

该投射物到达指定地点后会产生一个球形伤害。

#### 投射物的伤害计算 ####
要明确的一点逻辑是，投射物本身是不具有伤害的，具有伤害的是发出投射物的单位，所有投射物都是由单位（继承自Character的所有子类）产生的，由该Character类指定伤害，也就是Damaged，投射物在计算伤害时，直接使用该Damaged类来计算伤害。

对于一些攻击，如普通攻击等，需要计算敌我的防御值、攻击值，才能产生伤害，这些都是由Character类做的事情，当Character做完这些事之后，将产生的Damaged伤害类传递给投射物就OK了。

#### 投射物属性 ####
1. 目标位置 targetPosition : vector3
2. 目标敌人 target : CharacterMono
3. 移动速度 speed : float  :  <==
4. 投射物到达指定地点时产生的特效 targetPositionEffect : GameObject  :  <==
5. 投射物击中敌人时,在敌人身上产生的特效 targetEnemryEffect : GameObject  :  <==
#### 投射物的飞行轨迹 ####
对于一个投射物来说，他有两种飞行轨迹：  
　　　　　　一种是平的，也就是直接从某处平移到某处,这种飞行轨迹容易实现,只需让投射物对着目标地点进行均匀的平移就可以了  
　　　　　　一种是有弧线的,这种飞行轨迹一般用于弓箭、炮弹、飞行生物降落的攻击等投射物上,这种飞行轨迹较难实现,因为对于弓箭这种投射物,它的旋转角度也会发生改变,对于从高处降下,目前的想法是给物体增加刚体组件
## 战斗系统规则 ##
### 判定伤害 ###
1. 对于普通攻击来说，只有当一个人物动作完整的播放完攻击动画时，才对对面给予伤害，最终伤害判定根据被攻击者和攻击者的距离来判定（这里针对近战攻击），距离每超过攻击者的攻击范围的10%，被攻击者的闪避率增加10%。
2. 所有伤害判定均由"**伤害类**"来进行判定，所有普通攻击，伤害技能....等等，最终都会产生伤害类，由伤害类来判断最终给予目标的伤害。
3. 对于拥有投射物的单位，其攻击逻辑与直接造成伤害的不一样，当攻击动画播放完毕后，会自动在发射点创建一个投射物向敌人进行移动，当投射物碰到敌人时，造成伤害。
### 技能 ###
1. 所有技能都有一个基类，基类Skill包含了技能的基本特性，如：造成的伤害，出现的状态等等，拥有一个通用的用于计算技能伤害的方法，该方法将会产生一个伤害类，并执行此伤害类。
2. 技能细分下来，分为主动技能和被动技能,主动技能中分为指向性技能、原地释放技能等等，这些分为的种类，都各自写一个类。
3. 对于技能的编辑，到时可以撸一个类似Rm的技能编辑窗口，这个窗口最终将编辑好的技能保存为Json、CSV、sqlite等数据集合

## AI设计 ##

### ● NPC部分 ###
---
### 守卫型NPC行动分析（如：防守塔，野区野怪） ###
1. 基本操作有:攻击、移动、返回原地点
2. 攻击：当此NPC周围出现敌人时，自动对此敌人进行攻击，在这里分为两种情况。
	1. 对于有移动能力的守卫NPC，将对目标进行追击，直到目标跑出视野，返回原区域
	2. 对于没有移动能力的守卫NPC，则对目标进行攻击，当目标跑出攻击区域，自动停止攻击
3. 移动：这里针对有移动能力的NPC，当此NPC周围出现敌人时，对该敌人进行追击。
4. 返回原地：当此NPC周围没有敌人时，自动返回原本的根据地。


### 进攻型普通AI行动分析（如：固定出兵攻打敌方基地的小兵） ###
1. 基本操作：攻击、移动
2. 行为描述：
	1. 小兵从固定地点产出
	2. 小兵按照 3塔、2塔、高地塔、基地的顺序对敌方进行进攻
	3. 小兵按照固定的路线向地方前进
	4. 当小兵在移动中发现敌人时，将敌人消灭后继续前进
	5. 小兵毁灭3塔后，向2塔前进，毁灭2塔后，向1塔前进，毁灭1塔后，向基地前进，基地毁灭，阵营胜利。
3. 逻辑分析：

（伪码）

	if(周围有敌人)：
		消灭周围敌人
	else:
		进攻3、2、1塔、基地
	

### 处理人物死亡逻辑 ###
当人物HP降为0时，人物死亡。
进入死亡状态的单位，停止目前一切动作，播放死亡动画，同时设置isDying为true，表示人物正在垂死状态中，当死亡动画播放完毕后，Destory该单位。（这个Destory可以有多种意思，在有对象池的情况下，这个Destory可能仅仅只是收回对象回池内而已）


### ● 玩家部分 ###
---
### 人物行动分析 ###

1. 基本人物操作有：攻击、移动、施法、吃药、换装备。
2. 移动：鼠标对某处点击右键，当目标不是敌人时，进行移动操作。移动开始时，播放移动动画，显示移动特效，移动结束后，移动动画结束播放。
3. 攻击：当鼠标对某处点击右键且目标是敌人时，进行攻击操作。
	1. 攻击操作准备开始时，首先判断主角当前位置和敌人的距离是否是可以攻击的距离，如果不可以攻击，那就移动到目标敌人的位置上进行攻击，如果可以攻击，那么进行攻击操作。
	2. 当追击敌人的时候，如果敌人跑出视线范围，那么就自动放弃追击。否则会一直穷追不舍。
	2. 攻击开始时，播放角色攻击动画，此时进行逻辑判断，当角色攻击动画完成后（这里有近战及远程攻击的区别），对敌人进行伤害处理。
4. 施法：当按下某个未在冷却中且为主动技能的法术时，进行施法操作。（暂时只考虑指向敌人型技能和原地释放技能）
	1. 施法操作准备开始时，鼠标变换图片，变成一个带有指向性的攻击图标。
	2. 鼠标指针图标变换完成后，鼠标右键单击敌人，开始施法。
	3. 施法开始时，判断是否有施法时间 。
		1. 有的话，播放持续施法动画，持续施法动画播放结束后，进入2。
		2. 没有的话，播放施法动画，判断施法动画是否结束，当施法动画结束后，播放我方特效动画以及敌方特效动画，对伤害进行结算。（此处伤害结算包含了立即型伤害以及放一个触发器去碰敌人然后使敌人受到伤害）
5. 吃药：可以将药品理解为消耗性技能，其判断与施法大体是相同的，同样是判断按键，同样是吃药动画、伤害结算等等，唯一不同的地方是，吃药是可以把要吃完的。
6. 换装备：暂不实现。


### 尝试使用行为树 ###

---

### 尝试使用状态机 ###
#### 状态机注意事项 ####


#### 1.施法状态的编写 ####
施法考虑到技能有指向型技能（指向型技能又分为有必须单击敌人才能释放的单指向型技能和按照一定范围释放【类似WOW里面的"暴风雪"】的技能）与原地释放技能的区别.

所以在anyState的状态更新中判断用户是否按下了释放技能的按键(关于技能冷却,是否够mp放技能,都可以放到anyState的OnUpdate里面判断).当用户按下释放技能的按键且有足够条件释放技能时,设置CharacterMono中的prePareSkill技能类且使isPrePareUseSkill为True.

设置anyState状态状态的Transition类,该Transition定义了从anyState到Spell状态的规则.规则如下:


1. 当isPrePareUseSkill为True且释放的技能为原地释放技能时,进入Spell状态,设置isImmediatelySpell为True

2. 当释放的技能为指向型技能时(暂时不划分单指向型技能和范围指向技能),当玩家用鼠标单击敌人的时候,进入Spell状态,设置isImmediatelySpell为False


#### 2.施法状态的转移 ####
需要注意的是,施放技能状态和其他状态最大的不同是,施放技能状态会自动结束,并回到Idle状态.

#### 3.状态机图片 ####

![Avater](readmeImage/stateMachine.png)



## 开发日记 ##
### 2018/10/7 ###
#### Question ###
　　①现在面临的问题是,怎么将表示游戏数据类的Model类和表示UI的View或ViewModel类绑定到一起,达到Model类的属性改变,视图也一起改变的效果.   
　　比较蛋疼的是,不能把Model类写成ViewModel类的样式,不然后面写起来会特别麻烦并且可能会出现意料之外的问题,同时,不能用BinableProperty修饰Model类的属性,因为Model类是要存储从外部输入的游戏数据,如果使用BinableProperty来修饰,又要涉及到一些问题.

　　②要对AI和游戏逻辑进行一次分离,也就是说,人物攻击,施法,造成伤害,移动,这些都是人物本身的游戏逻辑,跟AI本身没什么关系,完全不用写在"行为树/状态机"里面,所以在这里要分离AI的逻辑和基本逻辑,让游戏逻辑写在Mono类里面,而AI到时需要攻击时,只需要调用Mono类的Attack方法就行了,不需要在行为树/状态机里面费劲的进行播放动画,计算伤害等操作.  
　　这里有个比较蛋疼的问题,一开始我是根据动画是否播放结束来判断伤害的,当人物动画播放结束时,对敌人造成伤害。但是这样做有一个问题，一个单位的攻击速度特别依赖于他的
#### Answer ####
　　①用了一个比较拙劣的方法,就是,在Model和ViewModel之间再加一层绑定.  
　　也就是,当Model对象的值改变的时候,ViewModel的值也一起随之改变,然后由于ViewModel的改变,导致视图View的变化,这样就使得Model类和View视图类有了一个间接的绑定.  
　　好吧,我知道这有点脱裤子放屁的感觉.........

　　