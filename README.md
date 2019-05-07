# Welcome to Txing's blog 

### 2019.5.6 解决LaTeX行内公式在GitHub Page不显示问题

一些处理办法是利用在线LaTeX编辑器，将公式转化为图片插入到MD中。但这种做法并没有真正解决问题，同时还影响了清晰度。

这里利用外挂JavaScript的方案来支持跨浏览器的内容渲染——[MathJax](<https://www.mathjax.org/>)。

- **Step 1:** 由于本博客使用Jekyll搭建，在_layout文件夹下找到根模板（一般为default.html）。添加：

  ```html
  <script type="text/javascript"
  	src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  </script>
  ```

  由于GitHub Page支持智能用HTTPS格式访问，一般外挂的网址如果不是HTTPS的（eg. http://），需要省略前半段（http:），只留下"//"。

- **Step 2:** 在_config.yml文件中添加

  ```
  markdown: kramdown
  ```

  替换默认的Markdown引擎为kramdown

- **Step 3:** 最后在cmd中安装kramdown

  ```
  gem install kramdown
  ```

成功之后就可以在MD中添加“\$\$”来编辑行内公式了（eg. `$$\LaTeX$$`  $\LaTeX$）





































