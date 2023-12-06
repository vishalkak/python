-- Rows where stock and mode match in both tables
SELECT 
    COALESCE(stock1.stock, stock2.stock) AS stock1, 
    COALESCE(stock1.mode, stock2.mode) AS mode1, 
    stock1.quantity AS quantity1,
    stock2.stock AS stock2, 
    stock2.mode AS mode2, 
    stock2.quantity AS quantity2
FROM stock1
FULL OUTER JOIN stock2
ON stock1.stock = stock2.stock
AND stock1.mode = stock2.mode

UNION

-- Rows from stock1 that don't have a match in stock2
SELECT 
    stock1.stock, 
    stock1.mode, 
    stock1.quantity,
    NULL AS stock2, 
    NULL AS mode2, 
    NULL AS quantity2
FROM stock1
WHERE stock1.stock IS NOT NULL
    AND stock1.mode IS NOT NULL
    AND NOT EXISTS (SELECT 1 FROM stock2 WHERE stock1.stock = stock2.stock AND stock1.mode = stock2.mode)

UNION

-- Rows from stock2 that don't have a match in stock1
SELECT 
    NULL AS stock1, 
    NULL AS mode1, 
    NULL AS quantity1,
    stock2.stock, 
    stock2.mode, 
    stock2.quantity
FROM stock2
WHERE stock2.stock IS NOT NULL
    AND stock2.mode IS NOT NULL
    AND NOT EXISTS (SELECT 1 FROM stock1 WHERE stock1.stock = stock2.stock AND stock1.mode = stock2.mode);
