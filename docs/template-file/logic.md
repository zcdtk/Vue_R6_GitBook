# 简介


逻辑主要作用是构建应用的交互操作，从事件触发，到数据请求、页面展示，都属于逻辑的一部分。

IBizSys 模型预置逻辑丰富了应用的交互操作，通过配置化导向，解析业务模型，发布业务处理过程。

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        <strong>
        本部分使用界面行为前台调用（类别文件）作为说明对象，解释逻辑的构成单位以及构成内容。
        </strong>
    </p>
</blockquote>


# 构成单位

前台调用的构成单位分为以下 2 个部分文件：
- 前台调用逻辑内容 LOGIC.tsx.ftl
- 前台调用逻辑标识 template.properties 


## 前台调用逻辑内容

前台调用逻辑内容如下：

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        其中默认内容使用 ------内容区------- 替换掉。
    </p>
</blockquote>

```freemarker
<#--  前台界面行为  -->
<#if front_block??>
${front_block}
<#else>
    ------内容区-------
</#if>
```

由于默认内容过多，此不做展开，查看详情，点击 LOGIC.tsx.ftl 访问

此处将选取部分要点做讲解：
- 扩展变量说明
- 支持界面行为数据类型和处理模式
- 预置参数处理
- 其他关联逻辑


### 扩展变量说明

`front_block` 是界面行为内容扩展，用于绘制内容。

发开人员使用界面行为扩展时，只需要在界面行为内定义如下，即可完成的替换默认内容。

```freemarker
<#assign front_block>
public ${item.getFullCodeName()}{
    return (
        <div>这是扩展内容</div>
    );
}
</#assign>

<#ibizinclude>
../前台调用/LOGIC.tsx.ftl
</#ibizinclude>
```

完整的扩展，还有很多其他的内容支持，此处只做 `front_block` 简单说明。


### 支持界面行为数据类型和处理模式

前台界面行为支持单项数据主键、多项数据主键和无数据，其他数据类型不支持，开发人员选择其他数据类型后，触发界面行为会有报错提示。

```freemarker
<#if item.getActionTarget() == 'SINGLEDATA'>
        return new Promise((resolve: any, reject: any) => {
            this.$Notice.error({ title: '错误', desc: '不支持单项数据' });
        });
<#elseif item.getActionTarget() == 'MULTIDATA'>
        return new Promise((resolve: any, reject: any) => {
            this.$Notice.error({ title: '错误', desc: '不支持多项数据' });
        });
<#else>
```

其中 `SINGLEDATA` 是单项数据，`MULTIDATA` 是多项数据。

处理模式支持以下模式：
- 打开视图或向导（模态）
- 打开顶级视图 
- 打开html页面
- 用户自定义

#### 打开html页面  

条件：`item.getFrontProcessType() == 'OPENHTMLPAGE'`

```freemarker
<#--  打开独立程序弹出  -->
return new Promise((resolve, reject) => {
    const openPopupApp = (url: string) => {
        window.open(url, '_blank');
        resolve();
    }
    const localdata: any = this.$store.getters.getLocalData();
    const url = `${item.getHtmlPageUrl()}`;
    openPopupApp(url);
});
```

#### 用户自定义

条件： `item.getFrontProcessType() == 'OTHER'`

```freemarker
// 自定义实体界面行为
this.$Notice.warning({ title: '错误', desc: '${item.getCaption()} 未实现' });
return new Promise((resolve: any, reject: any) => {
    
});
```

#### 打开视图或向导（模态）和打开顶级视图

条件：`item.getFrontProcessType() == 'TOP' || item.getFrontProcessType() == 'WIZARD'` 

两者打开视图，是根据视图的打开方式来支持不同的打开逻辑处理。

#### 打开顶级分页视图

```freemarker
<#elseif frontview.getOpenMode() =='INDEXVIEWTAB' || frontview.getOpenMode() == ''>
<#--  打开顶级分页视图  -->
// 所有数据保持在同一级
if (data.srfparentdata) {
    Object.assign(data, data.srfparentdata);
    delete data.srfparentdata;
}
const openIndexViewTab = (viewpath: string, data: any) => {
    const _params = this.$util.prepareRouteParmas({
        route: this.$route,
        sourceNode: this.$route.name,
        targetNode: viewpath,
        data: data,
    });
    this.$router.push({ name: viewpath, params: _params });
    resolve();
}
openIndexViewTab('${frontview.getPSAppModule().codeName?lower_case}_${frontview.codeName?lower_case}', data);
```

#### 打开模态

