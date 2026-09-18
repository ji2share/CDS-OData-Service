Table Function:


Task 1: Need to be done by same developer
1. Need to add BUKRS as parameter
2. Then adjust the class method for that.

Tsk 2: Billing Document

Add this code with existing method, and activate the method
    lt_billing_sum =
      SELECT aubel AS vbeln,
             SUM( netwr ) AS billing_amount
        FROM vbrp
       WHERE aubel IN ( SELECT vbeln FROM :lt_orders )
       GROUP BY aubel;
