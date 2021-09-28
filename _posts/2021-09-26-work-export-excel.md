---
layout: post
title: 前端导出excel
tags: export excel 
categories: work
---

* TOC 
{:toc}

前端直接对数据进行封装成excel进行下载，[参考文档](https://www.cnblogs.com/WhiteM/p/14068701.html)。  
根据我们的实际使用进行了部分修改，现对代码进行总结。

---

# 创建xlxs.js

```
import XLSX2 from 'xlsx';
import XLSX from 'xlsx-style';

class ToXlsx {
  // id指的是绑定数据的table的id，
  // title指的是导出表格的名称，记得带后缀xlsx，例如：title='重复导.xlsx';
  constructor (id, title) {
    this.id = id;
    this.title = title;
  }
  
  /**
   * 自定义表格
   * @returns {Promise<void>}
   */
  async createCustomizeTable(dataList,prop,colName) {
		prop = prop ? prop : [];
	  colName = colName ? colName : "default";
    var sheet = XLSX2.utils.table_to_sheet(document.querySelector(this.id),{raw:true});
    // 设置单个单元格样式 ，A2对应的是excel表格的A2
    // sheet["A2"].s = {
    //   alignment: {
    //     horizontal: "center",
    //     vertical: "center",
    //     wrap_text: true
    //   }
    //};
	var len = dataList.length ? dataList.length  : 0 ;
	if(len > 0){
		let arr =["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z","AA","AB","AC","AD","AE","AF","AG","AH","AI","AJ","AK","AL","AM","AN","AO","AP","AQ","AR","AS","AT","AU","AV","AW","AX","AY","AZ"];
		var fullref = sheet["!fullref"];
		var subNum = 2;
		if(fullref.length > 5){
			subNum = 3;
		}
		var addNum = parseInt(fullref.substr(fullref.indexOf(":")+subNum,fullref.length));
		fullref = fullref.substr(0,fullref.indexOf(":")+subNum) + (addNum+len);
		sheet["!fullref"] = fullref;
		sheet["!ref"] = fullref;
		for(var i = 0 ; i < len ; i++){
			var tempMap = dataList[i] ? dataList[i] : {};
			for(var j= 0 ; j < prop.length ; j++){
				var key = prop[j];
				var cell = arr[j]+(i+addNum+1);
				sheet[cell] =  {
					t: "s",
					v: tempMap[key]
				};
			}
		}
	}
	var closList = [];
    // sheet居然是个对象，所以遍历就用for in
    // 偷个懒，因为要所有的表格都居中，所以这里就用for in 遍历设置了，如果只是单个设置，那就用上面的单独设置就行
    for (var key in sheet){
      if (new RegExp('^[A-Za-z0-9]+$').exec(key)) {
		     var cell = key.toString();
		  if(cell.indexOf(colName) >= 0){
			  closList.push({wpx: 240});
		  }else{
			closList.push({wpx: 160});  
		  }
     
		if(sheet[cell].t != "z"){
			sheet[cell].s = {
				alignment: {
					horizontal: "center", // 水平对齐-居中
					vertical: "center", // 垂直对齐-居中
					wrap_text: true,
				},
				border: {
					top: { style: "thin" },
					left: { style: "thin" },
					bottom: { style: "thin" },
					right: { style: "thin" }
				}
			}
		}else{
			sheet[cell].s = {
				alignment: {
					horizontal: "center", // 水平对齐-居中
					vertical: "center", // 垂直对齐-居中
					wrap_text: true,
				},
				border: {
					left: { style: "thin" }
				}
			}
		}
      }
    }
	this.addRangeBorder(sheet['!merges'],sheet);
	sheet['!cols'] = closList;
    // wpx 字段表示以像素为单位，wch 字段表示以字符为单位
    // 注意，必须从第一个开始设置，不能只设置单独一个
    // !cows是列宽
   /* sheet['cols'] = [
          {wch: 16}, // A列
          {wch: 16}, // B列
          {wch: 16}, // C列
          {wch: 16}, // D列
    ];
   
    // ！rows设置的行高
    sheet['!rows'] = [
          {wpx: 40,}, // 1行
          {wpx: 40}, // 2行
          {wpx: 40}, // 3行
          {wpx: 40}, // 4行
    ]; */
    try {
      this.openDownloadDialog(this.sheet2blob(sheet), this.title);
    } catch (e) {
		this.msgError('excel创建失败')
    }
  }
  
  addRangeBorder(range, ws) {
          // s:起始位置，e:结束位置
        let arr =["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
        range.forEach((item) => {
          let startRowNumber = Number(item.s.r), startColumnNumber = Number(item.s.c),endColumnNumber = Number(item.e.c),
            endRowNumber = Number(item.e.r);
          //   合并单元格时会丢失边框样式，例如A1->A4 此时内容在A1 而range内获取到的是从0开始的，所以开始行数要+2
          for (let i = startColumnNumber; i <= endColumnNumber; i++) {
              for(let j = startRowNumber+2 ;j<= endRowNumber+1 ; j++) {
                            ws[arr[i] + j] = {
                              s: {
                              border: {
                                  top: { style: "thin" },
                                  left: { style: "thin" },
                                  bottom: { style: "thin" },
                                  right: { style: "thin" },
                              },
                              },
                          };
              }
  
          }
        });
        return ws;
      }
  /**
   * 将表转换为最终excel文件的blob对象，并使用URL.createObjectUR下载它
   * @param sheet 表格配置项
   * @param sheetName 表格名称
   * @returns {Blob}
   */
  sheet2blob(sheet, sheetName) {
    sheetName = sheetName || 'sheet1';
    let workbook = {
      SheetNames: [sheetName],
      Sheets: {}
    };
    workbook.Sheets[sheetName] = sheet; // 生成excel的配置项
    
    let wopts = {
      bookType: 'xlsx', // 要生成的文件类型
      bookSST: false, // 是否生成Shared String Table，官方解释是，如果开启生成速度会下降，但在低版本IOS设备上有更好的兼容性
      type: 'binary'
    };
    let wbout = XLSX.write(workbook, wopts);
    let blob = new Blob([s2ab(wbout)], {
      type: "application/octet-stream"
    }); // 字符串转ArrayBuffer
    
    function s2ab(s) {
      let buf = new ArrayBuffer(s.length);
      let view = new Uint8Array(buf);
      for (let i = 0; i != s.length; ++i) view[i] = s.charCodeAt(i) & 0xFF;
      return buf;
    }
    return blob;
  }
  
  /**
   *
   * @param url 生成的文件
   * @param saveName 保存文件名称
   */
  openDownloadDialog(url, saveName) {
    if (typeof url == 'object' && url instanceof Blob) {
      url = URL.createObjectURL(url); // 创建blob地址
    }
    let aLink = document.createElement('a');
    aLink.href = url;
    aLink.download = saveName || ''; // HTML5新增的属性，指定保存文件名，可以不要后缀，注意，file:///模式下不会生效
    let event;
    if (window.MouseEvent) event = new MouseEvent('click');
    else {
      event = document.createEvent('MouseEvents');
      event.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
    }
    aLink.dispatchEvent(event);
  }
}

export default ToXlsx
```

# 修改源码

```
在\node_modules\xlsx-style\dist\cpexcel.js 807行把 var cpt = require('./cpt' + 'able'); 改成 var cpt = cptable;
```

# 页面使用

```
import ToXlsx from "@/utils/xlsx";

//table为element表格id
//this.dataList导出数据（不含表头）
//this.exportProp导出数据的prop绑定值
let xlsx = new ToXlsx('table', 'xxx.xlsx');
xlsx.createCustomizeTable(this.dataList,this.exportProp);
```




