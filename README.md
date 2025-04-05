# 【数式】営業日(土日を除く)を考慮した期日判定  

## 概要
期日(DueDate__c)が今日以前の場合は"期日超過"、営業日ベースで2日以内なら"警告"、それ以外は空白を表示する。  
※20行目の`) <= 2`を変更することで閾値を調整できます。  

## 数式
```
IF(
    DueDate__c < TODAY(),
    "期日超過",
    IF(
        AND(
            DueDate__c >= TODAY(),
            (
                (DueDate__c - TODAY()) - 
                (2 * FLOOR((DueDate__c - TODAY() + WEEKDAY(TODAY()) - 1) / 7)) - 
                IF(
                    MOD(WEEKDAY(TODAY()) + (DueDate__c - TODAY()) - 1, 7) = 6, 
                    1, 
                    0
                ) - 
                IF(
                    MOD(WEEKDAY(TODAY()) + (DueDate__c - TODAY()) - 1, 7) = 0, 
                    1, 
                    0
                )
            ) <= 2
        ),
        "警告",
        ""
    )
)
```