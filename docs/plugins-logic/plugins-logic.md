## 逻辑扩展

IBizSys 模型预置逻辑处理可配置预置业务逻辑，当预置业务逻辑不足以承担所有业务功能，就需要对逻辑功能做除扩展。

扩展分为以下两个部分：
- 内容扩展
- 逻辑扩展

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
    本部分使用界面行为前台调用作为说明对象，解释逻辑扩展。
    </p>
</blockquote>

内容输出模板如下：

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        其中默认内容使用 ---  内容区  --- 替换掉。
    </p>
</blockquote>

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

```freemarker
<#assign render_block>
<#--  前台界面行为  -->
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
            <!---  扩展内容  --->    
        });
    }
</#assign>

```