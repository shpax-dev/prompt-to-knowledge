# SSDs and Flash Translation

Flash reads and programs pages but erases larger blocks containing many pages. A page must be in a suitable erased state before programming; overwriting an existing logical page does not necessarily erase its old physical block immediately.

## Flash translation layer

The FTL maps logical block/page addresses to physical flash locations. It does not generally know which application file owns each page; the file system manages that abstraction.

A direct-map teaching design updates a fixed physical location and can require reading, erasing, and reprogramming a whole block. Small random updates then have substantial write amplification.

A log-based FTL instead:

1. Programs the new data into an available erased page.
2. Updates the logical-to-physical mapping.
3. Makes the old physical copy obsolete.
4. Later cleans blocks by relocating remaining live pages, erasing whole blocks, and reusing them.

Mapping persistence and recovery still matter: log placement does not by itself guarantee power-failure safety.

## Read a device-state trace

To `read(L)`, consult the current mapping and return data at that physical page. Do not confuse logical address, physical page number, page contents, programmed state, and current liveness. A programmed page can be obsolete. `trim(L)` tells the device that the logical data need no longer be retained; it need not immediately erase the containing block.

**Original practice:** map logical page 5 to physical page 12 containing A. Updating logical 5 to B at erased physical page 20 changes the map to 5→20. Page 12 can remain programmed but is no longer live for logical 5.

## Performance and endurance

```text
write amplification = physical bytes written / host bytes written
```

Cleaning increases physical writes. Overprovisioning supplies spare space for mapping, cleaning, and scheduling. Wear leveling spreads erase/program wear, including moving relatively cold data when needed. Channels and chips provide parallelism, so an SSD is not one serial flash page.

SLC stores one bit per cell; MLC/TLC/QLC store more. More bits per cell trade density against performance/endurance under the compared technology. A fast SLC write cache can make sustained throughput fall after it fills; use the actual workload evidence before diagnosing a graph.

Source: `CS537 - Final Review.pdf`, PDF pp. 46-53; `FA19 - exam.pdf`, p. 14; `FA23 - exam.pdf`, pp. 3 and 11; `FA24 - exam.pdf`, pp. 4-5.
