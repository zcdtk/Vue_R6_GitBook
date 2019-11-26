# 编辑器

编辑器是应用原始数据的基本单位，用于数据的输入与展示。

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        <strong>
        本部分使用下拉列表框编辑器（类别文件）、文本框编辑器（类别文件）作为说明对象，解释编辑器的构成单位以及构成内容。
        </strong>
    </p>
</blockquote>


## 构成单位

编辑器的构成单位分为以下 4 个部分文件：
- 编辑器样式 EDITOR.less.ftl
- 默认编辑器标签 EDITOR.tsx.ftl
- 行编辑标签 GRIDEIDTOR.tsx.ftl 
- 面板编辑标签 PANELEDITOR.tsx.ftl
- 编辑器标识 template.properties 


### 编辑器样式

编辑器样式用于丰富编辑器内容。


### 默认编辑器标签

默认编辑器标签，一般为表单、搜索表单的组成单位。


### 行编辑标签

行编辑标签，一般为多数据部件的组成单位。


### 面板编辑标签

面板编辑标签，一般为面板部件的组成单位。


### 编辑器标识

```freemarker
EDITORTYPE=******
```

`EDITORTYPE` 是编辑器类型，`******` 是编辑器标识名称，属于 IBizSys 模型预置。


## 构成内容

不同的编辑器使用场景，发布不同的标签。


## 示例


### 下拉列表框

下拉列表框编辑器使用了 [iView](https://www.iviewui.com) 提供 Select 选择器。

此处介绍默认编辑器标签和编辑器标识。

默认编辑器标签：

```freemarker
<dropdown-list 
	v-model={this.data.${item.name}}  
	disabled={this.detailsModel.${item.name}.disabled}  
	<#if item.getPSCodeList()??>
    <#assign codelist=item.getPSCodeList()>tag='${codelist.getSystemTag()}_${codelist.codeName}'
    </#if> 
    placeholder=<#if item.getPlaceHolder()??>'${item.getPlaceHolder()}'<#else>'请选择...'</#if> 
    style="${item.getEditorCssStyle()}">
</dropdown-list>
```

编辑器标识：
```freemarker
EDITORTYPE=DROPDOWNLIST
```


### 文本框


默认编辑器标签：
```freemarker
<input-box 
	v-model={this.data.${item.name}}  
	on-enter={($event: any) => this.onEnter($event)} 
	disabled={this.detailsModel.${item.name}.disabled} 
	type='<#if item.getPSDEField()??><#assign datatype=srfjavatype(item.getPSDEField().getStdDataType())><#if datatype=='BigInteger' || datatype=='Integer' || datatype=='Double'>number<#else>string</#if><#else>string</#if>'  	<#if item.getPlaceHolder()??>
	placeholder={'${item.getPlaceHolder()}'}
	</#if> 
	style="${item.getEditorCssStyle()}">
</input-box>
```

编辑器标识：
```freemarker
EDITORTYPE=TEXTBOX
```

