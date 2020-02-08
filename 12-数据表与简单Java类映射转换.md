# 数据表与简单Java类映射转换

在实际开发中，数据表与简单JAVA类之间的基本映射关系如下：
* 数据实体表设计 = 类的定义 ；
* 表中的字段 = 类的成员属性 ；
* 表的外键关联 = 引用关联 ；
* 表的一行记录 = 类的一个实例化对象 ；
* 表的多行记录 = 对象数组 。

![数据表1](https://github.com/JCancy/JAVA/blob/master/picture/%E6%95%B0%E6%8D%AE%E8%A1%A81.PNG)

以上的护具表存在如下关联：
* 一个部门有多个雇员；
* 一个雇员属于一个部门；
* 一个雇员有一个领导；

将上表数据转为简单java类的定义形式，在整体代码之中要求可以获得如下数据信息：
* 根据部门信息获得以下内容：
	* 一个部门的完整信息；
	* 一个部门之中所有雇员的完整信息；
	* 一个雇员对应的领导信息；
* 根据雇员信息获得以下内容：
	* 一个雇员所在部门信息；
	* 一个雇员对应的领导信息。

对于数据表和简单JAVA类之间的映射最好的解决步骤：先抛开所有的关联字段不看，写出类的基本组成，之后再通过引用配置关联字段的关系。

第一步：定义DEPT和EMP两个实体类		
```java
class Dept{
	private long deptno ;//描述数据表主键的建议类型为long
	private String dname ;
	private String loc ;
	public Dept(long deptno,String dname,String loc) {
		this.deptno = deptno ;
		this.dname = dname ;
		this.loc = loc ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【部门信息】部门编号：" + this.deptno + "、部门名称：" + this.dname + "、部门位置：" + this.loc ;
	}
}
class Emp {
	private long empno ; 
	private String ename ; 
	private String job ;
	private double sal ;
	private double comm ;
	public Emp(long empno, String ename, String job, double sal,double comm) {
		this.empno = empno ;
		this.ename = ename ;
		this.job = job ;
		this.sal = sal ;
		this.comm = comm ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【雇员信息】雇员编号：" + this.empno + "、雇员名称：" + this.ename + "、雇员职位：" + this.job + "、雇员工资："  + this.sal + "、佣金：" + this.comm;
	}
}
public class JavaDemo {
	public static void main(String args[]) {

	}
}
```


第二步：配置所有关联字段		
```java
class Dept{
	private long deptno ;//描述数据表主键的建议类型为long
	private String dname ;
	private String loc ;
	public Emp emps [] ; //多个雇员信息
	public Dept(long deptno,String dname,String loc) {
		this.deptno = deptno ;
		this.dname = dname ;
		this.loc = loc ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【部门信息】部门编号：" + this.deptno + "、部门名称：" + this.dname + "、部门位置：" + this.loc ;
	}
	public void setEmps(Emp emps []) {
		this.emps = emps ;
	}
	public Emp getEmps() [] { //对象数组
		return this.emps ;
	}
}
class Emp {
	private long empno ; 
	private String ename ; 
	private String job ;
	private double sal ;
	private double comm ;
	private Dept dept ; //所属部门
	private Emp mgr ; //所属领导
	public Emp(long empno, String ename, String job, double sal,double comm) {
		this.empno = empno ;
		this.ename = ename ;
		this.job = job ;
		this.sal = sal ;
		this.comm = comm ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【雇员信息】雇员编号：" + this.empno + "、雇员名称：" + this.ename + "、雇员职位：" + this.job + "、雇员工资："  + this.sal + "、佣金：" + this.comm;
	}
	public void setDept(Dept dept) {
		this.dept = dept ;
	}
	public void setMgr(Emp mgr) {
		this.mgr = mgr
	}
	public Dept getDept() {
		return this.dept ;
	}
	public Emp getMgr() {
		return this.mgr ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {

	}
}
```


第三步：实现需求		
```java
class Dept{
	private long deptno ;//描述数据表主键的建议类型为long
	private String dname ;
	private String loc ;
	public Emp emps [] ; //多个雇员信息
	public Dept(long deptno,String dname,String loc) {
		this.deptno = deptno ;
		this.dname = dname ;
		this.loc = loc ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【部门信息】部门编号：" + this.deptno + "、部门名称：" + this.dname + "、部门位置：" + this.loc ;
	}
	public void setEmps(Emp emps []) {
		this.emps = emps ;
	}
	public Emp getEmps() [] { //对象数组
		return this.emps ;
	}
}
class Emp {
	private long empno ; 
	private String ename ; 
	private String job ;
	private double sal ;
	private double comm ;
	private Dept dept ; //所属部门
	private Emp mgr ; //所属领导
	public Emp(long empno, String ename, String job, double sal,double comm) {
		this.empno = empno ;
		this.ename = ename ;
		this.job = job ;
		this.sal = sal ;
		this.comm = comm ;
	}
	// setter,getter,无参构造省略
	public String getInfo() {
		return "【雇员信息】雇员编号：" + this.empno + "、雇员名称：" + this.ename + "、雇员职位：" + this.job + "、雇员工资："  + this.sal + "、佣金：" + this.comm;
	}
	public void setDept(Dept dept) {
		this.dept = dept ;
	}
	public void setMgr(Emp mgr) {
		this.mgr = mgr ;
	}
	public Dept getDept() {
		return this.dept ;
	}
	public Emp getMgr() {
		return this.mgr ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		//第一步：根据关系进行类的定义
		//定义出各个实例化对象，此时并没有任何关联定义
		Dept dept = new Dept(10,"财务部","上海") ;
		Emp empA = new Emp(1230L,"张三","售后",800.00,0.0) ;
		Emp empB = new Emp(1231L,"李四","前台",3000.00,0.0) ;
		Emp empC = new Emp(1232L,"王五","经理",8000.00,0.0) ;
		//需要为对象进行关联的设置
		empA.setDept(dept) ; //设置雇员与部门的联系
		empB.setDept(dept) ; //设置雇员与部门的联系
		empC.setDept(dept) ; //设置雇员与部门的联系
		empA.setMgr(empB) ; //设置雇员与领导的关联
		empB.setMgr(empC) ; //设置雇员与领导的关联
		dept.setEmps(new Emp[] {empA,empB,empC}) ; //部门与雇员的联系
		//第二步：根据关系获取数据
		System.out.println(dept.getInfo()) ; //部门信息
		for (int x = 0 ; x < dept.getEmps().length ; x ++) {
			System.out.println("\t|-" + dept.getEmps()[x].getInfo()) ;
			if (dept.getEmps()[x].getMgr() != null) { //领导不为空时输出领导
				System.out.println("\t\t|-" + dept.getEmps()[x].getMgr().getInfo()) ;
			}
		}
		System.out.println("------------------") ;
		System.out.println(empB.getDept().getInfo()) ; //根据雇员获取部门信息
		System.out.println(empB.getMgr().getInfo()) ; //根据雇员获取领导信息
	}
}
```


## 一对多映射

![数据表2](https://github.com/JCancy/JAVA/blob/master/picture/%E6%95%B0%E6%8D%AE%E8%A1%A82.PNG)

按照表的要求将表的结构转换为类结构，同时可获取如下信息：
* 获取一个分类的完整信息 ；
* 可以根据分类获取其对应的所有子分类信息。

```java
class Item {
	private long iid ;
	private String title ;
	private Subitem subitems [] ;
	public Item(long iid, String title) {
		this.iid = iid ;
		this.title = title ;
	}
	public String getInfo() {
		return "【分类信息】iid:" + this.iid + "、title：" + this.title ;
	}
	public void setSubitems(Subitem subitems []) {
		this.subitems = subitems ;
	}
	public Subitem getSubitems() [] {
		return this.subitems ;
	}
}
class Subitem {
	private long sid ;
	private String title ;
	private Item item ;
	public Subitem(long sid, String title) {
		this.sid = sid ;
		this.title = title ;
	}
	public String getInfo() {
		return "【子分类信息】sid:" + this.sid + "、title：" + this.title ;
	}
	public void setItem(Item item) {
		this.item = item ;
	}
	public Item getItem() {
		return this.item ;
	}
}
public class JavaDemo {
	public static void main(String args []) {
		//第一步：根据结构设置对象数据
		Item item = new Item(1L,"图书") ;
		Subitem subitems [] = new Subitem [] {
			new Subitem(10L,"编程类") ,
			new Subitem(10L,"文学类") } ;
		item.setSubitems(subitems) ; //一个分类下有多个子分类
		for (int x = 0 ; x < subitems.length ; x ++) {
			subitems[x].setItem(item) ;
		}
		//第二步：根据要求获取数据
		System.out.println(item.getInfo()) ;
		for (int x = 0 ; x < item.getSubitems().length ; x ++) {
			System.out.println("\t|-" + item.getSubitems()[x].getInfo()) ;
		}
		
	}
}
```


## 多对多映射

![数据表3](https://github.com/JCancy/JAVA/blob/master/picture/%E6%95%B0%E6%8D%AE%E8%A1%A83.PNG)

将以上结构转换为类结构，并且获取如下信息：
* 获取一个用户访问的所有商品的详细信息；
* 获取一个商品被浏览过的全部用户信息。

此时程序只需要考虑实体表设计，不需要考虑访问信息表，只定义两个类即可。

```java
class Member {
	private String mid ;
	private String name ;
	private Product products [] ; //一个人看过多个商品
	public Member(String mid, String name) {
		this.mid = mid ;
		this.name = name ;
	}
	public String getInfo() {
		return "【用户信息】mid:" + this.mid + "、name:" + this.name ;
	}
	public void setProducts(Product products []) {
		this.products = products ;
	}
	public Product getProducts() [] {
		return this.products ;
	}
}
class Product {
	private long pid ;
	private String title ;
	private double price ;
	private Member members [] ; //一个商品有多个人看过
	public Product(long pid, String title, double price) {
		this.pid = pid ;
		this.title = title ;
		this.price = price ;
	}
	public String getInfo(){
		return "【商品信息】pid:" + this.pid + "、title:" + this.title + "、price:" + this.price ;
	}
	public void setMembers(Member members []) {
		this.members = members ;
	}
	public Member getMembers() [] {
		return this.members ;
	}
}
public class JavaDemo {
	public static void main(String args []) {
		//第一步：根据结构设置对象数据
		Member memA = new Member("qwer","张三") ;
		Member memB = new Member("tyui","李四") ;
		Product proA = new Product(1L,"乐高",80.00) ;
		Product proB = new Product(2L,"电脑",5000.00) ;
		Product proC = new Product(3L,"手机",8000.00) ;
		memA.setProducts(new Product[] {proA,proB,proC}) ;
		memB.setProducts(new Product[] {proA}) ;
		proA.setMembers(new Member[] {memA,memB}) ;
		proB.setMembers(new Member[] {memA}) ;
		proC.setMembers(new Member[] {memA}) ;
		//第二步：根据要求获取数据
		System.out.println("-------根据用户查看浏览信息---------") ;
		System.out.println(memA.getInfo()) ;
		for (int x = 0 ; x < memA.getProducts().length ; x ++) {
			System.out.println("\t|-" + memA.getProducts()[x].getInfo()) ;
		}
		System.out.println("-------根据商品查看用户信息---------") ;
		System.out.println(proA.getInfo()) ;
		for (int x = 0 ; x < proA.getMembers().length ; x ++) {
			System.out.println("\t|-" + proA.getMembers()[x].getInfo()) ;
		}
	}
}
```


## 复杂多对多映射

![数据表4](https://github.com/JCancy/JAVA/blob/master/picture/%E6%95%B0%E6%8D%AE%E8%A1%A84.PNG)

上图的基本关系：
* 一个用户可以有多个角色，一个角色可能有多个用户；
* 一个角色可以拥有多个权限。


要求实现如下功能：
* 根据用户找到其对应的所有角色，以及每一个角色对应的所有权限；
* 根据角色找到其权限，以及拥有此角色的用户；
* 根据权限找到拥有该权限的所有用户信息。

```java
class Member {
	private String mid ;
	private String name ;
	public Role roles [] ; //一个用户有多个角色
	public Member(String mid, String name) {
		this.mid = mid ;
		this.name = name ;
	}
	public String getInfo() {
		return "【用户信息】mid:" + this.mid + "、name:" + this.name ;
	}
	public void setRoles(Role roles []) {
		this.roles = roles ;
	}
	public Role getRoles() [] {
		return this.roles ;
	}
}
class Role {
	private long rid ;
	private String title ;
	public Member members [] ; //一个角色有多个用户
	public Privilege privileges [] ; //一个用户有多个权限
	public Role(long rid, String title) {
		this.rid = rid ;
		this.title = title ;
	}
	public String getInfo() {
		return "【角色信息】rid:" + this.rid + "、title:" + this.title ;
	}
	public void setMembers(Member members []) {
		this.members = members ;
	}
	public void setPrivileges(Privilege privileges []) {
		this.privileges = privileges ;
	}
	public Member getMembers() [] {
		return this.members ;
	}
	public Privilege getPrivileges() [] {
		return this.privileges ;
	}
}
class Privilege {
	private long pid ;
	private String title ;
	private Role role ; //一个权限属于一个角色
	public Privilege(long pid, String title) {
		this.pid = pid ;
		this.title = title ;
	}
	public String getInfo() {
		return "【权限信息】pid:" + this.pid + "、title:" + this.title ;
	}
	public void setRole(Role role) {
		this.role = role ;
	}
	public Role getRole() {
		return this.role ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		//第一步：根据结构设置对象数据
		Member memA = new Member("a","张三") ;
		Member memB = new Member("b","李四") ;
		Role roleA = new Role(1L,"系统配置") ;
		Role roleB = new Role(2L,"备份管理") ;
		Role roleC = new Role(3L,"人事管理") ;
		Privilege priA = new Privilege(1000L,"系统初始化") ;
		Privilege priB = new Privilege(1001L,"系统还原") ;
		Privilege priC = new Privilege(1002L,"系统环境修改") ;
		Privilege priD = new Privilege(2001L,"备份员工数据") ;
		Privilege priE = new Privilege(2002L,"备份部门数据") ;
		Privilege priF = new Privilege(2000L,"备份公文数据") ;
		Privilege priG = new Privilege(3000L,"增加员工") ;
		Privilege priH = new Privilege(3001L,"编辑员工") ;
		Privilege priI = new Privilege(3002L,"查看员工") ;
		Privilege priJ = new Privilege(3003L,"删除员工") ;
		//增加角色与权限对应关系
		roleA.setPrivileges(new Privilege [] {priA,priB,priC}) ;
		roleB.setPrivileges(new Privilege [] {priD,priE,priF}) ;
		roleC.setPrivileges(new Privilege [] {priG,priH,priI,priJ}) ;
		//增加权限与角色的对应关系
		priA.setRole(roleA) ;
		priB.setRole(roleA) ;
		priC.setRole(roleA) ;
		priD.setRole(roleB) ;
		priE.setRole(roleB) ;
		priF.setRole(roleB) ;
		priG.setRole(roleC) ;
		priH.setRole(roleC) ;
		priI.setRole(roleC) ;
		priJ.setRole(roleC) ;
		//增加用户与角色的对应关系
		roleA.setMembers(new Member [] {memA,memB}) ;
		roleB.setMembers(new Member [] {memA,memB}) ;
		roleC.setMembers(new Member [] {memB}) ;
		memA.setRoles(new Role [] {roleA,roleB}) ;
		memB.setRoles(new Role [] {roleA,roleB,roleC}) ;
		//第二步：根据要求获取数据
		System.out.println("-------根据用户查找信息-------------");
		System.out.println(memB.getInfo()) ;
		for (int x = 0 ; x < memB.getRoles().length ; x ++ ) {
			System.out.println("\t|-" + memB.getRoles()[x].getInfo()) ;
			for (int y = 0 ; y < memB.getRoles()[x].getPrivileges().length ; y ++) {
				System.out.println("\t\t|-" + memB.getRoles()[x].getPrivileges()[y].getInfo()) ;
			}
		}
		System.out.println("-------根据角色查找信息-------------");
		System.out.println(roleB.getInfo()) ;
		System.out.println("-------此角色的所有权限-------------");
		for (int x = 0; x < roleB.getPrivileges().length ; x ++ ) {
			System.out.println("\t|-" + roleB.getPrivileges()[x].getInfo()) ;	
		}
		System.out.println("-------此角色的所有用户-------------");
		for (int x = 0 ; x < roleB.getMembers().length ; x ++) {
			System.out.println("\t|-" + roleB.getMembers()[x].getInfo()) ;
		}
		System.out.println("-------根据权限查找信息-------------");
		System.out.println(priA.getInfo()) ;
		for (int x = 0; x < priA.getRole().getMembers().length ; x ++) {
			System.out.println("\t|-" + priA.getRole().getMembers()[x].getInfo()) ;
		}
	}
}
```
