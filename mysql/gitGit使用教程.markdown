# Git使用教程

## 1、Git的作用

- 代码回溯
- 版本切换
- 多人协助
- 远程备份

## 2、什么是Git

### **一：Git是什么？**

​    Git是目前世界上最先进的分布式版本控制系统。

### **二：SVN与Git的最主要的区别？**

   SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就纳闷了。

   Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

![image-20220503120633037](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031206265.png)

> 进行下载Git可以在官方网站下载
>
> ![image-20220503120840563](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031208651.png)
>
> 下载完成后，在任意位置点击右键，可以下看到：
>
> ![image-20220503121014899](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031210994.png)

## 3、Git代码托管服务

![image-20220503121136823](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031211946.png)

> 其中的码云是国内的代码托管平台，速度还是很多的；推荐使用码云。

## 4、Git常用命令

### 一、Git全局设置

![image-20220503121857710](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031218796.png)

我们在目录下创建一个空目录，使用Git Bash here; 

![image-20220503122314352](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031223387.png)

​	可以看到在这里设置了 当前用户的name 和mail作为标记，可以识别出来是那个用户使用Git提交的信息。用处就是为了识别出提交信息的人是那个。

**获取Git仓库**

​	要使用Git对我们的代码进行版本控制，首先需要获得Git仓库。获取GIt仓库的方法有两种：一种是在本地初始化一个Git仓库（不常用）；一种是从远程仓库克隆（常用）。

**第一种，在本地初始化Git仓库：**

![image-20220503123105547](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031231615.png)

进行实际操作：

![image-20220503123447702](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031234743.png)

---

**第二种，从远程仓库进行克隆：**

![image-20220503123746681](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031237772.png)

在本地进行克隆远程的Git仓库，需要我们先去Git中创建一个GIt仓库，在创建后可以去复制远程仓库的地址供我们使用：

![image-20220503125130196](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031251248.png)

> 这个首先我们需要在Git中去创一个仓库；在自己的gitee中的：
>
> ![image-20220503123928788](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031239827.png)
>
> ![image-20220503124229286](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031242357.png)
>
> 在创建好，我们在进入仓库中：
>
> ![image-20220503124558516](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031245604.png)



### 二、工作区，缓存区，版本库概念

![image-20220503125339929](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031253039.png)

> 我们初始化和克隆下面的仓库下会有一个隐藏目录 .git  里面的文件不要随机修改，为了避免我们在操作git时出现错误。



### 三、Git工作区中文件的状态

Git工作区中的文件状态有两种：

- untracked  未跟踪（未被纳入版本控制）
- tracked  已跟踪（被纳入版本控制）
  - unmodified  未修改状态
  - modified  已修改
  - staged 已缓存状态

> 注意：这些文件的主管那天会随着我们执行Git的命令发生变化；
>
> ​	同时我们在本地进行操作Git仓库时，一定要注意我们目录选择的位置，避免在非Git仓库下进行操作，在我们进行初始化或者是克隆的仓库，我们要是需要执行一个命令，需要到含有 .git  的隐藏目录下进行操作。

![image-20220503131307288](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031313345.png)

![image-20220503131722973](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031317003.png)



### 四、本地仓库操作

![image-20220503131822852](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031318911.png)

使用 git reset  （文件） 把指定的文件进行清理出缓存区， 要是没有添加文件，可以把缓存区中所有文件全部清理掉。

![image-20220503132111216](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031321259.png)

​	使用git commit 进行把缓存区里面的文件进行提交到，在本地仓库就是表示未修改状态。其中我们在使用 git commit -m "手动添加日志" （文件名）--进行提交指定的文件  ，也可以不加文件名，这样可以把我们缓存区里面的文件全部进行提交处理，同时也会把没有添加发到缓存区的文件进行自动的加入缓存区后在进行提交。

![image-20220503133217809](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031332867.png)

​	我们可以使用git log可以查看前面进行提交的历史版本；并且配合这个使用git reset --hard 历史版本号就可以进行回溯到之前的版本

![image-20220503133913632](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031339663.png)

![image-20220503134440824](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031344865.png)



### 五、远程仓库操作

![image-20220503134640653](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031346721.png)

​	我们可以使用git remote 可以查看我们的远程仓库，并且当我们使用git remote -v可以查看远程仓库的详细信息（其中它会列出每个远程服务器的简写，如果是克隆远程仓库，至少可以看到origin 这是Git克隆的仓库服务器的默认的名字）：

