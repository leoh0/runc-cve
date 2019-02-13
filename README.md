CVE Builds for legacy docker-runc
---------------------------------

This repo provides a backport of patches for CVE-2019-5736 for older versions of runc
that were packaged with Docker.

## Build and Releases

Refer to the [releases](https://github.com/rancher/runc-cve/releases) section of this repo for the binaries. In order to build yourself,
or build for different architectures, just run `make` and the binaries will end up in
`./dist`.

The binaries will be of the form runc-${VERSION}-${ARCHITECTURE} where VERSION is the
associated Docker version, not the version of runc.


## Installing

To install, find the runc for you docker version, for example Docker 17.06.2 for amd64 
will be runc-v17.06.2-amd64.  Then replace the docker-runc on your host with the patched
one.

```bash
# Figure out where your docker-runc is, typically in /usr/bin/docker-runc
which docker-runc

# Backup
mv /usr/bin/docker-runc /usr/bin/docker-runc.orig.$(date -Iseconds)

# Copy file
cp runc-v17.06.2-amd64 /usr/bin/docker-runc

# Ensure it's executable
chmod +x /usr/bin/docker-runc

# Test it works
docker-runc -v
docker run -it --rm ubuntu echo OK
```

## Supporting systemd cgroup

If you see below error, you're using systemd cgroup.

```bash
# docker run --rm ubuntu echo OK
docker: Error response from daemon: OCI runtime create failed: systemd cgroup flag passed, but systemd support for managing cgroups is not available: unknown.
```

Only nonstatic runc build is supporting systemd cgroup, so you need to use nonstatic version

```bash
# Figure out where your docker-runc is, typically in /usr/bin/docker-runc
which docker-runc

# Backup
mv /usr/bin/docker-runc /usr/bin/docker-runc.orig.$(date -Iseconds)

# Copy file
cp runc-v17.06.2-amd64-nonstatic /usr/bin/docker-runc

# Ensure it's executable
chmod +x /usr/bin/docker-runc

# Test it works
docker-runc -v
docker run -it --rm ubuntu echo OK
```