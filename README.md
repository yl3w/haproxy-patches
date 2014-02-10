haproxy-patches
===============

Includes patches to haproxy that were not included as part of the distribution. 
See the corresponding haproxy repos (1.4/1.5)-dev for development of these patches.

The patches were developed either on 1.4 and/or on 1.5. Some functionality available
in patches for 1.4 have been included in 1.5. Please make sure the read the patch
documentation available in the __README__ of the corresponding patch folder to
determine if you need th patch.

The repos haproxy-1.4 and haproxy-1.5 are mirrors to the respective haproxy version.
The repos haproxy-1.4-dev and haproxy-1.5-dev include in them branches on which the
patches have been developed. Also in these repo is a branch **patched** which include
the source code with all the patches applied.

If you are only looking for source with the patches applied you can get it from these
repos. If you are looking to apply the patched yourself you can obtain the patches
from this repo for the corresponding version.

Each folder corresponding to the version also provides "sequenced" patches. These
patches can be applied in order starting with the lowest sequence number.

Patches in folder starting with 0001 can be applied independently of the 
remainder of the patches.
