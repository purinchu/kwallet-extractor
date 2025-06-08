# Synopsis

    ./kwallet-extractor.py /path/to/kwallet/export.xml

# Description

This reads in exported (and unencrypted!) XML exports of KWallet wallets, and
outputs the entries with one entry per line, suitable for outputting to a CSV
file or searching for a specific wallet entry.

To get an unencrypted XML export, you have use the `kwalletmanager5` command
(even for Qt6-based Plasma desktops, apparently), select the desired wallet,
and use the File -> Export as XML... menu option to choose where to save the
export.

No KDE libraries are required as the XML file itself contains the relevant
information.

# Usage

You need a recent Python (3.9 or better, I believe, but I haven't tested that
low).

In general, you run the command, passing the name of the XML file with the exported KWallet information,
and the script will print out the entries from the file.

## Showing Google Chrome data (if present)

Note that by default that Google Chrome form data entries that may be present
are not output, as the historical form data format uses a binary format that is
not easily representable.

However, if you wish to output absolutely all possible data, you can
pass the `--show-chrome-form-data` command to do so.  You can use `base64 -d` to decode
the KWallet-stored text based into the Chrome binary format (make sure to redirect this to a file!).

Tools like xxd or hexdump can be used to piece through the binary data,
usernames and passwords will not be hard to piece out.

## Showing non-password fields

There is also a `--all-fields` option which can be used to show entry types
other than KWallet's "Password" folder, since some programs used other
categories for these.

# CSV Output Format

The resulting CSV is generated with a header row and then a line for each entry.

Each line will contain the following fields:

* folder: The folder from within the KWallet that the entry was extracted from.
  There is likely to be a `Passwords` and `Personal` folder, but applications
  can (and do) also create their own folders.
* type: Reflects how the underlying XML was stored.  Will be either `password`
  (usually just a simple value with an attached name) or `map` (which can
  contain several name / value pairs and is decoded differently).  KWallet does
  support other XML fields like `<stream>` but they are not decoded yet.
* username: The username (or email) that was auto-detected. If not
  auto-detected it will be empty.
* website: The website that was auto-detected. If not auto-detected, it will be
  empty.
* other: Any remaining data that could not be assigned elsewhere.
* password: The sensitive password or credential, in plaintext. If not detected
  it will be empty.
