# Investigation Timeline
| Timestamp from Log | Source | Destination | Event | Evidence |
|---|---|---|---|---|
| `1747147647.668533` | `192.168.1.8:52917` | `192.0.78.212:80` | TCP connection recorded; service identified as HTTP | `conn.log` |
| `1747147647.702181` | `192.168.1.8:52917` | `192.0.78.212:80` | HTTP `GET /` for `zeek.org`; response status `301 Moved Permanently` | `http.log` |
| `1747147654.272660` | `192.168.1.8:52918` | `192.0.78.150:80` | TCP connection recorded; service identified as HTTP | `conn.log` |
| `1747147654.311012` | `192.168.1.8:52918` | `192.0.78.150:80` | HTTP request for `zeek.org`; method value `WEIRD`; response status `301 Moved Permanently` | `http.log` |

