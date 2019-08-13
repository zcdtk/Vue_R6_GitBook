## 逻辑扩展


* [内容扩展](#内容扩展)
    1. [新建扩展界面行为模板文件](#1-新建扩展界面行为模板文件)
    2. [新建扩展界面行为标识](#2-新建扩展界面行为标识)
    3. [新建扩展界面行为内容](#3-新建扩展界面行为内容)
    4. [新建系统应用插件](#4-新建系统应用插件)


IBizSys 模型预置逻辑处理可配置预置业务逻辑，当预置业务逻辑不足以承担所有业务功能，就需要对逻辑功能做除扩展。


### 内容扩展
<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
    本部分使用界面行为前台调用作为说明对象，解释逻辑扩展。
    </p>
</blockquote>

内容输出模板如下：

> 其中默认内容使用 ---  内容区  --- 替换掉。

```
<#--  前台界面行为  -->
<#if front_block??>
${front_block}
<#else>
    ---  内容区  ---
</#if>
    
```

`render_block` 是部件内容扩展变量，用于绘制内容。<br>

发开人员使用部件扩展时，只需要在扩展的部件内定义如下，即可完成的替换默认内容。

查看更多详情，[点击](http://172.16.180.229/wangxiang1/VUE_R6_FTL/blob/master/@LOGIC/@UIACTION/%E5%89%8D%E5%8F%B0%E8%B0%83%E7%94%A8/LOGIC.tsx.ftl)


内容扩展步骤如下：
- 新建扩展界面行为模板文件
- 新建扩展界面行为标识
- 新建扩展界面行为内容
- 新建系统应用插件


#### 1. 新建扩展界面行为模板文件

常规情况下，建立以下两个个部分即可：
- 部件绘制内容 LOGIC.tsx.ftl
- 部件标识标识 template.properties

如下图所图：

![部件扩展文件](/imgs/plugins-logic/logic-files.png)


#### 2. 新建扩展界面行为标识

IBizSys 模型预置前台界面行为标识如下：

```freemarker
LOGICTYPE=FRONT
```

`LOGICTYPE` 是逻辑类型，`FRONT` 是逻辑标识名称，属于IBizSys 模型预置。

扩展部件标识内容：

```freemarker
LOGICTYPE=pluginsfront
```

`LOGICTYPE` 是逻辑类型，`pluginsfront` 是逻辑标识名称，属于用户自定义，在配置平台中使用。


#### 3. 新建扩展界面行为内容

扩展内容如下:

```freemarker
<<#assign front_block>
    /**
     * ${item.getCaption()}
     *
     * @param {any[]} arg
     * @param {*} [params]
     * @returns {Promise<any>}
     * @memberof ${srfclassname('${view.name}')}
     */
    public ${item.getFullCodeName()}(args: any[], params?: any, $event?: any, xData?: any): Promise<any> {
        return new Promise((resolve: any, reject: any) => {
            <#--  扩展内容：开始  -->

            console.log('这部分是扩展内容输出！');

            <#--  扩展内容：结束  -->
        });
    }
</#assign>
```


#### 4. 新建系统应用插件

如下图所示：

![系统应用插件](/imgs/plugins-logic/plugins-logic.png)

其中插件标识必须与扩展部件标识一致。