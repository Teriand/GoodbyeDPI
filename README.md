GoodbyeDPI — Passive Deep Packet Inspection blocker and Active DPI circumvention utility
=========================

This software designed to bypass Deep Packet Inspection systems found in many Internet Service Providers which block access to certain websites.

It handles DPI connected using optical splitter or port mirroring (**Passive DPI**) which do not block any data but just replying faster than requested destination, and **Active DPI** connected in sequence.

**Windows 7, 8, 8.1 and 10** with administrator privileges required.

# How to use

Download [latest version from Releases page](https://github.com/ValdikSS/GoodbyeDPI/releases) and run.

```
1: block passive DPI, 2: fragment outbound, 4: replace Host with hoSt, 8: remove space between host header and value
```

Try to run `goodbyedpi.exe 13` first. This enables 1, 4 and 8 options (1+4+8 = 13).

Run `goodbyedpi.exe 15` if this doesn't work. Fragmentation will cause slowdowns but it works in most cases.

# How does it work

### Passive DPI

Most Passive DPI send HTTP 301 Redirect if you try to access blocked website over HTTP and TCP Reset in case of HTTPS, faster than destination website. Packets sent by DPI have always have IP Identification field equal to `0x0000` or `0x0001`, as seen with Russian providers. These packets are blocked by GoodbyeDPI.

### Active DPI

Active DPI is more tricky to fool. Currently the software uses 3 methods to circumvent Active DPI:

* TCP-level fragmentation for first data packet
* Replacing `Host` header with `hoSt`
* Removing space between header name and value in `Host` header

These methods do not break any website as are fully compatible with TCP and HTTP standards, yet it's sufficient to prevent DPI data classification and to circumvent censorship.

The program loads WinDivert driver which uses Windows Filtering Platform to set filters and redirect packets to the userspace. It's running as long as console window is visible and terminates when you close the window.

# How to build from source

This project can be build using **GNU Make** and [**mingw**](https://mingw-w64.org). The only dependency is [WinDivert](https://github.com/basil00/Divert).

To build x86 exe run:

`make CPREFIX=i686-w64-mingw32 WINDIVERTHEADERS=/path/to/windivert/include WINDIVERTLIBS=/path/to/windivert/x86`

And for x86_64:

`make CPREFIX=x86_64-w64-mingw32 WINDIVERTHEADERS=/path/to/windivert/include WINDIVERTLIBS=/path/to/windivert/amd64`

# Similar projects

[zapret](https://github.com/bol-van/zapret) by @bol-van (for Linux).

# Kudos

Thanks @basil00 for [WinDivert](https://github.com/basil00/Divert). That's the main part of this program.

Thanks for every [BlockCheck](https://github.com/ValdikSS/blockcheck) contributor. It would be impossible to understand DPI behaviour without this utility.
