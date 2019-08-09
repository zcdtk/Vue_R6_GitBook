## 部件扩展


在解决方案预置部件逻辑与部件内容不能满足业务需求时，就需求对部件进行扩展。

扩展分为以下两个部分：
- 内容扩展
- 逻辑扩展


### 内容扩展

部件内容扩展，主要是扩展部件标签和部件绘制内容，在部件标签上绑定新的参数以及用新的内容替换原始内容。

<blockquote style="border-color: #2892ec;background-color: #f0faff;">
    <p>
        本部分使用应用快捷菜单部件（类别文件）作为说明对象，解释部件的内容扩展。
    </p>
</blockquote>

菜单发布内容如下：

> 由于内容区代码太多，其中默认内容使用 `---  默认内容区  ---` 替换掉。

```freemarker
<#if render_block??>
${render_block}
<#else>
    ---  默认内容区  ---
</#if>
```

`render_block` 是部件内容扩展变量，用于绘制内容。<br>

内容扩展步骤如下：
- 新建部件模板文件
- 新建扩展部件标识
- 新建部件标签内容
- 新建部件绘制内容
- 新建系统应用插件


#### 1. 新建部件模板文件

常规情况下，建立以下三个部分即可：
- 部件标签 CONTROL.html.ftl
- 部件样式 CONTROL.less.ftl
- 部件绘制内容 CONTROL.tsx.ftl
- 部件标识标识 template.properties

如下图所图：

![部件扩展文件](/imgs/plugins-control/plugins-control.png)


#### 2. 新建扩展部件标识

IBizSys 模型预置菜单部件标识如下：

```freemarker
CTRLTYPE=APPMENU
```

`CTRLTYPE` 是部件类型，`APPMENU` 是部件标识名称，属于IBizSys 模型预置。

扩展部件标识内容：

```freemarker
CTRLTYPE=APPMENU#QUICKMENUBAR
```

`CTRLTYPE` 是部件类型，`APPMENU#QUICKMENUBAR` 是部件标识名称，其中QUICKMENUBAR属于用户自定义，在配置平台中使用。

部件级扩展标识，只需要在预置部件标识后添加 `#<部件名称>`，即可使用。


#### 3. 新建部件标签内容
#### 4. 新建部件绘制内容
#### 5. 新建系统应用插件


### 逻辑扩展

逻辑扩展时模板文件直接修改，它属于在原始文件中直接修改内容，处理业务逻辑。

逻辑扩展之后，再次更新将不再同步文件，需要用户独立维护模板文件。