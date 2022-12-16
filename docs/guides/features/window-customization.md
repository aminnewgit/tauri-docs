# 窗口自定义 
Window Customization

Tauri provides lots of options for customizing the look and feel of your app's window. You can create custom titlebars, 
have transparent windows, enforce size constraints, and more. 

Tauri提供了许多自定义应用程序窗口外观的选项。您可以创建自定义标题栏、透明窗口、强制大小限制等。

## Configuration 配置

There are three ways to change the window configuration:  
有三种方法可以更改窗口配置：

- [Through tauri.conf.json](../../api/config.md#tauri.windows)
- [Through the JS API](../../api/js/window.md#webviewwindow)
- [Through the Window in Rust](https://docs.rs/tauri/1/tauri/window/struct.Window.html)


一些注意事项 
1. 关闭默认标题栏。  

   >tauri.conf.json->tauri->windows->decorations: false  
2. 添加jsApi的allowList 最简单的是`all:true`， 安全期间可以单独添加权限。调用web调用tauri的api需要添加allowlist。
   >   tauri.conf.json->tauri->allowlist->all: true


## 创建自定义标题栏
Creating a Custom Titlebar

A common use of these window features is creating a custom titlebar. 
This short tutorial will guide you through that process.
这些窗口功能的常见用法是创建自定义标题栏。这个简短的教程将指导您完成这个过程。

### CSS

You'll need to add some CSS for the titlebar to keep it at the top of the screen and style the buttons:  
您需要为标题栏添加一些CSS，使其保持在屏幕顶部并设置按钮样式：

```css
.titlebar {
  height: 30px;
  background: #329ea3;
  user-select: none;
  display: flex;
  justify-content: flex-end;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}
.titlebar-button {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: 30px;
  height: 30px;
}
.titlebar-button:hover {
  background: #5bbec3;
}
```

### HTML

Now, you'll need to add the HTML for the titlebar. Put this at the top of your `<body>` tag:  
现在，您需要为标题栏添加HTML。将其放在`<body>`标签的顶部：

```html
<div data-tauri-drag-region class="titlebar">
  <div class="titlebar-button" id="titlebar-minimize">
    <img
      src="https://api.iconify.design/mdi:window-minimize.svg"
      alt="minimize"
    />
  </div>
  <div class="titlebar-button" id="titlebar-maximize">
    <img
      src="https://api.iconify.design/mdi:window-maximize.svg"
      alt="maximize"
    />
  </div>
  <div class="titlebar-button" id="titlebar-close">
    <img src="https://api.iconify.design/mdi:close.svg" alt="close" />
  </div>
</div>
```

Note that you may need to move the rest of your content down so that the titlebar doesn't cover it.  
请注意，您可能需要将其余内容向下移动，以便标题栏不会覆盖它。

### JS

Finally, you'll need to make the buttons work:
最后，您需要使按钮工作：

```js
import { appWindow } from '@tauri-apps/api/window'
document
  .getElementById('titlebar-minimize')
  .addEventListener('click', () => appWindow.minimize())
document
  .getElementById('titlebar-maximize')
  .addEventListener('click', () => appWindow.toggleMaximize())
document
  .getElementById('titlebar-close')
  .addEventListener('click', () => appWindow.close())
```
