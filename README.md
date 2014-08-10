Torrent BEncode
===============
PHP Library for decoding and encoding BitTorrent BEncoded data.

Bencode (pronounced like B encode) is the encoding used by the peer-to-peer file sharing system BitTorrent for storing and transmitting loosely structured data.

It supports four different types of values:
* byte strings,
* integers,
* lists, and
* dictionaries (associative arrays).

Bencoding is most commonly used in torrent files. These metadata files are simply bencoded dictionaries.

While less efficient than a pure binary encoding, bencoding is simple and (because numbers are encoded as text in decimal notation) is unaffected by endianness, which is important for a cross-platform application like BitTorrent. It is also fairly flexible, as long as applications ignore unexpected dictionary keys, so that new ones can be added without creating incompatibilities.