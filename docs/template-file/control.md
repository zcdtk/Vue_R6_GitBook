# 部件

部件是应用的数据承载单位，处理处理交互逻辑、绘制内容等。

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        <strong>
        本部分使用实体表格部件（类别文件）作为说明对象，解释部件的构成单位以及构成内容。
        </strong>
    </p>
</blockquote>


## 构成单位

表格的构成单位分为以下 5 个部分文件：

- 部件标签 CONTROL.html.ftl 
- 部件样式 CONTROL.less.ftl 
- 部件（逻辑与内容）CONTROL.tsx.ftl 
- 部件成员 CONTROL.tsx#COLUMN.ftl 
- 部件标识 template.properties 


### 部件标签

CONTROL.html.ftl 代码

```freemarker
<#ibizinclude>
../@MACRO/HTML/GRID.html.ftl
</#ibizinclude>
```

改标签引入了宏文件，经过组装后的标签内容如下

```freemarker
<#-- ctrl document  -->
<view_${ctrl.getName()} 
    viewState={this.viewState} 
    isSingleSelect={this.isSingleSelect} 
    showBusyIndicator={${ctrl.isShowBusyIndicator()?c}} 
    searchAction='search<#if handler.getTempMode() gt 0>temp</#if>${handler.getPSDEDataSet().getCodeName()?lower_case}' 
    <#if handler.getPSAjaxHandlerActions()??>
    <#list handler.getPSAjaxHandlerActions() as action>
    <#if action.getPSDEAction()??>
    ${action.name?lower_case}Action='${action.getPSDEAction().getCodeName()?lower_case}' 
    </#if> 
    </#list>
    </#if>
    name='${ctrl.name}' 
    ref='${ctrl.name}' 
    <#if ctrl.getHookEventNames()??>
    <#list ctrl.getHookEventNames() as eventName>
    on-${eventName?lower_case}={($event: any) => this.${ctrl.name}_${eventName?lower_case}($event)} 
    </#list>
    </#if>
    on-closeview={($event: any) => this.closeView($event)}>
</view_${ctrl.getName()}>
```
该标签由 `FreeMarker` 引擎处理后，结果如下

```html
<view_grid 
    viewState={this.viewState} 
    isSingleSelect={this.isSingleSelect} 
    showBusyIndicator={true} 
    searchAction='searchdefault' 
    updateAction='update' 
    removeAction='remove' 
    loadAction='get' 
    loaddraftAction='getdraft' 
    createAction='create' 
    name='grid' 
    ref='grid' 
    on-selectionchange={($event: any) => this.grid_selectionchange($event)} 
    on-beforeload={($event: any) => this.grid_beforeload($event)} 
    on-rowdblclick={($event: any) => this.grid_rowdblclick($event)} 
    on-remove={($event: any) => this.grid_remove($event)} 
    on-load={($event: any) => this.grid_load($event)} 
    on-closeview={($event: any) => this.closeView($event)}>
</view_grid>
```

一个部件标签组装和被引用后发布结果，如上所示。


### 部件样式

CONTROL.less.ftl 代码

```freemarker
.grid {
    flex-grow: 1;
    height: 100%;
    overflow: auto;
    .grid-pagination {
        height: 50px;
        padding: 6px 0px;
        .page-button {
            button  {
                padding: 0;
                font-size: 16px;
                min-width: 32px;
                height: 32px;
                margin-right: 4px;
            }
        }
        .page-column {
            position: absolute;
            left: 0;
        }
    }
}

<#ibizinclude>
../@MACRO/CSS/DEFAULT.less.ftl
</#ibizinclude>
```

样式代码添加了部件自定义样式优先级使用，其后一部分是默认样式逻辑，包括系统样式表等一系列样式。


### 部件（逻辑与内容）

CONTROL.tsx.ftl该部分包含逻辑与内容
- 逻辑主要包括变量声明、数据交互等，逻辑内容太多，此处不展开
- 内容主要指绘制内容（render）

以下为内容

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        其中默认内容使用 ---  内容区  --- 替换掉。
    </p>
</blockquote>

```freemarker
<#if render_block??>
${render_block}
<#else>
    ---  内容区  ---
</#if>
```

`render_block` 是部件内容扩展，用于绘制内容。

发开人员使用部件扩展时，只需要在扩展的部件内定义如下，即可完成的替换默认内容。

```freemarker
<#assign render_block>
public render() {
    return (
        <div>这是扩展内容</div>
    );
}
</#assign>

<#ibizinclude>
../表格/CONTROL.tsx.ftl
</#ibizinclude>

```

完整的扩展，还有很多其他的内容支持，此处只做 `front_block` 简单说明。

### 部件成员

CONTROL.tsx#COLUMN.ftl 在 `CONTROL.tsx` 后以 `#` 开始命令的文件，属于部件成员，该出演的类型在 IBizSys 模型中已经预置，可以直接声明使用。

成员是部件逻辑或者部件内容的组成单元，通过获取部件成员模型，可以输出对应的部件逻辑或者内容。

### 部件标识

template.properties

```freemarker
CTRLTYPE=GRID
```
`CTRLTYPE` 是部件类型，`GRID` 是部件标识名称，属于 IBizSys 模型预置。


## 构成内容

部件构成，部件成员组装部件（逻辑与内容），部件标签被视图引用发布，部件（逻辑与内容）负责数据交互，部件样式丰富部件内容表现。