Torrent BEncode
===============
PHP Library for decoding and encoding BitTorrent BEncoded data.

What is BEncode?
---------------
Bencode (pronounced like B encode) is the encoding used by the peer-to-peer file sharing system BitTorrent for storing and transmitting loosely structured data.

It supports four different types of values:
* byte strings,
* integers,
* lists, and
* dictionaries (associative arrays).

Bencoding is most commonly used in torrent files. These metadata files are simply bencoded dictionaries.

While less efficient than a pure binary encoding, bencoding is simple and (because numbers are encoded as text in decimal notation) is unaffected by endianness, which is important for a cross-platform application like BitTorrent. It is also fairly flexible, as long as applications ignore unexpected dictionary keys, so that new ones can be added without creating incompatibilities.

##Requirements

	Apache or Nginx = Latest production version
	PHP >= 5.3.0

##Installation

Add this package in your composer.json and update composer.
```
{
	"require": {
		"bhutanio/torrent-bencode": "dev-master"
	}
}
```
__Update Composer__

```composer update``` or ```php composer.phar update```

##Usage

###With Composer

```php
	
	require 'vendor/autoload.php';

	$bcoder = new Bhutanio\BEncode;
	$bcoder->set([
		'announce'=>'http://www.private-tracker.com',
		'comment'=>'Downloaded from Private Tracker',
		'created_by'=>'PrivateTracker v1.0'
	]);

	// decode Torrent file
	$torrent = $bcoder->bdecode( File::get('AwesomeMovie.torrent'));
	print_r($torrent);

	// show Torrent contents
	$files = $bcoder->filelist( $torrent );
	print_r($files);
	
	// make Torrent private
	$torrent = $bcoder->make_private($torrent);
	print_r($torrent);

	$infohash = sha1($bcoder->bencode($torrent["info"]));
	$binhash = pack("H*", $bcoder->bencode($torrent["info"])));

```

###Without Composer
```php

	require 'path/to/BEncode.php';

	// use above example

```

##Functions

```php
/**
 * Data Setter
 * @param array $data [array of public variables]
 * eg:
 *  $bcoder = new \Bhutanio\BEncode;
 * 	$bcoder->set([
 *		'announce'=>'http://www.example.com',
 *		'comment'=>'Downloaded from example.com',
 *		'created_by'=>'TorrentSite v1.0'
 *	]);
 */
public function set($data=array()) {}

/**
 * Decode a torrent file into Bencoded data
 * @param  string  $s 	[link to torrent file]
 * @param  integer $pos [file position pointer]
 * @return array/null 	[Array of Bencoded data]
 * eg:
 * 		$bcoder = new \Bhutanio\BEncode;
 * 		$torrent = $bcoder->bdecode( File::get('MyAwesomeTorrent.torrent'));
 *  	var_dump($torrent);
 */
public function bdecode($s, &$pos=0) {}

/**
 * Created Torrent file from Bencoded data
 * @param  array $d [array data of a decoded torrent file]
 * @return string 	[data can be downloaded as torrent]
 */
public function bencode(&$d) {}

/**
 * Decode a torrent file into Bencoded data
 * @param  string $filename 	[File Path]
 * @return array/null 			[Array of Bencoded data]
 */
public function bdecode_file($filename) {}

/**
 * Generate list of files in a torrent
 * @param  array $data 	[array data of a decoded torrent file]
 * @return array 		[list of files in an array]
 */
public function filelist($data) {}

/**
 * Replace array data on Decoded torrent data so that it can be bencoded into a private torrent file.
 * Provide the custom data using $this->set();
 * @param  array $data 	[array data of a decoded torrent file]
 * @return array 		[array data for torrent file]
 */
public function make_private($data) {}
```
