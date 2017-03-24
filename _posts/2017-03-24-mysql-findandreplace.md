---
layout: post
title:  "MySQL - Procedre - 替換Column內容"
date:   2017-03-24
categories: SQL
tags: MySQL
---

* content
{:toc}

要找到MySQL整個Schema中的varchar欄位是否有某個字，然後把該字替換掉。

這篇要筆記的是如何寫一個MySQL Stored Procedure，有
- 變數宣告
- 動態select, update語法組成
- Cursor使用


## 步驟

A.

  列出Schema所有varchar欄位，把找到的結果塞到CURSOR
  ```sql
  DECLARE cur1 CURSOR FOR select table_name, column_name from information_schema.columns
where table_schema = 'genius' and data_type = 'varchar' order by table_name,ordinal_position;
  ```


B.

  LOOP CURSOR，先看某table中某column是否包含要找的關鍵字，有找到的話就執行update。




## Stored Procedure
```sql
DROP PROCEDURE IF EXISTS replace_string;

DELIMITER //
/* 查看一schema中所有varchar column 中是否有某個字，找到時替換掉*/
CREATE PROCEDURE replace_string()
BEGIN
 DECLARE done INT DEFAULT 0;
 DECLARE tableName varchar(100);
 DECLARE columnName varchar(100);   
 DECLARE stmt VARCHAR(1024);   -- sql stmt 1 查找某table的colmn中是否有要找的關鍵字
 DECLARE stmt2 VARCHAR(1024);  -- sql stmt 2 執行update 語法

 DECLARE cur1 CURSOR FOR select table_name, column_name from information_schema.columns
where table_schema = 'genius' and data_type = 'varchar' order by table_name,ordinal_position;

 DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

 OPEN cur1;

  REPEAT
	  FETCH cur1 INTO tableName, columnName;
	  IF NOT done THEN

      /* 查看這個varchar column是否有比對到要找的字，這裡是找MARTY，把結果放在 @CheckExists ，沒比對到是 ０*/
      SET @sqltext = CONCAT('select count(*) into @CheckExists from ',tableName,' where ',columnName, ' LIKE ''%MARTY%'';');
      PREPARE stmt FROM @sqltext;
      execute stmt;
      DEALLOCATE PREPARE stmt;

      /* UPDATE -> 把找到關鍵字的column取出來replace後再set回去 */
      SET @sqltext2 = CONCAT('UPDATE ',tableName,' SET ',columnName,' = REPLACE(',columnName,' ,''Marty'', ''YTRMA'') WHERE ', columnName,' like ''%MARTY%'';');
      PREPARE stmt2 FROM @sqltext2;

    /* @CheckExists 大於0時執行UPDATE  */  
  IF ( @CheckExists > 0 ) THEN
    BEGIN
      execute stmt2;
	  END;
  END IF;

	END IF;
	UNTIL done END REPEAT;

    CLOSE cur1;
	DEALLOCATE PREPARE stmt2;

END//
DELIMITER ;

```