![image-20220503135128946](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031351983.png)

假如是自己在本地使用初始化一个Git仓库，并想把它连接到远程的仓库可以使用  git remote  add 远程仓库的地址：

![image-20220503135422936](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031354035.png)

![image-20220503140830083](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031408210.png)

我们从远程仓库拉取文件使用git pull origin master；进行拉取。

![image-20220503141001036](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031410158.png)

![image-20220503141542854](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031415882.png)

> 注意：出现这种错误说明本地仓库中存在文件，而从远程仓库进行拉取是会以文件冲突，来拒绝从远程仓库拉取文件，我们可以在git pull 后面加上 --allow-unrelated-histories
>
> ![image-20220503141914088](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031419128.png)

把我们从在本地git仓库添加文件后，给它推送到远程仓库。

![image-20220503140345629](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031403756.png)

### 六、分支操作

![image-20220503142404327](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031424429.png)

在使用git branch 可以查看分支也可以进行创建分支，

![image-20220503143041756](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031430854.png)

![image-20220503214237242](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205032142406.png)

​	上面可以看使用 ` git branch`进行查看分支，而使用`git branch -a`可以看到分支的详细信息。 

​	使用`git branch [创建分支的名称]` 可以进行创建一个新的分支。在创建完新分支后，我们可以使用`git checkout [分支的名称]`可以进行切换分支，这样就可以换到类一个分支。

> 这些操作创建分支全是创建在本地上，我们需要把分支推送到远程仓库上需要使用 git push origin 分支名称 给它添加到远程仓库上。
>
> ![image-20220503214953965](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205032149086.png)
>
> ![image-20220503143506510](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031435551.png)
>
> 这样我们可以在回到git中可以查看我们分支从一个master 变成了两个 一个master 一个test1 ：
> ![image-20220503143614215](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031436242.png)

**进行分支合并使用 git merge 加分支名称**：下图是我们在master主分支下面进行操作合并b3，也可以在其他的分支下面合并

![image-20220503143905211](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031439260.png)

在进行合并分支时使用`git merge [分支名称]`，如上图 在master分支下面进行合并b3分支。

> 在合并分支时，若是master分支下有一个test.txt 文件里面有内容，那么我们在的b3也有这个test.txt文件但是里面的内容不一样，在进行合并时会进行一个报错
>
> ![image-20220503144836969](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031448007.png)
>
> 我们可以查看master的test。txt文件:
>
> ![image-20220503144931453](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031449498.png)
>
> 这样我们进行修改文件内容在进行`git add test.txt` 进行添加到缓存中，在进行`git commit -m"合并" test.txt`  进行提交 ，而在执行合并时可能会遇到一个这样错误：
>
> ![image-20220503145332666](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031453698.png)
>
> 要是遇到这问题就是说明在执行合并时不能只提交一部分。可以使用`git commit -m"合并" test.txt -i`进行处理；
>
> 在进行把本地的文件推送到远程仓库去 `git push origin master`

### 七、标签操作

​	Git中标签，指的是某个分支某个特定时间点的状态，通过标签，可以很方便的切换到标记时的状态。比价有代表性的是人们会使用这个功能来标记发布结点（v1.0 、v1.2等等）；（类似于虚拟机中的快照。）

![image-20220503145959041](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031459100.png)

在我们在使用`git tag `可以进行查看仓库中的标签（版本号，类似于虚拟机中的快照），而在使用`git tag v1.1`可以创建一个版本号为v1.1的的标签。

![image-20220503220442489](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205032204541.png)

同时我们在创建好标签后，我们在使用`git push origin v0.1`，这样我们就把创建的标签v0.1推送到远程仓库里面了 

![image-20220503220354098](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205032203147.png)

我们在创建好标签后，这样我们就能够得到这个版本来供我们使用，但是我们需要先创建一个分支来存放标签：

![image-20220503221034846](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205032210919.png)

> 为什么获取标签前需要先进行创建一个分支来存放这个标签 ？
>
> 由于创建好的标签（版本），它是就像当有虚拟机中的快照，这样这个标签就无法进行修改，当我们进行获取这个标签后，需要对里面的文件进行添加或者修改，这样我们使用创建一个新的分支来存储这个标签版本，供我们对这个新的分支来处理标签版本。



