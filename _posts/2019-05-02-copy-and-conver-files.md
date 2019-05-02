---
layout: post
title:  "轉換資料匣內的某類型檔案"
date:   2019-05-02
categories: linux
tags: linux shell
---

* content
{:toc}

## 目標

把資料路徑下所有vtt檔案類型轉換成srt檔，
srt檔案內移除原vtt檔內第1,2行

![file_convert.JPG]({{ site.url }}/assets/images/file_convert.JPG)


## 程式

shell name: `copyAndChangname.sh`

```bash
#!/bin/sh

FILE=$1
files=''

copyFileAndRename(){
    sFILE=$1

    if [[ -e "$sFILE" ]]; then 
        if [[ -f "$sFILE" ]]; then 
            substr=$(echo $sFILE | cut -d"." -f 1-2)
            sed '1,2d' "$sFILE" > "$substr"".srt"
        fi 
    fi
}

if [[ -e "$FILE" ]]; then 
    if [[ -d "$FILE" ]]; then 
        cd "$FILE"
        files=(*)
        for file in "${files[@]}"; do            
            if [[ $file == *"vtt"* ]]; then
                copyFileAndRename "$file"
            fi
        done
    fi 
fi
```

run `copyAndChangname.sh "target path"`

## 步驟

進入程式後先判斷傳入的路徑是否是folder，

```bash
if [[ -e "$FILE" ]]; then 
    if [[ -d "$FILE" ]]; then 
    .....    
```

如果是的話切換到傳入的目標路徑，

取得該folder中所有檔案`files=(*)`

判斷檔案檔名是否包含「vtt」，是的話進入function `copyFileAndRename`

*copyFileAndRename* 要放在程式中需要被呼叫之前的位置，不然會*command not found*

進入copyFileAndRename後，先依檔名判斷是否是檔案，先用`$(echo $sFILE | cut -d"." -f 1-2)`取主檔名字串

然後用`sed '1,2d' "$sFILE"`讀取檔案內容，並移除檔案內頭兩行，接著`> "$substr"".srt"`把內容寫到名稱為`主檔名字串.srt`的檔案中

