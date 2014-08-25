Consider the following scenario: You have a two member replica set, a primary, and a secondary.

The data center with the primary goes down, and is expected to remain down for the foreseeable future. Your secondary is now the only copy of your data, and it is not accepting writes. You want to reconfigure your replica set config to exclude the primary, and allow your secondary to be elected, but you run into trouble. Find out the optional parameter that you'll need, and input it into the box below for your rs.reconfigure(new_cfg, OPTIONAL PARAMETER).

Use "double quotes" around the key(s) in your document.

_Hint: You may want to use this documentation page to solve this problem._

----

{"force" : true}