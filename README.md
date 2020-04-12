# Mailek — possibly usable email tools

---

**ACHTUNG!**  WORK IN PROGRESS!  
**ACHTUNG!**  SUBJECT TO ANY AND ALL CHANGE!

*Mailek* is a collection of tools, configuration, documentation, and
suggestions for dealing with user email on *Plan 9* systems.  It's a
MUA.

The *Mailek core* tools intend to provide a command programming
interface around functionality exposed by select lower-level
components of *Upas*, the venerable *Plan 9* email system.

Following is a listing of core tools with short descriptions and
pointers to their respective complete documentation:

- `mailek/box(1)` — manipulates accounts and mailboxes,
- `mailek/send(1)` — dispatches outbound mail,
- `mailek/select(1)` — runs queries on mailboxes,
- `mailek/exec(1)` — runs commands on selected emails,
- `mailek/print(1)` — formats components of emails for display,
- `mailek/filters(1)` — automatic, pluggable message transformation.

A suite of user-centric command-line and textual interfaces is
provided alongside and backed by the *core* tools.  These are
distinguished from the core group by the purpose of providing
convenience in day-to-day emailing workflows.  This distinction is
largely arbitrary; all the tools work with the exact same objects, and
— providing we have done a good job — should compose and work together
just as well.  They are listed here:

- `mailek/list(1)` — renders a message list interface,
- `mailek/open(1)` — renders a message interface,
- `mailek/write(1)` — message compose command-interface.

Textual interfaces are implemented with `rio(4)` windows holding
plumbable text glued with simple but effective `plumber(4)` rules and
contextual `rc(1)` shells.  The simplicity of this interface blends
the world of email with user's native environment, letting them
accomplish auxiliary tasks such as text editing, formatting, spell
checking, and similar, with tools and existing workflows they are
familiar with.

All of the above-mentioned programs are supported by a trivial
tree-structured per-user information base storing items ranging from
mailbox and relay access details, to custom display rules and filters.

The database, together with `mailek/box(1)` and (transparently)
`mailek/send(1)`, may prove useful on its own as an easy way to set up
email access, even to those who prefer not to use the whole package.

### Relationship with *Upas* and the existing email tools

As hinted in the introduction the *Mailek* programs hang quite heavily
on several key functions provided by *Upas*, which do all the
**actually heavy** email work.  They are listed here for reference:

- `upas/fs(4)` — *mailbox-as-a-fileserver*, a 10x program,
- `upas/smtp(8)` — dispatches mail towards destination,
- `upas/marshal(8)` — (RFC822) message formatting filter,
- `/bin/myupassend` — is where the *Upas* fuse is blown.

Besides these select executables and their select interfaces no other
part of *Upas* is currently being relied upon or is even expected to
continue working in any way for the (individual) user who installs
*Mailek*.

This could change so stay tuned if it affects you, or better yet
provide suggestions or code to work around it.

#### Background baggage, the so-called "motivation" for all this.

*Mailek* grew out of (author's) frustration of having to deal with a
loosely documented email system with lots of moving parts and
assumptions, whose main strength of being adaptable to complex needs
and environments is very much a burden when all one wants is access
remotely hosted mail and reply to it.

# Installation and setup **WIP**

	mk install

Binding a shorthand path for convenient interactive use is strongly
advised:

	mkdir /bin/m
	bind /bin/mailek /bin/m

NOTE: `/bin/mailek/` *must* exist as installed; this is the path
programs use internally to refer to each other.

## The database

## The `plumber(4)`

Add these rules to your plumbing:

	type is text
	data matches '✉([0-9][0-9]*)/?(.+)?'
	plumb start window mailek/open $1 $2
	
	type is text
	data matches '[^/][a-zA-Z0-9_+.\-~/]+@[a-zA-Z0-9_+.\-]*'
	plumb to sendmail
	plumb start window mailek/write $0

# Examples **WIP**

Example 1. Find and format ten most recent unread emails:

	m/select -10 -unread | m/print id from to subject

Example 2. Delete all messages by youtube.com:

	m/select -is from '@youtube.com$' | m/do rm .

# License

Copyright (c) 2020 kvik@a-b.xyz

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
