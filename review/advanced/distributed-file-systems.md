# Distributed File Systems

Distributed file systems expose familiar file operations while storing data on another machine. Caching reduces traffic but requires a consistency protocol that determines when clients see updates.

## NFSv2 course model

The supplied historical material uses NFSv2. Requests carry a file handle, explicit offsets where needed, and enough information for server operations without per-client open-file offset state. A handle can identify a volume, inode, and generation, preventing accidental reuse of a stale inode identity. Generation identifies an object incarnation; it is not a version incremented on every content write.

The client can implement local `open()` and descriptor/offset bookkeeping while mapping reads/writes to RPCs. Retry-safe offset writes differ from a naïve remote append request that could append twice after a lost reply. Do not conclude that every NFS operation is perfectly idempotent in all observable outcomes.

Data caching creates update-visibility and stale-cache problems. The course model uses flushing and attribute validation; some exercises reuse attributes for a stated short interval. That interval is an exercise assumption, not a fixed universal NFS constant. A server's durability acknowledgment must match its actual persistence guarantee.

## AFS course model

Whole-file client caching, callbacks, and session-oriented visibility reduce repeated server contact in the classic model. A client can continue reading the version associated with an open session while another client changes the file. Callback invalidation and fetching for a later open must follow the exercise's rules.

## Trace method

For each client, track open/closed status, cached blocks or whole file, dirty changes, cached attributes and their age, callback validity, and the server version. Keep network request/response operations separate from application read/write calls. Preserve the two-client column layout from the original PDF; extracted text can flatten it and lose which client acted.

Compare protocol behavior under the same trace and explicit assumptions. Do not present these historical models as a statement of every current NFS/AFS implementation.

Source: `CS537 - Final Review.pdf`, PDF pp. 60-64; `FA19 - exam.pdf`, first sequence pp. 9-11 and 14. For request failure semantics see [[review/advanced/network-reliability-and-rpc|RPC]].
