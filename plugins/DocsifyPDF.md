# 1、DocsifyPDF

- 嵌入PDF到您的个人站点

## 1.1、配置

1. `index.html`引入
```html
<!-- PDFObject.js is a required dependency of this plugin -->
  <script src="//cdnjs.cloudflare.com/ajax/libs/pdfobject/2.1.1/pdfobject.min.js"></script>
  <!-- This is the source code of the pdf embed plugin -->
  <script src="path-to-file/docsify-pdf-embed.js"></script>
  <!-- or use this if you are not hosting the file yourself -->
  <script src="//unpkg.com/docsify-pdf-embed-plugin/src/docsify-pdf-embed.js"></script>
```

2. markdown属性
```javascript
window.$docsify = {
    // pdf嵌入
    markdown: {
        renderer: {
            code: function (code, lang) {
                if(lang === 'pdf'){
                    var pdf_renderer = function(code, lang, verify) {
                        function unique_id_generator(){
                            function rand_gen(){
                                return Math.floor((Math.random()+1) * 65536).toString(16).substring(1);
                            }
                            return rand_gen() + rand_gen() + '-' + rand_gen() + '-' + rand_gen() + '-' + rand_gen() + '-' + rand_gen() + rand_gen() + rand_gen();
                        }
                        if(lang && !lang.localeCompare('pdf', 'en', {sensitivity: 'base'})){
                            if(verify){
                                return true;
                            }else{
                                var divId = "markdown_code_pdf_container_" + unique_id_generator().toString();
                                var container_list = new Array();
                                if(localStorage.getItem('pdf_container_list')){
                                    container_list = JSON.parse(localStorage.getItem('pdf_container_list'));
                                }
                                container_list.push({"pdf_location": code, "div_id": divId});
                                localStorage.setItem('pdf_container_list', JSON.stringify(container_list));
                                return (
                                    '<div style="margin-top:'+ PDF_MARGIN_TOP +'; margin-bottom:'+ PDF_MARGIN_BOTTOM +';" id="'+ divId +'">'
                                    + '<a href="'+ code + '"> Link </a> to ' + code +
                                    '</div>'
                                );
                            }
                        }
                        return false;
                    }
                    if(pdf_renderer(code, lang, true)){
                        return pdf_renderer(code, lang, false);
                    }
                }else {
                    return this.origin.code.apply(this, arguments);
                }
            }
        }
    },
}
```

3. 使用

````markdown
```pdf
	workstudy/workstudy_bank/银行法律法规必备百条.pdf
```
````

```pdf
	workstudy/workstudy_bank/银行法律法规必备百条.pdf
```
