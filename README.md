# Oracle APEX: Pass Values from Classic Report to Another Page

This guide demonstrates how to pass values from a classic report to another page using APEX items and JavaScript.

## Prerequisites
- Oracle APEX 19.1 or higher
- Classic report region with editable fields
- Target page with items to receive the values

## Implementation Steps

### Step 1: Configure Classic Report Query
Modify your classic report query to include APEX items and buttons:

```sql
SELECT 
    a.PID,
    a.PID_TRANSACTION,
    a.PID_PRODUCT,
    APEX_ITEM.DISPLAY_AND_SAVE(1, A.PID_TRANSACTION) TRANSACTION_ID,
    APEX_ITEM.TEXT(2, a.UNIT_PRICE, 2) AS UNIT_PRICE,
    APEX_ITEM.DISPLAY_AND_SAVE(3, b.OBJECT_NAME) PRODUCT_NAME,
    '<button class="btn-pass" type="button">Set Value Normal Page</button>' AS button,
    a.QUANTITY,
    a.DISCOUNT_PCT,
    a.VAT_PCT,
    a.AMOUNT_TOBE_PAY,
    a.TOTAL_DISCOUNT,
    a.LINE_TOTAL,
    a.TOTAL_VAT
FROM 
    TRD_TRANSACTION_DETAIL a
JOIN 
    TRD_PRODUCT_INFO b ON a.PID_PRODUCT = b.PID
WHERE 
    a.PID_TRANSACTION = NVL(:P5_TRANSACTION_ID, a.PID_TRANSACTION)



## Thank You
