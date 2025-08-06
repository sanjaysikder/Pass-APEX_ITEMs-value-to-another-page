# Oracle APEX: Pass Values from Classic Report to Another Page
This guide demonstrates how to pass values from a classic report to another page using APEX items and JavaScript.
## Prerequisites
- Oracle APEX 19.1 or higher
- Classic report region with editable fields
- Target page with items to receive the values
## IMPLEMENTATION STEPS
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

```



## Step 2: JavaScript Code
### Add this JavaScript to the page's Function and Global Variable Declaration section:

//apex_iten Edited Value set another normal page


```JavaScript
document.querySelector('#my_report').addEventListener('click', function(e) {
if (e.target && e.target.classList.contains('btn-pass')) {
var row = e.target.closest('tr');
var transID = row.querySelector('input[name="f01"]');
var unitPrice = row.querySelector('input[name="f02"]');
var prodctName = row.querySelector('input[name="f03"]');
var tranID = transID ? transID.value : '';
var uPrice = unitPrice ? unitPrice.value : '';
var itemName = prodctName ? prodctName.value : '';
// Redirect with multiple page items (P4_PID_TRANSACTION, P4_PID_PRODUCT, P4_UNIT_PRICE)
var url = 'f?p=' + $v('pFlowId') + ':4:' + $v('pInstance') +'::NO::P4_PID_TRANSACTION,P4_PID_PRODUCT,P4_UNIT_PRICE:' +encodeURIComponent(tranID) + ',' +encodeURIComponent(uPrice) + ',' +encodeURIComponent(itemName);
window.location.href = url;
}
});
```

### Key Components:

-Event listener attached to the report container (#my_report)
-Identifies clicked button by class (btn-pass)
-Extracts values from APEX items (f01, f02, f03)
-Constructs URL with encoded values for target page items
-Redirects to page 4 with the values

## Thank You
## Sanjay Sikder
