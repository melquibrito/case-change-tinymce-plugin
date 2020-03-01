# Case-Change plugin for _Tinymce WYSIWYG Editor_

To use this plugin copy the folder "case" and paste it into the "plugins" folder in tinymce directory.
Here's the path for tinymce_5.2.0 self-hosted production release -> tinymce_5.2.0/tinymce/js/tinymce/plugins

Download tinymce self-hosted releases [here](https://www.tiny.cloud/get-tiny/self-hosted/).

Here's an example on how to add the dropdown case-change button to your tinymce WYSIWYG editor when initializing it:
```javascript
tinymce.init(
	{
		selector: "YOUR_SELECTOR_HERE",
		toolbar: ["case"],
            	plugins: ["case"],
	}
);
```
Simply include "case" in your tinymce toolbar's configuration as well as in the plugins' configuration.

If you're using tinymce remotely, you might be interested in trying this out instead:
```javascript
tinymce.init(
    {
        selector: "YOUR_SELECTOR_HERE",
        toolbar: ["case"],
        setup: function(editor) {
            editor.ui.registry.addMenuButton('case', {
                icon: 'change-case',
                fetch: function (callback) {
                    let items = [
                        {
                            type: "menuitem",
                            text: 'lower case',
                            onAction: function () {
                                var selectedText = editor.dom.decode(editor.selection.getContent());
                                selectedText = selection.toLowerCase();
                                editor.selection.setContent(selectedText);
                                editor.save();
                                editor.isNotDirty = true;
                            }
                        },
                        {
                            type: "menuitem",
                            text: 'UPPER CASE',
                            onAction: function () {
                                var selectedText = editor.dom.decode(editor.selection.getContent());
                                selectedText = selectedText.toUpperCase();
                                editor.selection.setContent(selectedText);
                                editor.save();
                                editor.isNotDirty = true;
                            }
                        },
                        {
                            type: "menuitem",
                            text: 'Sentence case',
                            onAction: function () {
                                String.prototype.sentenceCase = function () {
                                    return this.toLowerCase().replace(/(^\s*\w|[\.\!\?]\s*\w)/g, function (c) {
                                        return c.toUpperCase()
                                    });
                                }
                                var selectedText = editor.dom.decode(editor.selection.getContent());
                                selectedText = selectedText.sentenceCase();
                                editor.selection.setContent(selectedText);
                                editor.save();
                                editor.isNotDirty = true;
                            }
                        },
                        {
                            type: "menuitem",
                            text: 'Tittle Case',
                            onAction: function () {
                                String.prototype.toTitleCase = function () {
                                    return this.toLowerCase().replace(/(^|[^a-z'])([a-z'])/g, function (m, p1, p2) {
                                        return p1 + p2.toUpperCase();
                                    });
                                }
                                var selectedText = editor.dom.decode(editor.selection.getContent());
                                selectedText = selectedText.toTitleCase();
                                editor.selection.setContent(selectedText);
                                editor.save();
                                editor.isNotDirty = true;
                            }
                        }
                    ]
                    callback(items);
                }
            });
        }
    }
);
```

