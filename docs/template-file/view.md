## 视图


视图是业务模型内容的最大承载节点，它包含了部件、逻辑等。

其中，视图内容逻辑在模板结构中是分离的。

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        <strong>
        本部分使用实体表格视图（类别文件）作为说明对象，解释视图的构成单位以及构成内容。
        </strong>
    </p>
</blockquote>

视图主要分为以下两个方面来介绍：
- 构成单位
- 构成内容


### 构成单位

[表格视图](http://172.16.180.229/wangxiang1/VUE_R6_FTL/tree/master/@VIEW/%E5%AE%9E%E4%BD%93%E8%A1%A8%E6%A0%BC%E8%A7%86%E5%9B%BE)视图的基本构成单位：
- 视图（逻辑与内容） VIEW.tsx.ftl
- 视图样式 VIEW.less.ftl
- 视图标识 template.properties


#### 视图（逻辑与内容）


##### 视图逻辑

[视图逻辑](http://172.16.180.229/wangxiang1/VUE_R6_FTL/tree/master/@VIEW)则是独立的目录维护，表格视图 [VIEW.tsx.ftl](http://172.16.180.229/wangxiang1/VUE_R6_FTL/tree/master/@VIEW/%E5%AE%9E%E4%BD%93%E8%A1%A8%E6%A0%BC%E8%A7%86%E5%9B%BE) 代码如下：

```freemarker
<#ibizinclude>
../@MACRO/GRID_VIEW.tsx.ftl
</#ibizinclude>
```

`GRID_VIEW.tsx.ftl` 宏文件内容如下：
```freemarker
<#ibizinclude>
./VIEW_HEADER.tsx.ftl
</#ibizinclude>

<#ibizinclude>
./VIEW_CONTENT.tsx.ftl
</#ibizinclude>
<#assign grid></#assign>
<#if view.hasPSControl('grid')>
<#assign grid = view.getPSControl('grid')>
</#if>
<#if grid??>

    /**
     * 是否单选
     *
     * @type {boolean}
     * @memberof ${srfclassname('${view.name}')}
     */
    public isSingleSelect: boolean = ${grid.isSingleSelect()?c};
</#if>

    /**
     * 搜索值
     *
     * @type {string}
     * @memberof ${srfclassname('${view.name}')}
     */
    public query: string = '';

    /**
     * 是否展开搜索表单
     *
     * @type {boolean}
     * @memberof ${srfclassname('${view.name}')}
     */
    public isExpandSearchForm: boolean = ${view.isExpandSearchForm()?c};

    /**
     * 表格行数据默认激活模式
     * 0 不激活
     * 1 单击激活
     * 2 双击激活
     *
     * @type {(number | 0 | 1 | 2)}
     * @memberof ${srfclassname('${view.name}')}
     */
    public gridRowActiveMode: number | 0 | 1 | 2 = ${view.getGridRowActiveMode()?c};

    /**
     * 快速搜索
     *
     * @param {*} $event
     * @memberof ${srfclassname('${view.name}')}
     */
    public onSearch($event: any): void {
        <#if grid??>
        const grid: any = this.$refs.${grid.name};
        if (grid) {
            grid.load({});
        }
        </#if>
    }
<#if grid??>

    /**
     * 刷新数据
     *
     * @readonly
     * @type {(number | null)}
     * @memberof ${srfclassname('${view.name}')}
     */
    get refreshdata(): number | null {
        return this.$store.getters['viewaction/getRefreshData'](this.viewtag);
    }

    /**
     * 监控数据变化
     *
     * @param {*} newVal
     * @param {*} oldVal
     * @returns
     * @memberof ${srfclassname('${view.name}')}
     */
    @Watch('refreshdata')
    onRefreshData(newVal: any, oldVal: any) {
        if (newVal === null || newVal === undefined) {
            return;
        }
        if (newVal === 0) {
            return;
        }
        const grid: any = this.$refs.${grid.name};
        if (grid) {
            grid.load({});
        }
    }
</#if>

<#ibizinclude>
./LAYOUTPANEL_VIEW.tsx.ftl
</#ibizinclude>

<#ibizinclude>
./VIEW_BOTTOM.tsx.ftl
</#ibizinclude>
```

其中 `LAYOUTPANEL_VIEW.tsx.ftl` 主要用于输出视图内容。


##### 视图内容

[视图内容](http://172.16.180.229/wangxiang1/VUE_R6_FTL/tree/master/@CONTROL/%E8%A7%86%E5%9B%BE%E5%B8%83%E5%B1%80%E9%9D%A2%E6%9D%BF)主要在部件的视图布局面板中，该面板包含所有的视图布局。


#### 视图样式

视图样式输出过程如下：

DEFAULT.less.ftl --------> 布局面板实体表格视图样式 --------> 实体表格视图样式

输出[实体表格视图](http://172.16.180.229/wangxiang1/VUE_R6_FTL/blob/master/@VIEW/%E5%AE%9E%E4%BD%93%E8%A1%A8%E6%A0%BC%E8%A7%86%E5%9B%BE/VIEW.less.ftl)样式 `FreeMarker` 代码，它指向布局面板实体表格视图样式。
```freemarker
${P.getLayoutCode().code}
```

[布局面板实体表格视图样式](http://172.16.180.229/wangxiang1/VUE_R6_FTL/blob/master/@CONTROL/%E8%A7%86%E5%9B%BE%E5%B8%83%E5%B1%80%E9%9D%A2%E6%9D%BF/%E5%AE%9E%E4%BD%93%E8%A1%A8%E6%A0%BC%E8%A7%86%E5%9B%BE/VIEW.less.ftl)，引入了宏文件，视图样式扩展，目标是替换该文件。
```freemarker
<#ibizinclude>
../@MACRO/DEFAULT.less.ftl
</#ibizinclude>
```

[DEFAULT.less.ftl](http://172.16.180.229/wangxiang1/VUE_R6_FTL/blob/master/@CONTROL/%E8%A7%86%E5%9B%BE%E5%B8%83%E5%B1%80%E9%9D%A2%E6%9D%BF/@MACRO/DEFAULT.less.ftl) 宏文件内容。
```freemarker
// this is less

<#if view.hasPSControl('toolbar')>
<@ibizindent blank=8>
${P.getCtrlCode('toolbar', 'CONTROL.less').code}
</@ibizindent>
</#if>

<#assign viewCssName = '' />
<#if view.getPSSysCss()??>
<#assign viewCssName = view.getPSSysCss().getCssName()>
</#if>
<#list view.getPSSysCsses() as syscss>
<#if syscss.getCssStyle()??>
<#if syscss.getCssName() != viewCssName>
<#if syscss.getRawCssStyle()?? && syscss.getRawCssStyle()?length gt 0>
.${syscss.getCssName() } {
    ${syscss.getRawCssStyle()}
}
</#if>
<#if syscss.getCssStyle()??>
${syscss.getCssStyle()}
</#if>
</#if>
</#if>
</#list>

<#if view.getPSSysCss()??>
<#assign css=view.getPSSysCss()/>
<#if css.getRawCssStyle()?? && css.getRawCssStyle()?length gt 0>
.${css.getCssName()} {
    ${css.getRawCssStyle()}
}
</#if>
<#if css.getCssStyle()??>
${css.getCssStyle()}
</#if>
</#if>
```


#### 视图标识

```freemarker
VIEWTYPE=APPDEGRIDVIEW
```

`VIEWTYPE` 是视图类型，`APPDEGRIDVIEW` 是视图标识名称，属于 IBizSys 模型预置。

### 构成内容
