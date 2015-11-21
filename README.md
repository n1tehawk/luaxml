[![License](http://img.shields.io/badge/License-MIT-green.svg)](#luaxml-license)
[![Build status](https://ci.appveyor.com/api/projects/status/d93a5k3b1xa8sb3r?svg=true)](https://ci.appveyor.com/project/n1tehawk/luaxml)
[![Build Status](https://travis-ci.org/n1tehawk/LuaXML.svg?branch=master)](https://travis-ci.org/n1tehawk/LuaXML)

## LuaXML

- A module that maps between Lua and XML without much ado
- Version 1.8.0 (Lua 5.2), 2013-06-10 by Gerald Franz, _eludi.net_

LuaXML provides a minimal set of functions for the processing of XML data in Lua. It offers a 
very simple and natural mapping between the XML data format and Lua tables, which 
allows one to parse XML data just using Lua's normal table access and iteration 
methods:

Substatements and text content is represented as array data having 
numerical keys, attributes and tags use string keys. This representation makes 
sure that the structure of XML data is exactly preserved in a read/write cycle. 

Since version 1.7, LuaXML consists of a well-optimized portable ISO-standard C file 
and a small Lua file. It is published under the same liberal licensing conditions as Lua 
itself (see below). It has been successfully compiled and used under Linux, various 
flavours of MS Windows, and MacOS X.



### Example

```lua
-- import the LuaXML module
xml = require("LuaXML")
-- load XML data from file "test.xml" into local table xfile
local xfile = xml.load("test.xml")
-- search for substatement having the tag "scene"
local xscene = xfile:find("scene")
-- if this substatement is found...
if xscene ~= nil then
  --  ...print it to screen
  print(xscene)
  --  print attribute id and first substatement
  print( xscene.id, xscene[1] )
  -- set attribute id
  xscene["id"] = "newId"
end

-- create a new XML object and set its tag to "root"
local x = xml.new("root")
-- append a new subordinate XML object, set its tag to "child", and its content to 123
x:append("child")[1] = 123
print(x)
```


### Documentation

LuaXML consists of the following functions:

**xml = require("LuaXML")**
<br>imports the LuaXML module as global object "xml". 
<br>LuaXML consists of a lua file (`LuaXML.lua`) and normally a shared library 
(`.dll`/`.so`), although a static linking is possible as well. Both parts 
are imported by this call, provided that they are found in Lua's package search path.

**function xml.new(arg)**
<br>creates a new LuaXML object.
* param `arg` (optional), _(1)_ a table to be converted to be converted to a 
LuaXML object, or _(2)_ the tag of the new LuaXML object
<br>Note that it is not mandatory to use this function in order to treat a Lua 
table as LuaXML object. Setting the metatable just allows the usage of a more 
object-oriented syntax (e.g. `xmlvar:str()` instead of `xml.str(xmlvar)`).
XML objects created by `xml.load()` or `xml.eval()` automatically offer the
object-oriented syntax.
* Returns new LuaXML object

**function xml.append(var, tag)**
<br>appends a new subordinate LuaXML object to an existing one, optionally sets tag.
* param `var` the parent LuaXML object
* param `tag` (optional) the tag of the appended LuaXML object
* Returns appended LuaXML object or `nil` in case of error

**function xml.load(filename)**
<br>loads XML data from a file and returns it as table
* param `filename` the name and path of the file to be loaded
* Returns a Lua table containing the XML data in case of success or nil.

**function xml.save(var, filename)**
<br>saves a Lua var as XML file.
* param `var`, the variable to be saved, normally a table
* param `filename` the filename to be used. An existing file of the same name gets overwritten.

**function xml.eval(xmlstring)**
<br>converts an XML string to a Lua table
* param `xmlstring` the string to be converted
* Returns a Lua table containing the XML data in case of success or nil.

**function xml.tag(var, tag)**
<br>sets or returns tag of a LuaXML object. This method is just "syntactic sugar" (using a typical Lua term)
that allows the writing of clearer code. LuaXML stores the tag value of an XML statement at table
index 0, hence it can be simply accessed or altered by `var[0]` or `var[xml.TAG]` (the latter is just a 
symbolic name for the value 0). However, writing `var:tag()` for access or `var:tag("newTag")` for altering 
may be more self explanatory.
* param `var`, the variable whose tag should be accessed, a LuaXML object
* param `tag` (optional) the new tag to be set.
* Returns the current tag as string

**function xml.str(var, indent, tag)**
<br>converts any Lua var to an XML string.
* param `var`, the variable to be converted, normally a table
* param `indent` (optional) the current level of indentation for pretty output. Mainly for internal use.
* param `tag`    (optional) the tag to be used for a table without tag. Mainly for internal use.
* Returns an XML string in case of success or nil.

**function xml.find(var, tag, attributeKey, attributeValue)**
<br>recursively parses a Lua table for a substatement fitting to the provided tag and attribute
* param `var`, the table to be searched in.
* param `tag` (optional) the xml tag to be found.
* param `attributeKey` (optional) the exact attribute to be found.
* param `attributeValue` (optional) the attribute value to be found.
* Returns the first (sub-)table which matches the search condition or nil.

**function xml.registerCode(decoded, encoded)**
<br>registers a custom code for the conversion between non-standard characters and XML character entities
* param `decoded` the character (sequence) to be used within Lua.
* param `encoded` the character entity to be used in XML.
* By default, only the most basic entities are known to LuaXML (`" & < > '`). 
<br>ANSI codes above 127 are directly converted to the XML character codes 
of the same number. If more character codes are needed, they can be 
registered using this function.



### LuaXML License

LuaXML is licensed under the terms of the MIT license reproduced below,
the same as Lua itself. This means that LuaXML is free software and can be
used for both academic and commercial purposes at absolutely no cost.

Copyright (C) 2007-2013 by Gerald Franz, eludi.net

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
