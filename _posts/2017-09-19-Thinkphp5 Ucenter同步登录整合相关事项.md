#写在前面
* 网上目前没有发现`tp5`和`Ucenter`整合的例子，我这里的这个项目是基于`tp5`接口方式整合`Ucenter`同步登陆退出以及免激活的方案。
* `tp5`的入口文件在项目文件下的`public`文件夹（默认），是所以上面的`tp3`的教程均以`public`目录为基础。
* `uc_client`文件夹放在`public`目录，入口`api`文件夹放在`public`目录，下午为展示目录（已删除不相关的东西）![image](http://olv6ixm8v.bkt.clouddn.com/uc_center_file.png)
* 这里的`discuz`是集成`ucenter`的，内包含了客户端和服务端，这里我只用了他的服务端，没有使用客户端。装`discuz`的时候默认会给`ucenter`装好的。
#配置过程

##大致步骤
将需要的文件放入相应的位置 （`uc_client`客户端文件夹，入口`api`文件夹，引用`Api`文件夹，可以一边做一边放）
1. 更改入口文件`uc.php`配置
2. 配置测试通讯控制器`Index.php`
3. 配置通讯接口和Demo
4. 配置免激活

##通讯配置
* 网上现有的例子是`tp3`的而且只是调试通通讯的例子，不过该作者写的这个东西对我改造成`tp5`有很大帮助，该帖子链接为[http://www.thinkphp.cn/code/1843.html](http://www.thinkphp.cn/code/1843.html)，需要加群才能获取到下载地址。前面`Ucenter`建立要通讯的应用配置参考这个就行了。
* 第二个参考是这个链接[http://www.121ask.com/thread-5530-1.html](http://www.121ask.com/thread-5530-1.html)，文中的2配置步骤，在代码里是有2处的，在`if`和`else`判断里都有，所以总共改4处地方。原文中的这个（未更改原文主要内容）
![image](http://olv6ixm8v.bkt.clouddn.com/uc_center_file.png3.png)
```
1、打开ucenter的后窗口在应用中添加所需要的应用，这里不详细说了，就按ucenter中的提示添写即可，关键是在添加完成以后要复制如下的内容,如上图（代码块贴不了图，放上面）

然后在ucenter1.6中的advanced\examples\api\中的api目录复制到你所对应的项目根目录下，然后在建立一个conf.ucenter.php配置文件。把应用Ucenter的配置信息复制到该文件，并做相对应的修改，然后在api/uc.php文件对ucenter引用配置文件的路径做修改

原有的：require_once DISCUZ_ROOT.'./config.inc.php';修改成为你所对应的文件名和路径，（注意：DISCUZ_ROOT这个常量是../相对的路径，根据自己的情况做修改）

2、把advanced\uc_client中的uc_client目录复制到项目的根目录，现在到ucenter后台查看通信是失败的，在此纠结了很长时间，查看在api/uc.php文件中有一行 require_once DISCUZ_ROOT.'./include/db_mysql.class.php';这个。

而所在项目中根本找不到些文件，这是就是受uncentet案例中的影响，要把此行修改成为    require_once DISCUZ_ROOT.'./uc_client/lib/db.class.php';。是在uc_client/lib/db.class.php所对应的文件，那么此处修改完了以后还是通信失败的。然后在api/uc.php文件做修改
  原有的：     
//$GLOBALS['db'] = new dbstuff;
 //$GLOBALS['db']->connect($dbhost, $dbuser, $dbpw, $dbname, $pconnect, true, $dbcharset);
 //$GLOBALS['tablepre'] = $tablepre;
   修改以后的
  $GLOBALS['db'] = new ucclient_db;
  $GLOBALS['db']->connect(UC_DBHOST, UC_DBUSER, UC_DBPW, UC_DBNAME, UC_DBCONNECT, true, UC_DBCHARSET);
  $GLOBALS['tablepre'] = UC_DBTABLEPRE;
然后保存，现在返回到ucenter后台中的应用列表中查看是通信成功的。
```
这边的项目配置好后的`uc.php`改动代码为（原代码都是以注释的方式，未删除）：
```
//note 普通的 http 通知方式
if(!defined('IN_UC')) {

	error_reporting(0);
	set_magic_quotes_runtime(0);

	defined('MAGIC_QUOTES_GPC') || define('MAGIC_QUOTES_GPC', get_magic_quotes_gpc());
	// require_once DISCUZ_ROOT.'./config.inc.php';
	require_once DISCUZ_ROOT.'./api/conf.ucenter.php';

	$_DCACHE = $get = $post = array();

	$code = @$_GET['code'];
	parse_str(_authcode($code, 'DECODE', UC_KEY), $get);
	if(MAGIC_QUOTES_GPC) {
		$get = _stripslashes($get);
	}

	$timestamp = time();
	if($timestamp - $get['time'] > 3600) {
		exit('Authracation has expiried');
	}
	if(empty($get)) {
		exit('Invalid Request');
	}
	$action = $get['action'];

	require_once DISCUZ_ROOT.'./uc_client/lib/xml.class.php';
	$post = xml_unserialize(file_get_contents('php://input'));

	if(in_array($get['action'], array('test', 'deleteuser', 'renameuser', 'gettag', 'synlogin', 'synlogout', 'updatepw', 'updatebadwords', 'updatehosts', 'updateapps', 'updateclient', 'updatecredit', 'getcreditsettings', 'updatecreditsettings'))) {
		// require_once DISCUZ_ROOT.'./include/db_mysql.class.php';
		require_once DISCUZ_ROOT.'./uc_client/lib/db.class.php';
		// $GLOBALS['db'] = new dbstuff;
		// $GLOBALS['db']->connect($dbhost, $dbuser, $dbpw, $dbname, $pconnect, true, $dbcharset);
		// $GLOBALS['tablepre'] = $tablepre;
		$GLOBALS['db'] = new ucclient_db;
		$GLOBALS['db']->connect(UC_DBHOST, UC_DBUSER, UC_DBPW, UC_DBNAME, UC_DBCONNECT, true, UC_DBCHARSET);
		$GLOBALS['tablepre'] = UC_DBTABLEPRE;
		unset($dbhost, $dbuser, $dbpw, $dbname, $pconnect);
		$uc_note = new uc_note();
		exit($uc_note->$get['action']($get, $post));
	} else {
		exit(API_RETURN_FAILED);
	}

//note include 通知方式
} else {

	// require_once DISCUZ_ROOT.'./config.inc.php';
	require_once DISCUZ_ROOT.'./api/conf.ucenter.php';
	// require_once DISCUZ_ROOT.'./include/db_mysql.class.php';
	require_once DISCUZ_ROOT.'./uc_client/lib/db.class.php';
	// $GLOBALS['db'] = new dbstuff;
	// $GLOBALS['db']->connect($dbhost, $dbuser, $dbpw, $dbname, $pconnect, true, $dbcharset);
	// $GLOBALS['tablepre'] = $tablepre;
	$GLOBALS['db'] = new ucclient_db;
	$GLOBALS['db']->connect(UC_DBHOST, UC_DBUSER, UC_DBPW, UC_DBNAME, UC_DBCONNECT, true, UC_DBCHARSET);
	$GLOBALS['tablepre'] = UC_DBTABLEPRE;
	unset($dbhost, $dbuser, $dbpw, $dbname, $pconnect);
}
```
* 通讯测试，在第一步下载的项目包里的引用文件（一共3个），如下图所示。图中的`ucenter`是模块名字，这三个文件的代码不用更改，可以直接用。
![image](http://olv6ixm8v.bkt.clouddn.com/uc_center_file2.png)
* 下载的项目包里的测试控制器需要改造成`tp5`的形式 在`/application/ucenter/controller/Index.php`下写入如下代码，即可测试通讯通过，去后台刷新应用，没问题的话应该会通讯成功的。
```
<?php
namespace app\ucenter\controller;
use think\Db;
use think\Request;
use Ucenter\Api\UcenterLib;
class Index {
    public function index(){
    	UcenterLib::back();
    }
}
```
* 在这里有个比较坑的地方，就是有时候本地通了服务器不通过，有时候服务器通了本地又不通，后来才知道是缓存的问题，配置好测试通讯之前，最好把本地环境和服务器环境的缓存清除一次，这样保证不会因为缓存的问题浪费时间。
##接口配置
* 由于我这边是接口形式，所以只写了公共方法来调用，这边我的需求只有简单的同步登陆和退出。在`/application/ucenter/event/`目录下创建`Ucservice.php`文件下写入代码，参考开发者文档，可以很快的写出一份接口，代码如下：
```
<?php
namespace app\ucenter\event;
use think\Db;
use think\Request;
use think\Cache;
use Ucenter\Api\UcenterLib;
class Ucservice
{
    public function __construct(){
        //BASE_PATH在入品的配置文件中建立一个常量，并赋值define('BASE_PATH',dirname(__FILE__));
        //上段注释是上面的其中一个连接教程参考过来的，由于`tp5`的路径不一样，所以才用了自己定义了路径，不才，发现这样可以使用就这么使用下来了。
        define('BASE_PATH',dirname(dirname(dirname(dirname(__FILE__)))));
        require_once(BASE_PATH . '/public/api/conf.ucenter.php');
        require_once(BASE_PATH . '/public/uc_client/client.php');
    }

    //会员注册接口
    /*
    $username 用户名
    $password 密码
    */
    public function uc_user_registerp($username,$password,$email='')
    {
        // if (!$email) {
            $topdomain = Cache::get('topdomain');
            if (!$topdomain) {
                $topdomain = get_sysconfig(5);
                Cache::set('topdomain',$topdomain);
            }
            $email = $username.'@'.$topdomain;
        // }
        $uid = uc_user_register($username, $password, $email);//UCenter的注册验证函数
        if($uid <= 0) {
            if($uid == -1) {
            return pbarray(3201,'用户名不合法');
            } elseif($uid == -2) {
            return pbarray(3202,'包含不允许注册的词语');
            } elseif($uid == -3) {
            return pbarray(3203,'用户名已经存在');
            } elseif($uid == -4) {
            return pbarray(3204,'Email 格式有误');
            } elseif($uid == -5) {
            return pbarray(3205,'Email 不允许注册');
            } elseif($uid == -6) {
            return pbarray(3206,'该 Email 已经被注册');
            } else {
            return pbarray(3207,'未定义');
            }
        } else {
            return pbarray(999,'',intval($uid));//返回一个非负数
        }
    }

    //会员同步登陆接口
    /*
    $username 用户名
    $password 密码
    */
    public function uc_user_loginp($username,$password)
    {
        list($uid, $usernameout, $passwordout, $emailout) = uc_user_login($username, $password);
        if($uid > 0) {
            $result = uc_user_synlogin($uid);
            if (strlen($result) > 10) {
                return pbarray(999,'',$result);
            } else {
                return pbarray(3204,'未定义');
            }
        } elseif($uid == -1) {
            return pbarray(3201,'用户不存在,或者被删除');
        } elseif($uid == -2) {
            return pbarray(3202,'密码错误');
        } else {
            return pbarray(3203,'未定义');
        }
    }

    //会员同步退出接口
    public function uc_user_synlogoutp()
    {
        $result = uc_user_synlogout();
        if (strlen($result) > 10) {
            return pbarray(999,'',$result);
        } else {
            return pbarray(3204,'未定义');
        }
    }

    // 获取用户数据
    public function uc_get_userp($username)
    {
        if($data = uc_get_user($username)) {
            list($uid, $username, $email) = $data;
            return [$uid,$username,$email];
        } else {
            return false;
        }
    }
}
```
* Demo示例：
```
// discuz 同步注册、登录、退出 Demo
    public function discuzdemo()
    {
        $Ucservice_event = controller('ucenter/Ucservice', 'event');
        $res_uc0         = $Ucservice_event->uc_get_userp($username);//获取用户信息
        $res_uc1         = $Ucservice_event->uc_user_registerp($username,$password);//注册用户
        $res_uc2         = $Ucservice_event->uc_user_loginp($username,$password);//用户登陆和同步登陆
        $res_uc3         = $Ucservice_event->uc_user_synlogoutp();//退出
        $this->assign('name',$res_uc2['data']['data']);//返回一段js，放入网页中执行即可。
        $this->assign('name',$res_uc3['data']['data']);//返回一段js，放入网页中执即可。
        return view();
    }
```
* 这里有一个比较坑的，我这边需要的同步登陆和同步推出根据文档说明是需要将返回的代码放入网页，而我们用的是接口形式，所以在同步登陆或者同步退出之后，返回了该js，并由前端来放入网页中，如果是直接使用的话，Demo里的例子直接输出的话是有效的。

##免激活设置
__免激活设置最好留在最后一步做，要不然这里出现问题导致整个通信不成功会比较麻烦的。__
这样配置好后还是需要激活才能使用的，所以需要处理一下免激活，后台可以设置一个免验证码激活，第一次还是需要激活，那么在网上找到如下的链接[http://www.ngro.org/tech/ucenter-synclogin-activation.html](http://www.ngro.org/tech/ucenter-synclogin-activation.html)
按照教程配置是没有问题的，__唯一的问题是他的代码是把数据库表名和表前缀写死的__，所以这点比较尴尬，我也没有进行处理，以下为该教程的部分内容。
```
修改各个程序目录下的 ./uc_client/model/user.php 文件，大概在 129 行处的 function add_user 函数里添加代码
如 Discuz X 的：
```
```
$this->db->query("INSERT INTO `dbname`.pre_common_member SET uid='$uid', username='$username', password='$password', email='$email', adminid='0', groupid='10', regdate='".$this->base->time."', credits='0', timeoffset='9999'");
$this->db->query("INSERT INTO `dbname`.pre_common_member_status SET uid='$uid', regip='$regip', lastip='$regip', lastvisit='".$this->base->time."', lastactivity='".$this->base->time."', lastpost='0', lastsendmail='0'");
$this->db->query("INSERT INTO `dbname`.pre_common_member_profile SET uid='$uid'");
$this->db->query("INSERT INTO `dbname`.pre_common_member_field_forum SET uid='$uid'");
$this->db->query("INSERT INTO `dbname`.pre_common_member_field_home SET uid='$uid'");
$this->db->query("INSERT INTO `dbname`.pre_common_member_count SET uid='$uid', extcredits1='0', extcredits2='0', extcredits3='0', extcredits4='0', extcredits5='0', extcredits6='0', extcredits7='0', extcredits8='0'");
```
```
Discuz 的参数比较多，`dbname` 是数据库名，.pre_ 是表前缀，按自己的情况修改，注意需要在 MySQL 设置相应的权限，假设 Discuz X 和 UCenter 是在不同的数据库且不同数据库用户，Discuz X 数据库和用户是 discuz，UCenter 的数据库和用户是 ucenter，那么需要设置 ucenter 拥有数据库 discuz 的 insert 权限。(如果是同一数据库、同一用户则忽略这些步骤)
phpMyAdmin 的操作步骤大概为：权限 -> 编辑权限 -> 按数据库指定权限 -> 选择数据库 -> 勾选 INSERT -> 执行。
这些代码的原理就是，在某应用注册用户时，同时添加其它应用的数据库字段，因为 UCenter 在首次注册时并没有这一步骤所以才没能同步登录与激活。

修改后完整的 function add_user 函数是这样的：
```
```
function add_user($username, $password, $email, $uid = 0, $questionid = '', $answer = '', $regip = '') {
	$regip = empty($regip) ? $this->base->onlineip : $regip;
	$salt = substr(uniqid(rand()), -6);
	$password = md5(md5($password).$salt);
	$sqladd = $uid ? "uid='".intval($uid)."'," : '';
	$sqladd .= $questionid > 0 ? " secques='".$this->quescrypt($questionid, $answer)."'," : " secques='',";
	$this->db->query("INSERT INTO ".UC_DBTABLEPRE."members SET $sqladd username='$username', password='$password', email='$email', regip='$regip', regdate='".$this->base->time."', salt='$salt'");
	$uid = $this->db->insert_id();
	$this->db->query("INSERT INTO ".UC_DBTABLEPRE."memberfields SET uid='$uid'");
	// BEGIN
	$this->db->query("INSERT INTO `dbname`.pre_common_member SET uid='$uid', username='$username', password='$password', email='$email', adminid='0', groupid='10', regdate='".$this->base->time."', credits='0', timeoffset='9999'");
	$this->db->query("INSERT INTO `dbname`.pre_common_member_status SET uid='$uid', regip='$regip', lastip='$regip', lastvisit='".$this->base->time."', lastactivity='".$this->base->time."', lastpost='0', lastsendmail='0'");
	$this->db->query("INSERT INTO `dbname`.pre_common_member_profile SET uid='$uid'");
	$this->db->query("INSERT INTO `dbname`.pre_common_member_field_forum SET uid='$uid'");
	$this->db->query("INSERT INTO `dbname`.pre_common_member_field_home SET uid='$uid'");
	$this->db->query("INSERT INTO `dbname`.pre_common_member_count SET uid='$uid', extcredits1='0', extcredits2='0', extcredits3='0', extcredits4='0', extcredits5='0', extcredits6='0', extcredits7='0', extcredits8='0'");
	// END
	return $uid;
}
```
#附录
* 开发者文档下载地址：[http://www.discuz.net/thread-879237-1-1.html](http://www.discuz.net/thread-879237-1-1.html)
* 相关下载链接：[http://pan.baidu.com/s/1i5HnVdr](http://pan.baidu.com/s/1i5HnVdr) 密码：uo8j（目前有开发者文档，教程网页html缓存版）
* tp3和ucenter整合例子如外网失效我将公布地址，原作者肯定希望宣传加群，而不公网分享。