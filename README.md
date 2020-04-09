# Case-Change plugin for _Tinymce WYSIWYG Editor_

![preview](/Toolbar-menuButton-preview.png)

To use this plugin copy the folder "case" and paste it into the "plugins" folder in tinymce directory.
Here's the path for tinymce_5.2.0 self-hosted production release -> tinymce_5.2.0/tinymce/js/tinymce/plugins

Download any of tinymce self-hosted releases [here](https://www.tiny.cloud/get-tiny/self-hosted/).

## Tutorial
Add the dropdown case-change button to your _tinymce WYSIWYG editor_ by simply including **"case"** in your tinymce toolbar's configuration as well as in the plugins' configuration as demonstrated bellow:
```javascript
tinymce.init(
    {
	toolbar: ["case"],
        plugins: ["case"],
    }
);
```

