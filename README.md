# LPS-Scripts
Useful LPS scripts for analyzing iis logs

## Request per hour ##
```
SELECT QUANTIZE(TO_TIMESTAMP(date, time), 3600) AS Hour, 
    COUNT(*) AS Total,  
    SUM(sc-bytes) AS TotBytesSent 
FROM '[LOGFILEPATH]'
GROUP BY Hour
ORDER BY Hour
```

## Average Latency per hour ##
```
SELECT QUANTIZE(TO_TIMESTAMP(date, time), 3600) AS Hour, 
    COUNT(*) AS Total,  
    AVG(time-taken) AS AvgTime 
FROM '[LOGFILEPATH]'
GROUP BY Hour
ORDER BY Hour
```

## Hits by IP address ##
```
SELECT c-ip, count(c-ip) as requestcount
FROM '[LOGFILEPATH]' 
WHERE cs-uri-stem like '%/%' 
GROUP BY c-ip 
ORDER BY count(c-ip) desc
```

## HTTP 404 errors ##
```
SELECT sc-status,
    cs-uri-stem,
    Count(*) AS Total
From '[LOGFILEPATH]'
Where sc-status = 404
Group By sc-status, cs-uri-stem
ORDER BY Total desc
```

## Search by keyword ##
```
SELECT cs-uri-stem
FROM '[LOGFILEPATH]'
WHERE cs-uri-stem LIKE '%keyword%'
```

## Request > 8 seconds ##
```
SELECT 
    date,
    time,
    cs-uri-stem,
    cs-uri-query,
    sc-status,
    time-taken
FROM '[LOGFILEPATH]'
WHERE time-taken>8000
GROUP BY date, time,cs-uri-stem, cs-uri-query, cs(user-Agent), sc-status, time-taken
ORDER BY time-taken DESC
```