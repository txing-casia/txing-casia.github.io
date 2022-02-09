---
layout:     post
title:      "在VS Code配置编译LaTeX环境"
subtitle:   ""
date:       2021-01-03
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - LaTeX
    - VS Code
---

# 在VS Code配置编译LaTeX环境

## 实施步骤

- Step 1：下载[VS Code](https://code.visualstudio.com/)，[SumatraPDF](https://www.sumatrapdfreader.org/free-pdf-reader.html)，[texlive (阿里云)](https://mirrors.aliyun.com/CTAN/systems/texlive/Images/)或者[texlive (华为云)](https://mirrors.huaweicloud.com/CTAN/systems/texlive/Images/)，texlive只下载texlive.iso文件，安装参考网上教程

- Step 2：VS Code中安装 LaTeX Workshop 插件

- Step 3：修改 user setting

```

{

    "latex-workshop.latex.tools": [
        {
            // 编译工具和命令
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOCFILE%"
            ]
        },
        {
            // 编译工具和命令
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],

    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ],
        },
        {
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],

    "latex-workshop.view.pdf.viewer": "external",

    "latex-workshop.view.pdf.external.viewer.command": "D:/Program Files (x86)/SumatraPDF/SumatraPDF.exe",
    "latex-workshop.view.pdf.external.viewer.args": [
        "-forward-search",
        "%TEX%",
        "%LINE%",
        "-reuse-instance",
        "-inverse-search",
        "\"D:/Program Files (x86)/Microsoft VS Code/Code.exe\" \"D:/Program Files (x86)/Microsoft VS Code/resources/app/out/cli.js\" -gr \"%f\":\"%l\"",
        "%PDF%"
    ],


}
```

***记住修改VS Code位置和SumatraPDF位置**

- Step 4：在新建文件夹中，新建.tex文件，输入

  ```latex
  \documentclass{article}
  \begin{document}
  hello,world
  \end{document}
  ```

  Ctrl+S编译，也可以在左侧Tex一栏设置编译方式



## 参考资料

- [使用VSCode编写LaTeX](https://zhuanlan.zhihu.com/p/38178015)