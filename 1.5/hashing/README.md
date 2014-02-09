DESCRIPTION
===========

This change set implement hashing functions enhancements. It adds hash functions
and makes the use of avalanche logic explicit. These patches are part of 1.5
release and you do not need these if using 1.5.

Enhance hash type directive with algorithm options
--------------------------------------------------

In testing at tumblr, we found that using djb2 hashing instead of the
default sdbm hashing resulted is better workload distribution to our backends.

This commit implements a change, that allows the user to specify the hash
function they want to use. It does not limit itself to consistent hashing
scenarios.

The supported hash functions are sdbm (default), and djb2.

For a discussion of the feature and analysis, see mailing list thread
"Consistent hashing alternative to sdbm" :

      http://marc.info/?l=haproxy&m=138213693909219

Note: This change does NOT make changes to new features, for instance,
applying an avalance hashing always being performed before applying
consistent hashing.

Implement avalanche as modifier
-------------------------------

Avalanche is supported not as a native hashing choice, but a modifier
on the hashing function. Note that this means that possible configs
written after 1.5-dev4 using "hash-type avalanche" will get an informative
error instead. But as discussed on the mailing list it seems nobody ever
used it anyway, so let's fix it before the final 1.5 release.

The default values were selected for backward compatibility with previous
releases, as discussed on the mailing list, which means that the consistent
hashing will still apply the avalanche hash by default when no explicit
algorithm is specified.

<pre>
Examples
  (default) hash-type map-based
	Map based hashing using sdbm without avalanche

  (default) hash-type consistent
	Consistent hashing using sdbm with avalanche

Additional Examples:

  (a) hash-type map-based sdbm
	Same as default for map-based above
  (b) hash-type map-based sdbm avalanche
	Map based hashing using sdbm with avalanche
  (c) hash-type map-based djb2
	Map based hashing using djb2 without avalanche
  (d) hash-type map-based djb2 avalanche
	Map based hashing using djb2 with avalanche
  (e) hash-type consistent sdbm avalanche
	Same as default for consistent above
  (f) hash-type consistent sdbm
	Consistent hashing using sdbm without avalanche
  (g) hash-type consistent djb2
	Consistent hashing using djb2 without avalanche
  (h) hash-type consistent djb2 avalanche
	Consistent hashing using djb2 with avalanche
</pre>

