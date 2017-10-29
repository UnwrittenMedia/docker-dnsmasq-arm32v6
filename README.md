# docker-dnsmasq-arm32v6

It's an Alpine based distribution of [dnsmasq][dnsmasq] in a Docker image that works on the Raspberry PI. It is only several MB in size. It is just an `ENTRYPOINT` to the `dnsmasq` binary.

This was inspired by https://github.com/andyshinn/docker-dnsmasq for x86.

## Usage

It is usually a good idea to use a tag other than `latest` if you are using this image in a production setting.

* `unwritten/dnsmasq-arm32v6:latest`: dnsmasq based on Alpine 3.6 for ARMv6 32bit 

[dnsmasq][dnsmasq] requires `NET_ADMIN` capabilities to run correctly. Start it with something like `docker run -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN andyshinn/dnsmasq:2.75`.

The configuration is all handled on the command line (no wrapper scripts here). The `ENTRYPOINT` is `dnsmasq -k` to keep it running in the foreground. If you wanted to send requests for an internal domain (such as Consul) you can forward the requests upstream using something like `docker run -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN andyshinn/dnsmasq:2.75 -S /consul/10.17.0.2`. This will send a request for `redis.service.consul` to `10.17.0.2`

As this is a very barebones entrypoint with just enough to run in the foreground, there is no logging enabled by default. To send logging to stdout you can add `--log-facility=-` as an option.

[dnsmasq]: http://www.thekelleys.org.uk/dnsmasq/doc.html
