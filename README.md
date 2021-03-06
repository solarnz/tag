# MP3/MP4/FLAC metadata parsing library
[![Build Status](https://travis-ci.org/dhowden/tag.svg?branch=master)](https://travis-ci.org/dhowden/tag)
[![GoDoc](https://godoc.org/github.com/dhowden/tag?status.svg)](https://godoc.org/github.com/dhowden/tag)

This package provides MP3 (ID3v1,2.{2,3,4}) and MP4 (ACC, M4A, ALAC) and FLAC metadata detection, parsing and artwork extraction.

Parsed metadata is exported via a single interface (giving a consistent API for all supported metadata formats).

    // Metadata is an interface which is used to describe metadata retrieved by this package.
    type Metadata interface {
    	Format() Format

    	Title() string
    	Album() string
    	Artist() string
    	AlbumArtist() string
    	Composer() string
    	Year() int

    	Track() (int, int) // Number, Total
    	Disc() (int, int) // Number, Total

    	Picture() *Picture // Artwork

    	Raw() map[string]interface{} // NB: raw tag names are not consistent across formats.
    }

## Audio Data Checksum (SHA1)

This package also provides a metadata-invariant checksum for audio files: only the audio data is used to
construct the checksum.

[http://godoc.org/github.com/dhowden/tag#Sum](http://godoc.org/github.com/dhowden/tag#Sum)

## Example Usage

There are a simple command-line tools which demonstrate basic tag extraction and summing:

    $ go get github.com/dhowden/tag/...
    $ cd $GOPATH/bin
    $ ./tag 11\ High\ Hopes.m4a
    Metadata Format: MP4
     Title: High Hopes
     Album: The Division Bell
     Artist: Pink Floyd
     Composer: Abbey Road Recording Studios/David Gilmour/Polly Samson
     Year: 1994
     Track: 11 of 11
     Disc: 1 of 1
     Picture: Picture{Ext: jpeg, MIMEType: image/jpeg, Type: , Description: , Data.Size: 606109}

    $ ./sum 11\ High\ Hopes.m4a
    2ae208c5f00a1f21f5fac9b7f6e0b8e52c06da29
