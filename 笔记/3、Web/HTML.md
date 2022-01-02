# 文本

### 	标题标签

```html
<h1>  h1  </h1>
<h2>  h2  </h2>
<h3>  h3  </h3>
<h4>  h4  </h4>
<h5>  h5  </h5>
<h6>  h6  </h6>
```

### 	段落

```html
<p>
        这是一个段落    这是一个段落    这是一个段落    这是一个段落    这是一个段落    这是一个段落
</p>
```

### 	格式

```html
<br/>换行
<hr/>水平线
<b>粗体</b>
<i>斜体</i>
<small>小号字</small>
<del>删除字</del>
<ins>插入字</ins>
<sub>下标字</sub>
<sup>上标字</sup>
```

### 	列表

```html
无序列表
<ul>
    <li>列表项目</li>
</ul>

有序列表
<ol>
    <li>列表项目</li>
</ol>
```

### 	表格

```html
<table>表格
    <caption>表格标题</caption>
    <tr>行
    	<th>表头单元格</th>
    </tr>
    <tr>
        <td>表格中的单元格</td>
    </tr>
</table>
<table>
    <thead>表头</thead>
    <tfoot>表低栏</tfoot>
    <tbody>表格体</tbody>
</table>
```

### 	表单

```html
<form>所有输入框都包含在form标签中
    <input type="">表单标签
    	type指表单类型，主要有一下几种类型：
    		text		单行文本框
    		radio		单选按钮
    		checkbox	复选按钮
    		password	密码框
    <select name="" id="">下拉菜单
        <option value="alipay">支付宝</option>
    </select>
    <textarea name="" id="" cols="30" rows="10">多行文本框
    </textarea>
    按钮：表单中常见的按钮有三种，都是input标签，type属性值不一样
	    	button		普通按钮（自行添加事件）
    		submit		提交（提交表单）
    		reset		重置（清空输入框内容）
    
    MORE：更多type种类
    		color		颜色选择器
    		data/time	日期/时间选择控件
    		email		电子邮件输入控件
    		file		文件选择控件
    		number		数字输入控件
    		range		拖拽条
    		search		搜索框
    		url			网址输入控件
    <label>与input标签进行绑定，当点击label标签时，input标签获得焦点
        <input type="text">
    </label>
    <datalist id="province-list">
        为输入框提供备选项
        <option value="陕西">
        <option value="北京">
        <option value="湖南">
        <option value="广东">
        <option value="山东">
        <option value="山西">
        <option value="江苏">
     </datalist>
</form>
```

### 	图像与影音

```html
<figure>定义媒介分组（类似div）
    <p>This is a log</p>
    <img src="./temp/log.png" usemap="#aaa" alt="log">引入图片
    <map name="aaa" id="aaa">对图片区域做连接
        <area shape="rect" coords="0,0,130,41" href ="./temp/aaa.jpg" alt="regit"/>定义一个区域
    </map>
    <figcaption>log</figcaption>
</figure>


```

### 	连接

```html
<a href="https://www.baidu.com">百度</a>
<a href="https://www.baidu.com"><img src="./temp/aaa.jpg" ></a>
```

# 小节

### 	定义节

```html
<div>   </div>	占据整行
<span>  </span>	行内元素
<header>   </header>	页眉
<footer>   </footer>	页脚
<section>  </section>	定义节
<article>  </article>	定义文章
<aside>    </aside>		定义页面内容之外的内容
<details>  </details>	定义元素细节
<dialog>   </dialog>	对话框或窗口
<summary>  </summary>	
```
