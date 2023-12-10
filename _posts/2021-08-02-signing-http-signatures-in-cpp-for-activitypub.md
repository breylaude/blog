---
layout: post
title: Signing HTTP Signatures in C++ (for ActivityPub)
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [c++]
comments: true
---

{: .box-note}
DISCLAIMER: Consult a cryptographer if you want to use the following code in production.

![](/assets/img/http-signature.png)

I’m attempting to implement a basic **ActivityPub server** following [Mastodon’s tutorial](https://blog.joinmastodon.org/2018/06/how-to-implement-a-basic-activitypub-server/). It’s easy to follow and and explains everything clearly. However, I got stock at the last step signing the HTTP signature.

The code looks like this in their official example:

First generate a RSA keypair

```bash
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```

Then sign the HTTP signature using OpenSSL (RSA with SHA256)

```c
require 'openssl'
require 'base64'
 
keypair       = OpenSSL::PKey::RSA.new(File.read('private.pem'))
signed_string = "(request-target): post /inbox\nhost: mastodon.social\ndate: the_date"
signature     = Base64.strict_encode64(keypair.sign(OpenSSL::Digest::SHA256.new, signed_string))
 
puts signature
```

I don't think I should use the C API provided by OpenSSL for my project. It's just too laborious. I also don't want to employ a third-party C++ wrapper that its maintainer might at any time stop supporting. As a result, I resorted to employing the C++ cryptography library **Botan**, which I had previously utilized for other projects.

First, important formats. The **PEM** (Private Encrypted Email) format is used to store the content created above. However, **PEM keys** need to be changed to PKCS8. Despite supporting it, **Botan** doesn't seem to want to load the particular PEM files for some reason.

```bash
openssl pkcs8 -topk8 -inform PEM -outform DER -in private.pem -out private.pkcs8 -nocrypt
```

In order to sign the message, instructing **Botan** to load the newly generated key that is kept in **PKCS8**. Next, make a singer with the same **SHA-256** (OpenSSL on Ruby appears to use EMSA3 by default).

```c
std::string data = "(request-target): post /inbox\nhost: mastodon.social\ndate: the_date";
 
Botan::System_RNG rng;
Botan::Private_Key* private_key = Botan::PKCS8::load_key("private.pkcs8", rng);
Botan::PK_Signer signer(*private_key, rng, "EMSA3(SHA-256)");
signer.update(data);
 
auto signicture = Botan::base64_encode(signer.signature(rng));
std::cout << signicture << "\n";
```

{: .box-note}
Both programs generate the same output.

```ruby
❯ ruby test.rb
jpCO8Oe37MjXMHsuzb7olftsfSEeUtjT1xxUSBgJCqm7XoWccoIOuR2DUrEOWujqfOQdNnVfiQhx5Co3hljXjeBT8V+dOtKcqknX/t0d+XDwgp2EtkNipIRqrghji4YLI95Xx+zDyrfk5gEm7jGwO9qBg48s6pQkVmfEPCVol56PlycyaYjGW+kq0LjVZCw538Gk8xjo8p8tLdbBG7/mX2wY19Pa0BHh9TNEV32Iw9OHC0EanCTgBqnv1Zj34wX2SXgVHfrsHst58UgFRAXSh5I4Bk6glLltiqijJMSo3rJvT8RJZqX3eJ4LbgiN4P57ExmO4hEZozX0v/eHwQ5k1A==
❯ c++ test.cpp -o test -I/usr/include/botan-2/ -lbotan-2 && ./test
jpCO8Oe37MjXMHsuzb7olftsfSEeUtjT1xxUSBgJCqm7XoWccoIOuR2DUrEOWujqfOQdNnVfiQhx5Co3hljXjeBT8V+dOtKcqknX/t0d+XDwgp2EtkNipIRqrghji4YLI95Xx+zDyrfk5gEm7jGwO9qBg48s6pQkVmfEPCVol56PlycyaYjGW+kq0LjVZCw538Gk8xjo8p8tLdbBG7/mX2wY19Pa0BHh9TNEV32Iw9OHC0EanCTgBqnv1Zj34wX2SXgVHfrsHst58UgFRAXSh5I4Bk6glLltiqijJMSo3rJvT8RJZqX3eJ4LbgiN4P57ExmO4hEZozX0v/eHwQ5k1A==
```

Going back to getting my **ActivityPub server** working.
