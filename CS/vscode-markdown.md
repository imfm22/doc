# 扩展文件配置选项

> vscode, extentions

[toc]

# &sect; Markdown

## &para; Markdown preview

### 1. Windows 上的中文字体显示

在 `Extentions` &rarr; `Markdown Preview Enhanced` &rarr; `Preview Theme` 中可以选择主题 css 文件

> Markdown preview 的字体设置在 css 文件中

本机 css 文件夹为 `C:\Users\liuja\.vscode\extensions\shd101wyy.markdown-preview-enhanced-0.4.3\node_modules\@shd101wyy\mume\styles\preview_theme`

ps. 其他系统可能不同

Windows 上的默认中文字体为有衬线体，此处修改为 `Sarasa Term SC`

```css
font-family:'Sarasa Term SC',"Helvetica Neue",Helvetica,"Segoe UI",Arial,freesans,sans-serif;
```