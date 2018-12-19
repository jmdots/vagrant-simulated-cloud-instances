# vagrant-simulated-cloud-instances

Vagrantfiles for VirtualBox that simulate public cloud instance types for local
prototyping.

## Instance Types

| Cloud | Type | CPU (Cores) | CPU Cap (%) | Memory (MB) | Image | Vagrantfile |
| ----- | ---- | ----------- | ----------- | ----------- | ----- | ----------- |
| Google | f1-micro | 1 | 20 | 600 | ubuntu-minimal-1804-lts | [link](google/f1-micro/ubuntu-minimal-1804-lts/Vagrantfile) |

## FYI

### How to spin up a simulated instance?

Example:
```
## Choose what you want to simulate.
cd google/f1-micro/ubuntu-minimal-1804-lts

## Spin up a simulated instance.
vagrant up

## SSH into it.
vagrant ssh
```

### Why do this?

I'm working on a project right now that I'd love to eventually run inside
Google's [Free Tier's][google-free-tier] Always Free one `f1-micro` instance.
Since I'm fresh out of free credits and something else needs to run on that
`f1-micro` today, I decided to try simulating an `f1-micro` instance locally.

This project is the byproduct of that effort.

### Nonequivalency

The computer where you use this repo is probably not completely capable of 
completely simulating all aspects of the exact cloud instance type you want
to simulate.

There are several reasons for this, but here are just a few likely deltas
between your host and that of any given cloud instance type.

- Storage
- CPU boost
- CPU cache
- CPU clock speed
- CPU generation
- CPU manufacturer
- RAM generation
- RAM speed
- The fact that you're probably running a GUI or some other processes on the
  same cores that will back any Vagrant / VirtualBox VM you create using this
  repo.
- *etc.*

The goal of this repo is to only **try to approximate** what happens in reality.
Keep this in mind as you use this repo and don't draw any conclusions without
comparing with the real thing yourself.

In the future, perhaps we can use some benchmarking method to better address
some of these deltas.

### CPU Bursting (not simulated)

CPU bursting is not simulated here. Where bursting occurs in a real upstream
cloud instance, its instance simuation in this repo caps CPU share at the
"before bursting" amount.

For example, Google's [f1-micro][google-shared-machine-types] is currently
advertised as:

> Micro machine type with 0.2 vCPU, 0.60 GB of memory, backed by a shared
> physical core.

And in this repo `f1-micro` is represented as having 1 CPU, 600MB of memory
with a [CPU execution cap][cpu-execution-cap] of 20%.

### What differs at the distro and software level?

At this time, the instance's distro and the software therein is only very
loosely simulated. That is, the distro names and major version matches,
but any other details may well differ in this repo versus the real thing.

And I'm okay with that for now. (Help Wanted) :-)

## Credits

This project is based on these excellent projects:

- [chef/bento][chef/bento]
- [hashicorp/vagrant][hashicorp/vagrant]
- [virtualbox][virtualbox]

## License

```
Copyright 2018, Joshua Mark Dotson (<vulariter-git@yahoo.com>)
Copyright 2012-2017, Chef Software, Inc. (<legal@chef.io>)
Copyright 2011-2012, Tim Dysinger (<tim@dysinger.net>)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

<!--- Hidden Reference Links -->

[chef/bento]: https://github.com/chef/bento
[hashicorp/vagrant]: https://github.com/hashicorp/vagrant
[cpu-execution-cap]: https://github.com/hashicorp/vagrant/issues/625
[google-free-tier]: https://cloud.google.com/free/docs/gcp-free-tier#always-free-usage-limits
[google-shared-machine-types]: https://cloud.google.com/compute/docs/machine-types#sharedcore
[virtualbox]: https://www.virtualbox.org/
