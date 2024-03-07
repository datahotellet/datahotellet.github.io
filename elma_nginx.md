# ELMA-loggsjekk

Her er diverse innsikt frå Nginx-loggfilene, inkludert kommandoane som vart brukt og nokre betraktningar.

Livar Bergheim. 07.03.2024

## Loggfilene

Nginx-loggar for 14 heile dagar (22. februar – 5. mars 2024)

9 615 294 logginnslag i desse loggane.

`gunzip -c *access*.gz | wc -l`

## Innsikt

### Topp: sekund med flest forespørslar

Tydelegivs ein prosess som køyrer kl. 21:00 som er altfor aggressiv.

```gunzip -c *access*.gz | grep 'difi/elma/participants' | awk '{if ($1 ~ /,/) print $5; else print $4}' | tr -d '[]' | sort | uniq -c | sort -nr | head -n 30```

```
  88 03/Mar/2024:21:00:02
  86 01/Mar/2024:21:00:19
  84 25/Feb/2024:21:00:02
  84 24/Feb/2024:21:00:23
  82 04/Mar/2024:21:00:02
  81 28/Feb/2024:21:00:02
  81 24/Feb/2024:21:00:02
  80 23/Feb/2024:21:00:03
  80 22/Feb/2024:21:00:28
  79 25/Feb/2024:21:00:07
  77 24/Feb/2024:21:00:33
  77 24/Feb/2024:21:00:09
  77 23/Feb/2024:21:00:10
  77 02/Mar/2024:21:00:28
  76 29/Feb/2024:21:00:18
  76 28/Feb/2024:21:00:12
  76 28/Feb/2024:21:00:06
  76 01/Mar/2024:21:00:36
  75 27/Feb/2024:21:00:11
  74 29/Feb/2024:21:00:04
  74 27/Feb/2024:21:00:07
  74 23/Feb/2024:21:00:02
  74 22/Feb/2024:21:00:15
  74 22/Feb/2024:21:00:02
  74 01/Mar/2024:21:00:30
  74 01/Mar/2024:21:00:16
  73 27/Feb/2024:21:00:29
  73 27/Feb/2024:21:00:02
  73 23/Feb/2024:21:00:09
  73 23/Feb/2024:21:00:07
```

