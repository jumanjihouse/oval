OVAL vulnerability scan
-----------------------

The Red Hat Security Response Team provides OVAL definitions
for all vulnerabilities (identified by CVE name) that affect RHEL.
This enables users to perform a vulnerability scan and
diagnose whether the system is vulnerable.

This repo contains a script to download the latest
OVAL definitions from Red Hat and perform a vulnerability scan
against a system. If the system has one or more known vulnerabilies,
the script exits non-zero.

* The scan is time-dependent, so it should be run on a daily basis
  (or whenever Red Hat updates the files).

* A vulnerability scan is distinct from a *SCAP secure configuration scan*.

The exact output of the vulnerability scan varies according to the
latest Red Hat OVAL feed, but it looks similar to this snapshot from August 2014:

    -snip copious checks-

    RHSA-2014:1051: flash-plugin security update (Critical)
    oval-com.redhat.rhsa-def-20141051
    CVE-2014-0538
    CVE-2014-0540
    CVE-2014-0541
    CVE-2014-0542
    CVE-2014-0543
    CVE-2014-0544
    CVE-2014-0545
    pass

    RHSA-2014:1052: openssl security update (Moderate)
    oval-com.redhat.rhsa-def-20141052
    CVE-2014-3505
    CVE-2014-3506
    CVE-2014-3507
    CVE-2014-3508
    CVE-2014-3509
    CVE-2014-3510
    CVE-2014-3511
    pass

    RHSA-2014:1053: openssl security update (Moderate)
    oval-com.redhat.rhsa-def-20141053
    CVE-2014-0221
    CVE-2014-3505
    CVE-2014-3506
    CVE-2014-3508
    CVE-2014-3510
    pass

    vulnerability scan exit status 0


Sample usage
------------

Add this repo as a git submodule in a docker repo:

    git submodule add https://github.com/jumanjihouse/oval.git oval
    git submodule update --init

Add the files to your Dockerfile and run a scan during `docker build`:

    FROM ...
    ADD oval /oval
    RUN ...

    # Do this after installing packages, etc.
    # If the image is not patched according to latest feed,
    # image fails to build (so you can remediate it).
    RUN /oval/remediate-oscap.sh
    RUN /oval/oval-vulnerability-scan.sh


Contributing
------------

Please...

* Minimize diff churn to enhance git history commands.
* Use `git rebase upstream/master` to update your branch.
* Provide good commit messages, such as<br/>
  http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
* Work in a topic branch.

See CONTRIBUTING.md in this repo for details.


License
-------

See LICENSE in this repo.


References
----------

* http://www.redhat.com/security/data/metrics/
* https://fedorahosted.org/scap-security-guide/wiki


History
-------

I had originally created these files within
https://github.com/jumanjiman/wormhole
but they're useful enough to use as a git submodule
for other docker images.
