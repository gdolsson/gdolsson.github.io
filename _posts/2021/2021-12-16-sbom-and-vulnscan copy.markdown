---
layout: "post"
title: "SBOM generation and vulnerbillity scanning with syft and gripe"
date: "2021-12-16 09:40"
---
[Syft](https://github.com/anchore/syft) is a CLI tool and Go library for generating a Software Bill of Materials (SBOM) from container images and filesystems. Exceptional for vulnerability detection when used with a scanner tool like [Grype](https://github.com/anchore/grype).

[Grype](https://github.com/anchore/grype) is a vulnerability scanner for container images and filesystems.