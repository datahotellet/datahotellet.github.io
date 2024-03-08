# ELMA-loggsjekk

Her er diverse innsikt frå Nginx-loggfilene, inkludert kommandoane som vart brukt og nokre betraktningar.

Livar Bergheim. 07.03.2024

## Loggfilene

Nginx-loggar for 14 heile dagar (22. februar – 5. mars 2024)

9 615 294 logginnslag i desse loggane.

`gunzip -c *access*.gz | wc -l`

## Innsikt

### Andel trafikk som er ELMA
Ser kun på participants-datasettet og ser vekk frå dei to andre (capabilities og documenttypes).

`gunzip -c *access*.gz | grep 'difi/elma/participants' | wc -l`

3 055 350 = nesten 1/3 av trafikken i perioden.

### Topp: timar med flest forespørslar

`gunzip -c *access*.gz | grep 'difi/elma/participants' | awk '{if ($1 ~ /,/) print substr($5, 2, length($5)-7); else print substr($4, 2, length($4)-7)}' | sort | uniq -c | sort -nr | head -n 20`

```
47359 04/Mar/2024:05
46975 26/Feb/2024:05
25625 29/Feb/2024:05
24297 28/Feb/2024:05
23718 01/Mar/2024:05
22975 27/Feb/2024:05
22961 23/Feb/2024:05
22694 05/Mar/2024:05
22241 22/Feb/2024:05
20299 26/Feb/2024:23
19825 01/Mar/2024:06
19677 01/Mar/2024:23
19650 23/Feb/2024:23
19614 25/Feb/2024:23
19487 03/Mar/2024:23
18010 29/Feb/2024:06
17642 26/Feb/2024:06
17486 04/Mar/2024:06
17432 28/Feb/2024:06
17383 27/Feb/2024:06
```

### Topp: minutt med flest forespørslar

Ser ut til å vere batch-jobbar som køyrer kl. 05 og 21.

`gunzip -c *access*.gz | grep 'difi/elma/participants' | awk '{if ($1 ~ /,/) print substr($5, 2, length($5)-4); else print substr($4, 2, length($4)-4)}' | sort | uniq -c | sort -nr | head -n 20`

```
2988 29/Feb/2024:21:00
2985 28/Feb/2024:21:00
2970 23/Feb/2024:21:00
2969 24/Feb/2024:21:00
2945 01/Mar/2024:21:00
2944 03/Mar/2024:21:00
2926 04/Mar/2024:21:00
2914 05/Mar/2024:21:00
2895 27/Feb/2024:21:00
2864 25/Feb/2024:21:00
2863 26/Feb/2024:21:00
2863 02/Mar/2024:21:00
2847 26/Feb/2024:05:02
2842 26/Feb/2024:05:04
2837 26/Feb/2024:05:06
2823 26/Feb/2024:05:03
2814 04/Mar/2024:05:03
2797 26/Feb/2024:05:05
2796 22/Feb/2024:21:00
2795 04/Mar/2024:05:04
```

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

### Kor stor del er nedlasting?

6219 nedlastingskall. 

`gunzip -c *access*.gz | grep 'download/difi/elma/participants' | grep '"GET ' | wc -l`

### Kor mange filtrerer på org.nr.
Ved å bruke identifier=<orgnr> i URL:
482 594

`gunzip -c *access*.gz | grep 'difi/elma/participants' | grep '"GET ' | grep 'identifier=' | wc -l`

Ein kan også søke på org.nr. med query-paremeteret.
Tal på logg-linjer med "query=" etterfulgt av ni siffer (org.nr.):
2 073 451

`gunzip -c *access*.gz | grep 'difi/elma/participants' | grep '"GET ' | grep -E 'query=[0-9]{9}' | wc -l`

Totalt blir dette 2 556 045 for perioden.

Snitt:
- 182 574 pr. døgn
- 7607 pr. time
- 126 pr. minutt
- 2,1 pr. sekund

### Kor mange søker på foretaksnamn

Logg-linjer med "query=" der første teikn etterpå ikkje er eit tal:
126 047

`gunzip -c *access*.gz | grep 'difi/elma/participants' | grep '"GET ' | awk '/query=[^0-9]/' | wc -l`