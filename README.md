# Minimal Vagrant installation for SLES11

This repo contains an absolutely minimal, no-frills 3-node
installation of SUSE Linux Enterprise Server 11 SP3 (SLE 11 SP3).

## Host requirements

This setup requires an x86_64 host environment running either Libvirt
or VirtualBox, plus Vagrant 1.6+.

## Vagrant requirements

This installation requires the `hosts` provisioner, which can be
installed with

    vagrant plugin install vagrant-hosts

If you deploy on Libvirt, you will also need the `libvirt` provider:

    vagrant plugin install vagrant-libvirt

## Bringing up the VMs

Simply fire up the three VMs, `alice`, `bob` and `charlie`, with the
following command:

    vagrant up

You may also choose to bring them up selectively:

    vagrant up alice
    vagrant up bob
    vagrant up charlie

You can then connect to either VM with

    vagrant ssh alice
    vagrant ssh bob
    vagrant ssh charlie

## Automating SUSE registration

You can set have the provisioner automatically run `suse_register` on
your behalf, so that your system is immediately eligible for SLES
updates. To do so, in `vagrant.yml` add the following hash:

```yaml
suse_register:
  email: <your registered email>
  regcode-sles: <your SLES 11 registration code>
```

