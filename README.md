# OpenStreetMap   Data   Case   Study 

## Problems   encountered   in   the   map
Initially   I   downloaded   a   small   sample   of   the   Ho   Chi   Minh   City   area,   and   then   running   it   against   a provisional   data.py   file.   Later   on,   after   importing   the   data   into   SQL,   I   found   several   problems with   the   data.   Below   is   some   of   them.

* k   and   value   don't   match   in   meaning
* Inconsistency   in   using   k='street'   and   k='route'   to   describe   the   street   name
* Inconsistency   in   using   k='name'   to   convey   street   name,   while   having   k='street'
* Duplicated   values   between   k='name'   and   k='name:vi'   because   they   are   for   the   most   part
both   in   Vietnamese

Due   to   the   limitation   of   time,   I   only   cleaned   2   of   the   above   problems,   which   are   the inconsistency   problems.

### Street   and   Route   Inconsistency
As   mentioned   above,   the   dataset   has   k   as   street   and   route   to   both   describe   street   names,   so this   will   create   inconsistency   when   later   on   we   perform   analysis   on   it.   Therefore,   I   wrote   a   small function   in   audit.py   to   make   sure   that   my   observation   in   SQL   about   this   problem   was   right,   and after   running   it   in   audit.py,   I   used   my   advantage   in   Vietnamese   to   confirm   again   that   they   were both   describing   the   same   thing.   Then,   I   wrote   a   small   function   call   update_route   in   data.py   to   fix the   problem   before   importing   the   data   into   csv   files.

```python
def update_route(tag):
    tag.attrib['k'] = 'street'
```
After   updating,   all   the   k='route'   will   be   turned   into   k='street' 

### Street   and   Name   Inconsistency
As   above,   I   used   a   function   in   audit.py   to   confirm   my   observation   about   the   mixture   of   actual names   and   street   names.   Again,   I   used   Vietnamese   as   an   advantage   to   clean   this inconsistency. In Vietnamese, we mostly only have 2 ways to call street, which are "Đường" and "Hẻm" at the beginning of a phrase. And I audited the data using audit.py to see if there's any other way. There seemed none, but I got to collect variations of “Đường" and “Hẻm", and used them   in   my   update   function   to   clean   the   data.   Therefore,   I   went   ahead   and   used   the   function below   in   data.py   to   clean   the   data   before   importing   to   a   csv   file.
```python
expected = ["Duong","duong", "đường", "Đường", 'DUONG', "Hẻm", "hẻm",'Hem','HẺM']

def is_street(string):
    for i in expected:
        if i in string:
            return True
        
def update_street(tag):
    if is_street(tag.attrib['v']):
        tag.attrib['k'] = 'street'
```
After running this function, any k='name' that had a variation of "Hẻm" or "Đường" would be turned   into   k='street'

## Overview   of   the   data
This   section   contains   basic   statistics   about   the   dataset,   and   I   used   SQL   queries   in   jupyter
notebook   to   gather   them,   except   for   the   file   size   section.

### File size
![alt text] (https://github.com/hanhbuical/datawrangling/blob/master/Screen%20Shot%202017-09-22%20at%2011.10.16%20PM.png)


```python
```

```python
```

```python
```

```python
```

```python
```

```python
```
