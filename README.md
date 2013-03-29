XSLX
====
xlsx is intended to be a library to simplify reading the XML format
used by recent version of Microsoft Excel in Go programs.

[https://github.com/tealeg/xlsx2csv/blob/master/main.go](https://github.com/tealeg/xlsx2csv/blob/master/main.go)

具体的例子可以参考此程序！

Currently it is in the very early stages and development and only does very basic reading.  It should progress rapidly, please feel free to help out.

There are no current plans to support writing documents.


License
-------
This code is under a BSD style license:


Copyright 1992-2011 The Geoffrey Teale. All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
THIS SOFTWARE IS PROVIDED BY THE FREEBSD PROJECT ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE FREEBSD PROJECT OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.




Eat a peach - Geoff

我测试的一个例子：
  // 读取 excel 2007 实例说明
	package main
	
	import (
		"fmt"
		"github.com/tealeg/xlsx" //需要引入的包
	)
	
	func main() {
		var xlFile *xlsx.File
		var sheetLen int
	
		xlFile, _ = xlsx.OpenFile("Mytest01.xlsx")
		//defer xlFile.CloseFile
	
		// 按 sheet 的顺序编号从 0 开始处理到最后 sheet
		sheetLen = len(xlFile.Sheets)
		for i := 0; i < sheetLen; i++ { // 第一种方式
			sheet := xlFile.Sheets[i]
			fmt.Println("现在是 sheet:", i)
			for rowIndex, row := range sheet.Rows {
				for cellIndex, cell := range row.Cells {
					fmt.Printf("第%d行，第%d列：%v\n", rowIndex, cellIndex, cell)
				}
			}
		}
	
		for i, _ := range xlFile.Sheets { // 第二种方式, 比较好
			sheet := xlFile.Sheets[i]
			fmt.Println("现在是 sheet:", i)
			for rowIndex, row := range sheet.Rows {
				for cellIndex, cell := range row.Cells {
					fmt.Printf("第%d行，第%d列：%v\n", rowIndex, cellIndex, cell)
				}
			}
		}
	
		// 按 sheet 的名字进行处理, 顺序是随机的不确定
		for shname, _ := range xlFile.Sheet {
			sheet := xlFile.Sheet[shname]
			fmt.Println("现在是 sheet:", shname)
			for rowIndex, row := range sheet.Rows {
				for cellIndex, cell := range row.Cells {
					fmt.Printf("第%d行，第%d列：%v\n", rowIndex, cellIndex, cell)
				}
			}
		}
	
		// 打出各种结构看看
		fmt.Printf("文件结构:%#v\n\n", xlFile)
		fmt.Printf("Sheet 结构:%#v\n\n", xlFile.Sheet)   // sheet 名称放在 map 中
		fmt.Printf("Sheets 结构:%#v\n\n", xlFile.Sheets) // 按sheet 的顺序号从零开始
	}

         2013.03.28 测试通过。
