



> ***计算机技术*** <span id="MSG" style="color:red"></span>



```dataviewjs
// Date input
// dv.span("Date: ");
// let today = dv.date("today");
// let eleDate = dv.el("input");
// eleDate.value = dv.date("today").toString().substring(0,10); //取年月日
// eleDate.style.width = "120px";

// 书名 input
dv.span("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;");
dv.span("书名(关键字): ");
let eleName = dv.el("input");
eleName.style.width = "120px";

// 作者 input
dv.span("&nbsp;&nbsp;");
dv.span("作者(关键字): ");
let eleAuther = dv.el("input");
eleAuther.style.width = "120px";

// 状态选项 想读 在读 读过
dv.span("&nbsp;&nbsp;");
dv.span("状态: ");
const statusOptionsText = ["所有","想读", "在读", "读过"];
const statusOptionsValue = ["","💡想读","📆在读","✅读过"];
const statusDropdown = this.container.createEl("select");
const statusOption = [];
for(let i = 0;i<statusOptionsText.length;i++)
{
	statusOption[i] = statusDropdown.createEl("option");
	statusOption[i].text = statusOptionsText[i];
	statusOption[i].value = statusOptionsValue[i];
}


// 类别 
dv.span("&nbsp;&nbsp;");
dv.span("标签: ");
const typeOptionsText = ["所有","编程语言","数据结构与算法","操作系统","计算机网络","计算机组成原理","程序设计","嵌入式","人工智能","数据分析","职业规划"];
const typeOptionsValue = ["","编程语言","数据结构与算法","操作系统","计算机网络","计算机组成原理","程序设计","嵌入式","人工智能","数据分析","职业规划"];
const typeDropdown = this.container.createEl("select");
const typeOption = [];
for(let i = 0;i<typeOptionsText.length;i++)
{
	typeOption[i] = typeDropdown.createEl("option");
	typeOption[i].text = typeOptionsText[i];
	typeOption[i].value = typeOptionsValue[i];
}



// query button 搜索按键
dv.span("&nbsp;&nbsp;");
let eleBtn = dv.el("button","搜索")

dv.span("<br><br>");

function msg(sMsg) {
  new Notice(sMsg);
  MSG.innerText = sMsg;
}

eleBtn.onfocus = function() {
  MSG.innerText = "";
}


// 查询

eleBtn.onclick = function() {

	var table = document.querySelector('.dataview.table-view-table');

	if (table) {
	
	  // 获取表格元素的父节点
	  var parent = table.parentNode;
	
	  // 获取父节点的上一级节点（即祖父节点）
	  var grandparent = parent.parentNode;
	
	  // 从祖父节点中移除父节点
	  grandparent.removeChild(parent);
	
	}
	let data = []
	try {
		let sName = ""; // 书名匹配字符串
		let sAuther = ""; // 作者
		if(eleName.value != "") // 判空
		{
			sName = eleName.value; 
		}
		if(eleAuther.value != "") // 判空
		{
			sAuther = eleAuther.value;
		}
		// 获得可选框的值
		const statusSelectedValue = statusDropdown.value;
		const typeSelectedValue = typeDropdown.value;

		data = dv.pages('"800_Library/Book/书库/T_工业技术/TP_计算机技术" and #书籍')
			.filter(p=>String(p.file.name).includes(sName))
			.filter(p=>String(p.作者).includes(sAuther))
			.filter(p=>String(p.状态).includes(statusSelectedValue))
			.filter(p=>String(p.tags).includes(typeSelectedValue))
			.map(p => ["![|60]("+ p.封面 +")",p.file.link,p.作者,p.豆瓣评分,p.总页数,p.file.etags,p.状态]);
	} catch(e){ // 报错
		MSG.innerText = "[error] "+ e.message;
	}
	msg("匹配结果: " + data.length);
	// let md_table = dv.markdownTable(["File("+data.length+")",], data);
	let md_table = dv.markdownTable(["封面","书名","作者","豆瓣评分","总页数","标签","状态"], data);
	if (data.length > 0) {
		// dv.header(2, sDate);
		// dv.span(table);
		dv.table(["封面","书名","作者","豆瓣评分","总页数","标签","状态"], data);
		// dv.markdownTable(["File"], data);
		// dv.paragraph(md_table);
		navigator.clipboard.writeText(md_table);
	  }
}

```


