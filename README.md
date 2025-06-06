# Synopsis

    uv run main.py /path/to/kwallet/export.xml

# Description

This doesn't do anything yet. But the plan is that it will take the
KWallet-encoded XML passwords etc. and dump them straight to stdout so that you
can put the credentials into a file or similar.  This can allow you to get your
credentials out of KWallet (as long as you already have an XML export!) even
without the KDE libraries, or export KWallet credentials into other secret
storage managers.
