## 部件扩展


在解决方案预置部件逻辑与部件内容不能满足业务需求时，就需求对部件进行扩展。

扩展分为以下两个部分：
- 内容扩展
- 逻辑扩展

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
    本部分使用实体表格部件（类别文件）作为说明对象，解释部件的内容扩展与逻辑扩展。
    </p>
</blockquote>


### 内容扩展

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

`render_block` 是部件内容扩展变量，用于绘制内容。<br>

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


### 逻辑扩展

逻辑扩展时模板文件直接修改，它属于在原始文件中直接修改内容，处理业务逻辑。

逻辑扩展之后，再次更新将不再同步文件，需要用户独立维护模板文件。