## 5、在IDEA中配置Git

### 一、在idea中配置git

​	在下载好Git后，以及进行尝试着使用它和连接远程仓库后。我们是在使用idea来使用git这样可以帮我们把写的代码存放到码云中去管理，也能随时的去拉取之前的代码等等。

​	首先我们在idea的设置中可以知道**版本控制Version Control**种找个Git  会显示出我们之前安装好git的存放git.exe文件目录，我们点击test能够看到git的版本就说明没有问题，这样我们点击应用就行。

![image-20220503150810103](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031508227.png)

### 二、获取git仓库

​	在应用完Git后，我们可以在idea的控制区可以看到**VCS(S)**我们点击后，可以看到**Get from Version Control**点击后可以看到Git 点击Git 弹出一个git的克隆窗口。

![image-20220504101130755](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041011888.png)

这个是我们可以从远程仓库去克隆下来仓库里面的代码（这样使用般是团队合作时，我们进行克隆项目下来供我们进行工作）；

​	然而我们在自己使用时，就是先进行**import into version Control** 再 **create Git Repository** 创建一个本地仓库，这样我们的项目中就会出现一个.git的隐藏文件。

![image-20220503152509872](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031525966.png)

### 三、本地仓库操作

​	在进行连接好仓库后，我们在进行对项目操作。开始我们项目里面的类会从黑色变成红色。在创建新的类时会提示我们是否将新建类加图缓存区：
![image-20220504102038101](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041020147.png)

在我们进行加入缓存区后，文件会从红色变为绿色。

我们也可以点击文件找到git 可以看到一个**+Add** 也可以把文件添加到缓存区，上面还有一个**commit文件** 可以把文件进行提交。

![image-20220504102235797](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041022837.png)

> 想这种用是git的常用操作，这样idea帮我们生成一个工具，
>
> ![image-20220504102532516](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041025544.png)
>
> 我们可以点击**对号**进行提交要是还没有添加到缓存区，它也会把我们把文件先加入缓存区在进行提交。

### 四、远程仓库操作

​	创造远程仓库和本地仓库类似；

​	但是我们在使用本地仓库后在去连接远程仓库：我们可以单击项目名：找Git ：

![image-20220504103003203](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041030248.png)

在仓库管理这个我们可以点击Remotes进添加远程仓库的url：

![image-20220504103237676](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041032705.png)

在连接好后，我们先把远程仓库进行拉取下来，在推送上去，要是拉取失败，我们可以先找到项目的文件进入含有.git文件夹下面进入**git bash**在里面使用命令选进行拉取  一般出现错误是：

![image-20220504103936005](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041039052.png)

这样我们可以使用`git pull origin master  --allow-unrelated-histories`先进行这样操作。要是想把文件推送到远程仓库也可以在idea中点击**对号**进行提交。

![image-20220504104346823](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041043856.png)

选择提交的同时并进行推送就行。要是这里没有这样选择，还可以点击项目找到Git  里面有个push，点击也是一样。
![image-20220504104606109](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041046132.png)

要是在push过程中出现被拒绝，可以在idea的下面**控制台**那里找到**version Control**  我们可以在这里进查看我们的日志，以及出现错误的问题。

![image-20220504105655824](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041056851.png)

在推送中要是遇到这种情况，说明没有没有分支存在，这样我们就需要进行创建分支。

### 五、分支操作

![image-20220503170907773](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205031709847.png)

![image-20220504105316014](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041053049.png)

我们在创建分支可以在idea的右下角看到Git点击进入后，我们可以看到：![image-20220504105401566](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041054600.png)

其中最下面表示我们进行连接了 远程仓库。上面的一个本地的一个仓库，并且我们可以看到一个master1后面有origin/maeter 表示我们这个分支是我们远程仓库的分支。

### 六、标签

​	我们的项目在开发一定程度上可以使用标签把我们现在的开发进度给记录下来。

![image-20220504110858512](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041108542.png)

点击**tag** 进行打一个标签。

![image-20220504110939275](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041109309.png)

在进入里面后给标签进行输入名称 等信息。

然后这样我们在打好标签后它是进行保存的本地的，然后我们在点击**对号**进行提交，在提价的页面下进行**√选tag的选项**进推送这样就可以把标签进行推送到远程仓库。

