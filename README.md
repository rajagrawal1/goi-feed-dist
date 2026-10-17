# goi-feed-dist

The signed tax news feed that the Spend Free app downloads. This repo holds only the
published files; a private pipeline builds them from official sources and pushes here
about twice a day.

| File | What it is |
|---|---|
| `announcements.json` | News items from official Government of India and RBI sites: Income Tax Department (`incometax.gov.in`), GST portal (`gst.gov.in`) and RBI press releases (`rbi.org.in`). Each item links to the original page. |
| `announcements.json.sig` | Ed25519 signature over the exact bytes of `announcements.json`, base64. |
| `ed25519_pub.pem` | The public key. The app has the same key built in and refuses any file it doesn't verify. |

`generatedAt` says when the file was built. `sources` says whether each site worked on the
latest run and `lastOkAt` when it last did.

Check a copy yourself:

```sh
python3 -c "
import base64
from cryptography.hazmat.primitives.serialization import load_pem_public_key
key = load_pem_public_key(open('ed25519_pub.pem', 'rb').read())
key.verify(base64.b64decode(open('announcements.json.sig').read()), open('announcements.json', 'rb').read())
print('signature OK')"
```

The content is public government record, reproduced with attribution from the sources
above. Spend Free shows it as news only: check with a CA before acting on it.
