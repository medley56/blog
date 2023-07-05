---
title: "About"
date: 2023-07-04T22:31:32-06:00
draft: false
---

# Reasons for Blogging

This blog is currently a selfish enterprise. If you arrive here and find you have learned something 
or are even interested in something I've written, that's great! I will be glad if I can have a positive impact 
on someone reading my writing. However, I'm a new enough writer that I'm keeping my bar on the floor and 
just trying to write more regularly. 

# Projects

## Space Packet Parser

https://space-packet-parser.readthedocs.io/en/latest/

I spend a lot of time working with data downlinked from spacecraft and satellite borne instruments. 
These days pretty much all of that data is in a binary packet format defined by the Consultative 
Committee on Space Data Systems (CCSDS). I wrote a configurable parsing library to decode CCSDS packet 
data into engineering units. It uses a somewhat esoteric configuration format called XTCE, which is an 
XML representation of the packet format, complete with all kinds of fancy stuff like automatic 
calibrations, enum lookups, conditional packet structure (e.g. 
`if FIELD_A=0, then include FIELD_B, else include FIELD_C`)
