## Variant Support

The Next-Gen CircleCI Convenience Images support the ability to have "variant" images.
These are variations of a main image, say `cimg/ruby`, that have additional or alternative software for a very common use case.

Traditionally with our legacy images, we've had 5 global variants.
These are variants that apply to almost all of our images.
These variants were:

- `-node`
- `-browsers`
- `-browsers-legacy`
- `-node-browsers`
- `-node-browsers-legacy`

The `-browsers-legacy` variant has unofficially already been deprecated which leaves us with 3 variants to think about for next-gen images.


## The Plan

Moving forward with Next-Gen images, the plan is to carry over the `-node` variant while giving it some additional flexibility.
The `-browsers` variant would be dropped and instead the software it contains would be offered up as one or more service images.

**-node** - For legacy images, this variant installed the current LTS version of Node.js, which changes approximately once a year.
This has worked for many users though some wanted the current version of Node.js instead of the LTS.
The `-node` variant was inflexible here.
For the Next-Gen `-node` variant, the plan is to install NVM and with it, install both the LTS version of Node.js as well as the current version.
While the LTS version will be defaulted to, switching versions would be a single command during a build without the need to download anything extra.
This allows for a direct migration path from the legacy -node variant while providing additional functionality for users wanting a more cutting edge Node.js release.

**-browsers** - This is a legacy variant that won't carry over to next-gen images.
One of the issues with the `-browsers` variant is the speed in which browsers get updated.
For legacy images that get rebuilt daily, this was less of a problem.
For next-gen images which only get rebuilt on demand, we'd have trouble keeping up with browser changes while maintaining the current update procedure which is efficient.
Instead, browsers such as Google Chrome and Firefox as well as their drivers Chromedriver and Gecko specifically can be operated via Selenium remotely over TCP/IP.
This gives us an opportunity to create one or more "browser" service images that the user connects to via their primary image.
This would be similar to how users use service images now such as the PostgreSQL or MariaDB in their build.
This allows us to update browsers at the fast pace they need while giving users full control on which versions they want to use.

We could have a single "browsers" image ( `cimg/browsers` ) that contains everything needed, a more direct replacement for the `-browsers` variant, or an image for each browser ( `cimg/chrome`, `cimg/firefox` ).
The former is more simple while the later allows us to use Docker tags for each specific version of a browser, giving users more direct control of which browser version they want to test with.
