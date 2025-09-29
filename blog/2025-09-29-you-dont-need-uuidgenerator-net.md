---
tags: ["en", "linux"]
---

# You Don't Need uuidgenerator.net

You know you can generate a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) easily?

Just execute this:

```sh
uuidgen
```

or if you want a version 7 UUID:

```sh
uuidgen -7
```

voilà! Now you have a UUID in your stdout!

> [!NOTE]
> In NixOS, `uuidgen` is part of the [`util-linux`](https://search.nixos.org/packages?channel=25.05&show=util-linux&query=util-linux) package, so it should be pre-installed in most Linux distribution

You can even try:

```sh
cat /proc/sys/kernel/random/uuid
```

this also give you a v4 UUID.

In the past, when I needed a UUID for testing, I would search for `uuid` and click the first search result, [www.uuidgenerator.net](https://www.uuidgenerator.net), and copy the UUID, then close the site. I feel so dumb for doing that since I discovered this...

> This blog is copy from my old [Reddit post](https://www.reddit.com/r/linux/comments/1m8syx4/i_just_found_out_procsyskernelrandomuuid_and)