```freemarker
<#elseif frontview.getOpenMode() = 'POPUPMODAL'>
<#--  打开模态  -->
const openPopupModal = (view: any, data: any) => {
    let container: Subject<any> = this.$appmodal.openModal(view, data);
    container.subscribe((result: any) => {
        if (!result || !Object.is(result.ret, 'OK')) {
            return;
        }
        const _this: any = this;
        <#--  是否重新加载数据  -->
        <#if item.isReloadData?? && item.isReloadData()>
        if (xData && xData.refresh && xData.refresh instanceof Function) {
            xData.refresh(args);
        } else if (_this.refresh && _this.refresh instanceof Function) {
            _this.refresh(args);
        }
        </#if>
        <#--  后续界面行为  -->
        <#if item.getNextPSUIAction?? && item.getNextPSUIAction()??>
        <#assign nextPSUIAction = item.getNextPSUIAction()/>
        if (_this.${nextPSUIAction.getFullCodeName()} && _this.${nextPSUIAction.getFullCodeName()} instanceof Function) {
            _this.${nextPSUIAction.getFullCodeName()}(result.datas, params, $event, xData);
        }
        </#if>
        resolve(result.datas);
    });
}
const view: any = {
    viewname: '${srffilepath2(frontview.getCodeName())}', 
    height: ${frontview.getHeight()?c}, 
    width: ${frontview.getWidth()?c},  
    title: '${frontview.title}', 
};
openPopupModal(view, data);
```

#### 打开抽屉 

```freemarker
<#elseif frontview.getOpenMode()?index_of('DRAWER') == 0>
<#--  打开抽屉  -->
const openDrawer = (view: any, data: any) => {
    let container: Subject<any> = this.$appdrawer.openDrawer(view, data);
    container.subscribe((result: any) => {
        if (!result || !Object.is(result.ret, 'OK')) {
            return;
        }
        const _this: any = this;
        <#--  是否重新加载数据  -->
        <#if item.isReloadData?? && item.isReloadData()>
        if (xData && xData.refresh && xData.refresh instanceof Function) {
            xData.refresh(args);
        } else if (_this.refresh && _this.refresh instanceof Function) {
            _this.refresh(args);
        }
        </#if>
        <#--  后续界面行为  -->
        <#if item.getNextPSUIAction?? && item.getNextPSUIAction()??>
        <#assign nextPSUIAction = item.getNextPSUIAction()/>
        if (_this.${nextPSUIAction.getFullCodeName()} && _this.${nextPSUIAction.getFullCodeName()} instanceof Function) {
            _this.${nextPSUIAction.getFullCodeName()}(result.datas, params, $event, xData);
        }
        </#if>
        resolve(result.datas);
    });
}
const view: any = {
    viewname: '${srffilepath2(frontview.getCodeName())}', 
    height: ${frontview.getHeight()?c}, 
    width: ${frontview.getWidth()?c},  
    title: '${frontview.title}', 
    placement: '${frontview.getOpenMode()}',
};
openDrawer(view, data);
```

#### 打开气泡卡片

```freemarker
<#elseif frontview.getOpenMode() == 'POPOVER'>
<#--  打开气泡卡片  -->
const openPopOver = (view: any, data: any) => {
    let container: Subject<any> = this.$apppopover.openPop($event, view, data);
    container.subscribe((result: any) => {
        if (!result || !Object.is(result.ret, 'OK')) {
            return;
        }
        const _this: any = this;
        <#--  是否重新加载数据  -->
        <#if item.isReloadData?? && item.isReloadData()>
        if (xData && xData.refresh && xData.refresh instanceof Function) {
            xData.refresh(args);
        } else if (_this.refresh && _this.refresh instanceof Function) {
            _this.refresh(args);
        }
        </#if>
        <#--  后续界面行为  -->
        <#if item.getNextPSUIAction?? && item.getNextPSUIAction()??>
        <#assign nextPSUIAction = item.getNextPSUIAction()/>
        if (_this.${nextPSUIAction.getFullCodeName()} && _this.${nextPSUIAction.getFullCodeName()} instanceof Function) {
            _this.${nextPSUIAction.getFullCodeName()}(result.datas, params, $event, xData);
        }
        </#if>
        resolve(result.datas);
    });
}
const view: any = {
    viewname: '${srffilepath2(frontview.getCodeName())}', 
    height: ${frontview.getHeight()?c}, 
    width: ${frontview.getWidth()?c},  
    title: '${frontview.title}', 
    placement: '${frontview.getOpenMode()}',
};
openPopOver(view, data);
```


### 预置参数处理

界面行为数据变量声明
```javascript
const data: any = { srfparentdata: {} };
```
预置参数处理，主要包括
- 父数据处理
- 单项数据主键处理 
- 多项数据主键处理
- 界面行为附加参数处理

#### 父数据处理

