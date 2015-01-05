---
---

# 使用 table 画格子

## 使用 border-collapse: collapse
### 代码
```
<style>
table {
    border-collapse: collapse;
}
table td{
    width: 16px;
    height: 16px;
    border: 1px solid #eee;
}
</style>

<table>
    <tr><td>1</td><td>2</td></tr>
    <tr><td>3</td><td>4</td></tr>
    <tr><td>5</td><td>6</td></tr>
</table>
```

### 效果

<style>
table {
    border-collapse: collapse;
}
table td{
    width: 16px;
    height: 16px;
    border: 1px solid #eee;
}
</style>

<table>
    <tr><td>1</td><td>2</td></tr>
    <tr><td>3</td><td>4</td></tr>
    <tr><td>5</td><td>6</td></tr>
</table>

## 延伸阅读

The border-collapse property sets whether the table borders are collapsed into a single border or detached as in standard HTML.

border-collapse: separate|*collapse*|initial|inherit;

## 参考
1. http://stackoverflow.com/questions/1763032/html-css-table-with-gridlines
2. http://www.w3schools.com/cssref/pr_border-collapse.asp
