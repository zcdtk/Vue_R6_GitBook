## 编辑器扩展


* [扩展编辑器步骤](#扩展编辑器步骤)
    1. [新建编辑器模板文件](#1-新建编辑器模板文件)
    2. [新建扩展编辑器标识](#2-新建扩展编辑器标识)
    3. [新建编辑器默认内容](#3-新建编辑器默认内容)
    4. [新建系统应用插件](#4-新建系统应用插件)
* [扩展编辑器使用](#扩展编辑器使用)


编辑器扩展，在表单等需要特定展示数据和处理数据的部件中，经常被使用。


### 扩展编辑器步骤


#### 1. 新建编辑器模板文件

常规情况下，建立以下三个部分即可：
- 编辑器样式 EDITOR.less.ftl
- 默认编辑器内容 EDITOR.tsx.ftl
- 编辑器标识 template.properties

如下图所图：

![编辑器文件](/imgs/plugins-editor/editor-files.png)



#### 2. 新建扩展编辑器标识

扩展标识内容：

```freemarker
EDITORTYPE=pluginseditor
```
`EDITORTYPE` 是编辑器类型，`pluginseditor` 是编辑器标识名称，属于用户自定义，在配置平台中使用。


#### 3. 新建编辑器默认内容

```freemarker
<plugins-editor 
    name='${item.name}' 
    v-model={this.data.${item.name}} 
    formState={this.formState} 
    data={this.data} 
    disabled={this.detailsModel.${item.name}.disabled} 
    style='${item.getEditorCssStyle()}'   
    on-formitemvaluechange={this.onFormItemValueChange}>
</plugins-editor>
```


#### 4. 新建系统应用插件

如下图所示：

![系统应用插件](/imgs/plugins-editor/plugins-editor.png)

其中插件类型必须是编辑器自定义绘制插件，插件标识必须与扩展编辑器标识一致。



### 扩展编辑器使用


*此处不做扩展编辑器使用介绍，请参考其他文档。*


假设编辑属性代码名称为  `ordername`，引用后的扩展编辑器经过 `FreeMarker` 引擎处理后，代码如下：
```html
<plugins-editor 
    name='ordername' 
    v-model={this.data.ordername}} 
    formState={this.formState} 
    data={this.data} 
    disabled={this.detailsModel.ordername.disabled} 
    style='width: 100px;'   
    on-formitemvaluechange={this.onFormItemValueChange}>
</plugins-editor>
```

绑定属性简介：

`name` <br>
编辑器名称。

`v-model`<br>
值双向绑定。

`formState`<br>
表单状态。

编辑器内使用方式如下：
```javascript
@Prop() public formState?: Subject<any>

private formStateEvent: Unsubscribable | undefined;

public created(): void {
    this.formStateEvent = this.formState.subscribe(($event: any) => {
        // 表单加载完成
        if (Object.is($event.type, 'load')) {
        }
        // 表单保存完成
        if (Object.is($event.type, 'save')) {
        }
        // 表单项更新
        if (Object.is($event.type, 'updateformitem')) {
        }
    });
}

public destroyed(): void {
    if (this.formStateEvent) {
        this.formStateEvent.unsubscribe();
    }
}
```
`data` <br>
表单数据。


`disabled` <br>
编辑器是否启用

`style` <br>
编辑器样式。

`on-formitemvaluechange` <br>
编辑器值回填表单。

编辑器内使用方式如下：
```javascript
this.$emit('formitemvaluechange', { name: 'ordername', value: '123123123132132132' });
```