##Unity开发NGUI代码实现ScrollView(滚动视图)

**下载NGUI包**
*导入NGUI3.9.1版本package* 
- 链接: http://pan.baidu.com/s/1mgksPBU 密码: bacy

**导入NGUI包**
![](http://images0.cnblogs.com/blog2015/662304/201508/201133009102412.png)


**创建MainCameraScript.cs脚本**
`MainCameraScript.cs`
```
using UnityEngine;
using System.Collections;

public class MainCameraScript : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}

```
**创建NGUI根节点的方法**
```
	private GameObject Window{ set; get;}

	void CreateUI()
	{
		//创建根节点
		this.Window = NGUITools.CreateUI(false).gameObject;
	}
```

**在Window上添加滚动子视图**
```
	void CreateUI()
	{
		//创建根节点
		this.Window = NGUITools.CreateUI(false).gameObject;

		//在根节点上创建一个UIScrollView子控件
		UIScrollView scrollView = NGUITools.AddChild<UIScrollView>(this.Window);

	}
```

**在滚动视图上添加Grid表格调整布局**
```
	void CreateUI()
	{
		//创建根节点
		this.Window = NGUITools.CreateUI(false).gameObject;

		//在根节点上创建一个UIScrollView子控件
		UIScrollView scrollView = NGUITools.AddChild<UIScrollView>(this.Window);

		//在滚动视图上添加UIGrid子控件,来调整布局
		UIGrid	grid = NGUITools.AddChild<UIGrid>(scrollView.gameObject);

		//设置grid表格的布局方向
		grid.arrangement = UIGrid.Arrangement.Horizontal;
		grid.cellWidth = 100.0f;
		grid.cellHeight = 100.0f;
		grid.animateSmoothly = false;
	}
```

**添加创建Label的方法**
```
	/// <summary>
	/// 创建一个小Label控件
	/// </summary>
	/// <returns>The label item.</returns>
	UILabel CreateLabelItem(string name,GameObject parent)
	{
		//创建一个Lalel控件
		UILabel label = NGUITools.AddChild<UILabel> (parent);

		//修改Label的字体及颜色
		Font f = (Font)Resources.Load ("Arial", typeof(Font));
		label.bitmapFont = NGUITools.AddMissingComponent<UIFont>(label.gameObject);
		label.bitmapFont.dynamicFont = f;
		label.color = Color.red;

		//设置Label要显示的文字
		label.text = name;

		//添加滚动ScrollView时要用到的碰撞器和脚本
		label.autoResizeBoxCollider = true;
		NGUITools.AddMissingComponent<UIDragScrollView> (label.gameObject);
		NGUITools.AddMissingComponent<BoxCollider> (label.gameObject);

		//重新调整碰撞器的大小
		label.ResizeCollider ();
        
        return label;
	}
```
**在Grid上添加10个Label控件**
```
	void CreateUI()
	{
		//创建根节点
		this.Window = NGUITools.CreateUI(false).gameObject;

		//在根节点上创建一个UIScrollView子控件
		UIScrollView scrollView = NGUITools.AddChild<UIScrollView>(this.Window);

		//在滚动视图上添加UIGrid子控件,来调整布局
		UIGrid	grid = NGUITools.AddChild<UIGrid>(scrollView.gameObject);

		//设置grid表格的布局方向
		grid.arrangement = UIGrid.Arrangement.Horizontal;
		grid.cellWidth = 100.0f;
		grid.cellHeight = 100.0f;
		grid.animateSmoothly = false;

		//在Grid表格上添加20个Label对象
		for (int i = 0; i < 10; i++) 
		{
			CreateLabelItem (Random.Range (100, 999).ToString(), grid.gameObject);
		}

		//重新排版
		grid.Reposition ();
	}
```

**整个MainCameraScript.cs的代码如下**
```
using UnityEngine;
using System.Collections;

public class MainCameraScript : MonoBehaviour {


	private GameObject Window{ set; get;}

	void CreateUI()
	{
		//创建根节点
		this.Window = NGUITools.CreateUI(false).gameObject;

		//在根节点上创建一个UIScrollView子控件
		UIScrollView scrollView = NGUITools.AddChild<UIScrollView>(this.Window);

		//在滚动视图上添加UIGrid子控件,来调整布局
		UIGrid	grid = NGUITools.AddChild<UIGrid>(scrollView.gameObject);

		//设置grid表格的布局方向
		grid.arrangement = UIGrid.Arrangement.Horizontal;
		grid.cellWidth = 100.0f;
		grid.cellHeight = 100.0f;
		grid.animateSmoothly = false;

		//在Grid表格上添加20个Label对象
		for (int i = 0; i < 10; i++) 
		{
			CreateLabelItem (Random.Range (100, 999).ToString(), grid.gameObject);
		}

		//重新排版
		grid.Reposition ();
	}

	/// <summary>
	/// 创建一个小Label控件
	/// </summary>
	/// <returns>The label item.</returns>
	UILabel CreateLabelItem(string name,GameObject parent)
	{
		//创建一个Lalel控件
		UILabel label = NGUITools.AddChild<UILabel> (parent);

		//修改Label的字体及颜色
		Font f = (Font)Resources.Load ("Arial", typeof(Font));
		label.bitmapFont = NGUITools.AddMissingComponent<UIFont>(label.gameObject);
		label.bitmapFont.dynamicFont = f;
		label.color = Color.red;

		//设置Label要显示的文字
		label.text = name;

		//添加滚动ScrollView时要用到的碰撞器和脚本
		label.autoResizeBoxCollider = true;
		NGUITools.AddMissingComponent<UIDragScrollView> (label.gameObject);
		NGUITools.AddMissingComponent<BoxCollider> (label.gameObject);

		//重新调整碰撞器的大小
		label.ResizeCollider ();
        
        return label;
	}

	// Use this for initialization
	void Start () {

		CreateUI ();
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

**效果如下**
![](http://images0.cnblogs.com/blog2015/662304/201508/201132374727852.gif)