```javascript
const _this: any = this;
if (_this.srfparentdata) {
    Object.assign(data.srfparentdata, _this.srfparentdata);
}
```

#### 单项数据主键处理

```freemarker
<#if item.getActionTarget() == 'SINGLEKEY'>

    <#--  单项数据主键  -->
    if (args.length > 0) {
        Object.assign(data, { srfkey: args[0].srfkey });
    }

    // 值项名称 - 单项数据主键，默认为srfkey
    const valueItem: string = '<#if item.getValueItem?? && item.getValueItem() != ''>${item.getValueItem()}<#else>srfkey</#if>';
    // 数据项名称 - 单项数据主键，默认为srfkey
    const paramItem: string = '<#if item.getParamItem?? && item.getParamItem() != ''>${item.getParamItem()}<#else>srfkey</#if>';
    if (args.length > 0 && args[0].hasOwnProperty(valueItem)) {
        const _params2: any = {};
        Object.assign(_params2, { [paramItem]: args[0][valueItem] });
        // 参数层级处理
        if (Object.is(paramItem, 'srfkey')) {
            Object.assign(data, _params2);
        } else {
            Object.assign(data.srfparentdata, _params2);
        }
        // 非默认值转换 删除原始值
        if (Object.is(valueItem, 'srfkey') && !Object.is(paramItem, 'srfkey')) {
            delete data.srfkey;
        }
    }
</#if>
```

#### 多项数据主键处理

```freemarker
<#if item.getActionTarget() == 'MULTIKEY'>
    
<#--  多项数据主键  -->
let keys: string[] = [];
args.forEach((arg: any) => {
    if (arg.srfkey) {
        keys.push(arg.srfkey);
    }
});
if (keys.length > 0) {
    Object.assign(data, { srfkeys: keys.join(';') });
}

// 值项名称 -  多项数据主键，默认为srfkey
const valueItem: string = '<#if item.getValueItem?? && item.getValueItem() != ''>${item.getValueItem()}<#else>srfkey</#if>';
// 数据项名称 - 多项数据主键，默认为srfkeys
const paramItem: string = '<#if item.getParamItem?? && item.getParamItem() != ''>${item.getParamItem()}<#else>srfkeys</#if>';
const _params2: any = {};
let _keys: string[] = [];
args.forEach((arg: any) => {
    if (arg.hasOwnProperty(valueItem)) {
        _keys.push(arg[valueItem]);
    }
});

Object.assign(_params2, { [paramItem]: _keys.join(';') });
// 参数层级处理
if (Object.is(paramItem, 'srfkeys')) {
    Object.assign(data, _params2);
} else {
    Object.assign(data.srfparentdata, _params2);
}
</#if>
```

#### 界面行为附加参数处理

```javascript
if (params && Object.keys(params).length > 0) {
    const _params: any = {};
    const arg: any = args[0];
    Object.keys(params).forEach((name: string) => {
        if (!name) {
            return;
        }
        let value: string | null = params[name];
        if (value && value.startsWith('%') && value.endsWith('%')) {
            const key = value.substring(1, value.length - 1);
            if (arg && arg.hasOwnProperty(key)) {
                value = (arg[key] !== null && arg[key] !== undefined) ? arg[key] : null;
            } else {
                value = null;
            }
        }
        Object.assign(_params, { [name]: value });
    });
    Object.assign(data.srfparentdata, _params);
}
```


### 其他关联逻辑

前台调用界面行为关联逻辑主要包含两个部分：
- 是否重新加载数据
- 后续界面行为

#### 是否重新加载数据

```freemarker
<#--  是否重新加载数据  -->
<#if item.isReloadData?? && item.isReloadData()>
if (xData && xData.refresh && xData.refresh instanceof Function) {
    xData.refresh(args);
} else if (_this.refresh && _this.refresh instanceof Function) {
    _this.refresh(args);
}
```

#### 后续界面行为

```freemarker
<#--  后续界面行为  -->
<#if item.getNextPSUIAction?? && item.getNextPSUIAction()??>
<#assign nextPSUIAction = item.getNextPSUIAction()/>
if (_this.${nextPSUIAction.getFullCodeName()} && _this.${nextPSUIAction.getFullCodeName()} instanceof Function) {
    _this.${nextPSUIAction.getFullCodeName()}(result.datas, params, $event, xData);
}
</#if>
```


## 前台调用逻辑标识

```freemarker
LOGICTYPE=FRONT
```

`LOGICTYPE` 是逻辑类型，`FRONT` 是逻辑标识名称，属于 IBizSys 模型预置。


# 构成内容

解析业务逻辑通过 `FreeMarker` 引擎发布业务逻辑内容