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
		selector: "YOUR_SELECTOR_HERE",
		toolbar: ["case"],
            	plugins: ["case"],
	}
);
```

If you're using tinymce remotely in your application, then you might be interested in trying this out instead:
```javascript
tinymce.init(
    {
        selector: "YOUR_SELECTOR_HERE",
        toolbar: ["case"],
        setup: function(editor) {
            tinymce.PluginManager.add('case',
		function (editor) {

		    const strings = {
			TOOLNAME: 'Change Case',
			LOWERCASE: 'lowercase',
			UPPERCASE: 'UPPERCASE',
			SENTENCECASE: 'Sentence case',
			TITLECASE: 'Title Case'
		    }, defaultTitleCaseExeptions = [
			'at', 'by', 'in', 'of', 'on', 'up', 'to', 'en', 're', 'vs',
			'but', 'off', 'out', 'via', 'bar', 'mid', 'per', 'pro', 'qua', 'til',
			'from', 'into', 'unto', 'with', 'amid', 'anit', 'atop', 'down', 'less', 'like', 'near', 'over', 'past', 'plus', 'sans', 'save', 'than', 'thru', 'till', 'upon',
			'for', 'and', 'nor', 'but', 'or', 'yet', 'so', 'an', 'a', 'some', 'the'
		    ], getParameterArray = function () {
			let param = editor.getParam('title_case_excepions');
			if (param) {
			    if (Array.isArray(param)) {
				return param;
			    } else if (typeof param === "string") {
				return param.replace(/(\s{1,})/g, "?").trim().split('?');
			    }
			}
			return defaultTitleCaseExeptions;
		    }

		    var titleCaseExceptions = getParameterArray();
		    /*
		     * Appending new functions to String.prototype...
		     */
		    String.prototype.toSentenceCase = function () {
			return this.toLowerCase().replace(/(^\s*\w|[\.\!\?]\s*\w)/g, function (c) {
			    return c.toUpperCase()
			});
		    }
		    String.prototype.toTitleCase = function () {
			let tt = (str) => {
			    let s = str.split('.'), w;
			    for (let i in s) {
				let w = s[i].split(' '),
				    j = 0;

				if (s[i].trim().replace(/(^\s+|\s+$)/g, "").length > 0) {
				    for (j; j < w.length; j++) {
					let found = false;
					for (let k = 0; k < w[j].length; k++) {
					    if (w[j][k].match(/([a-z'áàâãäéèêëíìîïóòôõöúùûü])/i)) {
						w[j] = w[j][k].toUpperCase() + w[j].slice(k + 1);
						found = true;
						break;
					    }
					}
					if (found) {
					    break;
					}
				    }
				    for (j; j < w.length; j++) {
					if (titleCaseExceptions.indexOf(w[j]) === -1) {
					    for (let k = 0; k < w[j].length; k++) {
						if (w[j][k].match(/([a-z'áàâãäéèêëíìîïóòôõöúùûü])/i)) {
						    w[j] = w[j][k].toUpperCase() + w[j].slice(k + 1);
						    break;
						}
					    }
					}
				    }
				    s[i] = w.join(' ');
				}
			    }
			    return s.join('.');
			};
			return tt(this.toLowerCase());
		    }

		    /*
		     * Helpers
		     */
		    const getDecodedSelection = function () {
			return editor.dom.decode(editor.selection.getContent());
		    }

		    const perform = function (func) {
			let content = func(),
			    bookmark = editor.selection.getBookmark();
			editor.selection.setContent(content);
			editor.save();
			editor.isNotDirty = true;
			editor.focus();
			editor.selection.moveToBookmark(bookmark);
		    }

		    String.prototype.apply = function (method) {
			switch (method) {
			    case strings.LOWERCASE:
				return this.toLowerCase();
			    case strings.UPPERCASE:
				return this.toUpperCase();
			    case strings.SENTENCECASE:
				return this.toSentenceCase();
			    case strings.TITLECASE:
				return this.toTitleCase();
			    default:
				return this;
			}
		    }

		    const handler = function (f, l, node, rng, method) {
			if (f && l) {
			    node.textContent = node.textContent.slice(0, rng.startOffset) + node.textContent.slice(rng.startOffset, rng.endOffset).apply(method) + node.textContent.slice(rng.endOffset);
			} else if (f && !l) {
			    node.textContent = node.textContent.slice(0, rng.startOffset) + node.textContent.slice(rng.startOffset).apply(method);
			} else if (!f && l) {
			    node.textContent = node.textContent.slice(0, rng.endOffset).apply(method) + node.textContent.slice(rng.endOffset);
			} else {
			    node.textContent = node.textContent.apply(method);
			}
		    }

		    const apply = function (method) {
			let walker = new tinymce.dom.TreeWalker(editor.selection.getNode()),
			    rng = editor.selection.getRng(),
			    bm = editor.selection.getBookmark(2,true),
			    current, next, first = walker.current();

			console.log(rng);
			do {
			    current = walker.current();
			    if (current.nodeName === '#text') {
				let f = current === rng.startContainer, l = current === rng.endContainer;
				handler(f, l, current, rng, method);
			    }
			    next = walker.next();
			    if (next === first) break;
			} while (next);
			editor.save();
			//editor.isNotDirty = true;
			editor.focus();
			editor.selection.moveToBookmark(bm);
		    }
		    /*
		     * Element Builders 
		     */
		    const getMenuItems = function () {
			return [
			    {
				type: "menuitem",
				text: strings.LOWERCASE,
				//onAction: lowerCase()
				onAction: () => apply(strings.LOWERCASE)
			    },
			    {
				type: "menuitem",
				text: strings.UPPERCASE,
				//onAction: upperCase()
				onAction: () => apply(strings.UPPERCASE)
			    },
			    {
				type: "menuitem",
				text: strings.SENTENCECASE,
				//onAction: sentenceCase()
				onAction: () => apply(strings.SENTENCECASE)
			    },
			    {
				type: "menuitem",
				text: strings.TITLECASE,
				//onAction: titleCase()
				onAction: () => apply(strings.TITLECASE)
			    }
			]
		    }

		    const getMenuButton = function () {
			return {
			    icon: 'change-case',
			    tooltip: strings.TOOLNAME,
			    fetch: function (callback) {
				const items = getMenuItems();
				callback(items);
			    }
			}
		    }

		    const getNestedMenuItem = function () {
			return {
			    text: strings.TOOLNAME,
			    getSubmenuItems: () => {
				return getMenuItems();
			    }
			}
		    }

		    /*
		     * Creating tinymce elements so that they can be invoked in tinymce init function...
		     */
		    editor.ui.registry.addMenuButton('case', getMenuButton());
		    editor.ui.registry.addNestedMenuItem('case', getNestedMenuItem());

		    /*
		     * Adding commands to tinymce...
		     */
		    editor.addCommand('mceLowerCase', () => apply(strings.LOWERCASE));
		    editor.addCommand('mceUpperCase', () => apply(strings.UPPERCASE));
		    editor.addCommand('mceSentenceCase', () => apply(strings.SENTENCECASE));
		    editor.addCommand('mceTitleCase', () => apply(strings.TITLECASE));

		    /*
		     * Metadata returned so that tinymce help plugin can make use of it.
		     */
		    return {
			getMetadata: function () {
			    return {
				name: "Case",
				url: "https://github.com/melquibrito/Case-change-tinymce-plugin"
			    }
			}
		    }
		}
	    );
        }
    }
);
```

