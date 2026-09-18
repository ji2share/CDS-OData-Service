    lt_orders =
      SELECT h.mandt, h.vbeln,
           t.bukrs,
           h.vkorg,
           h.kunnr,
           h.netwr,
           h.waerk,
           h.erdat,
           DAYS_BETWEEN( h.erdat, CURRENT_DATE ) AS days_open,
           k.lfgsk                               AS overall_delivery_status,
           k.fksak                               AS overall_billing_status,
           k.gbstk                                AS status_indicator
      FROM vbak AS h
      left outer join vbuk AS k
        ON k.vbeln = h.vbeln
      left outer join tvko AS t
        ON t.vkorg = h.vkorg;
