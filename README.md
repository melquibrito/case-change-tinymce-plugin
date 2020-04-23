# Case-Change plugin for _Tinymce WYSIWYG Editor_

![preview](/Toolbar-menuButton-preview.png)

To use this plugin copy the folder "case" and paste it into the "plugins" folder in tinymce directory.
Here's the path for tinymce_5.2.0 self-hosted production release -> tinymce_5.2.0/tinymce/js/tinymce/plugins

Download any of tinymce self-hosted releases [here](https://www.tiny.cloud/get-tiny/self-hosted/).

## Tutorial
### Initializing
Add the dropdown case-change button to your _tinymce WYSIWYG editor_ by simply including **"case"** in your tinymce toolbar's configuration as well as in the plugins' configuration as demonstrated bellow:
```javascript
tinymce.init({
    toolbar: ["case"],
    plugins: ["case"],
});
```
### Configuration
#### Title case minors
The following prepositions won't be title cased by default: _at_, _by_, _in_, _of_, _on_, _up_, _to_, _en_, _re_, _vs_, _but_, _off_, _out_, _via_, _bar_, _mid_, _per_, _pro_, _qua_, _til_, _from_, _into_, _unto_, _with_, _amid_, _anit_, _atop_, _down_, _less_, _like_, _near_, _over_, _past_, _plus_, _sans_, _save_, _than_, _thru_, _till_, _upon_, _for_, _and_, _nor_, _but_, _or_, _yet_, _so_, _an_, _a_, _some_, _the_. 

To change the whole set of title case minors (to a different language, for instance), simply include _**title_case_minors**_ to your tinymce init configuration as demonstrated bellow:
```javascript
tinymce.init({
    toolbar: ["case"],
    plugins: ["case"],
    title_case_minors: ["your", "customized", "set", "of", "minors", "here"]
    // You can also set it this way. Do it as you prefer.
    // title_case_minors: "your customized set of minors here with a space in between them"
});
```
To include minors to the default set, use _**inlcude_to_title_case_minors**_.
To exclude any of the title minors, use _**rule_out_from_title_case_minors**_.

In the example below we are including **who**, **whom**, and **that**, and excluding **down**, **into**, and **onto**.
```javascript
tinymce.init({
    toolbar: ["case"],
    plugins: ["case"],
    inlcude_to_title_case_minors: ["who", "whom", "that"], // These won't be title cased anymore.
    rule_out_from_title_case_minors: ["down", "into", "onto"] // These will now be title cased.
});
```

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE.md) file for more details.
