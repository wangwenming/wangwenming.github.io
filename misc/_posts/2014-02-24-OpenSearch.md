---
---

#OpenSearch

```xml
<OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/" xmlns:moz="http://www.mozilla.org/2006/browser/search/">
    <ShortName>360搜索</ShortName>
    <Description>360搜索</Description>
    <InputEncoding>UTF-8</InputEncoding>
    <Url type="text/html" method="get" template="http://wwm.www.so.com/s">
        <Param name="q" value="{searchTerms}"/>
    </Url>
    <Image height="16" width="16" type="image/png">http://wwm.www.so.com/favicon.ico</Image>
    <moz:SearchForm>http://wwm.www.so.com/</moz:SearchForm>
</OpenSearchDescription>
```

AddSearchProvider.html
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>title</title>
</head>
<body>
aaaa
<script>
function installSearchEngine() {
 if (window.external && ("AddSearchProvider" in window.external)) {
   // Firefox 2 and IE 7, OpenSearch
   window.external.AddSearchProvider("http://wwm.www.so.com/opensearch.xml");
 } else {
   // No search engine support (IE 6, Opera, etc).
   alert("No search engine support");
 }
}

document.body.onclick = installSearchEngine;
</script>
</body>
</html>
```
