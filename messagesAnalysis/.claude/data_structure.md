# Data Structure

## Source data

Facebook Messenger export: one or more `message_*.json` files in `Data/PrivateData/`.
Facebook exports strings with mojibake (latin-1 bytes re-encoded as UTF-8); the encoder fixes this with `fix_encoding()` before storing.

### Raw JSON shape (per file)
```
{
  "participants": [ {"name": "..."}, ... ],
  "messages":    [ <message>, ... ]
}
```

Multiple export files are merged: participants are deduplicated by name, messages are concatenated then sorted ascending by `timestamp_ms`.

### Message object
| Field | Type | Notes |
|---|---|---|
| `sender_name` | str | Display name (or encrypted token at Level-1) |
| `timestamp_ms` | int | Unix epoch in **milliseconds** |
| `content` | str | Text body (may be absent) |
| `reactions` | list\<reaction\> | May be absent |
| `type` | str | e.g. `"Generic"`, `"Share"` |
| other fields | varies | Photos, stickers, gifs, shares, etc. |

### Reaction object
```json
{ "reaction": "😆", "actor": "Name or token" }
```

---

## Encoded file

`Data/EncodedData/messages_l2.enc` — a **two-layer Fernet-encrypted** blob.

### Layer 1 (inner, name privacy)
- Password: Level-1
- Salt: `b'philosophy_group_salt_l1'`, PBKDF2-HMAC-SHA256, 200 000 iterations
- Every occurrence of every participant name in the entire JSON is replaced with a stable Fernet-encrypted token (`gAAAAA...`).
- Lookup table stored in the JSON under `_name_tokens`: `{original_name: token}`.

### Layer 2 (outer, file privacy)
- Password: Level-2
- Salt: `b'philosophy_group_salt_l2'`, same KDF
- Encrypts the full Level-1 JSON blob (UTF-8 bytes).

### Decrypted dict shape
```
{
  "participants":  [ {"name": "<token or name>"}, ... ],
  "messages":      [ <message>, ... ],
  "_name_tokens":  { "<original_name>": "<fernet_token>", ... }
}
```
After Level-1 decryption all tokens are replaced back with plain names everywhere in the dict.

---

## Display name masking (`messages_analyst.ipynb`)

`display_name(name)` returns the real name only if:
- The name starts with `gAAAAA` (still an encrypted token — Level-1 was skipped), **or**
- The name is in `DISPLAY_WHITELIST`.

Otherwise it returns `***`.
