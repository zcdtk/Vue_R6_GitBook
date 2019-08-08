## 编辑器扩展


编辑器扩展，在表单等需要特定展示数据和处理数据的部件中，经常被使用。


### 扩展编辑器步骤


#### 1.模板文件新建编辑器文件

常规情况下，建立以下三个部分即可：
- 编辑器样式 EDITOR.less.ftl
- 默认编辑器内容 EDITOR.tsx.ftl
- 编辑器标识 template.properties


#### 2.新建扩展编辑器标识

```freemarker
EDITORTYPE=pluginseditor
```
`EDITORTYPE` 是部件类型，`pluginseditor` 是编辑器标识名称，属于用户自定义，在配置平台中使用。


#### 3.新建编辑器默认内容

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

假设编辑属性代码名称为  `ordername`，经过 `FreeMarker` 引擎处理后，代码如下：
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

