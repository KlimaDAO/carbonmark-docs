# Versioning & Release Process

## Versioning and breaking changes

When consuming our API, **be sure to prefix the API URL with a version number**. For example, the base URL of version 1 would be `v1.api.carbonmark.com`.

Any changes that we make to an API version are guaranteed to be backwards compatible. However, if you omit the version prefix ([`api.carbonmark.com`](http://api.carbonmark.com)) **your application will be exposed to breaking changes** because [api.carbonmark.com](http://api.carbonmark.com) is always routed to the latest major version.

To find the latest version of the API, visit [quickstart.md](../quickstart.md "mention")or view the REST reference docs at [api.carbonmark.com](http://api.carbonmark.com) (in your web browser) with the version prefix omitted. Likewise you can find the REST reference docs for previous API versions by navigating to the base URL for that version.

## Deprecation policy

When a new version of the Carbonmark API is released, we consider all previous versions to be _deprecated_ from that date forward. We can only guarantee that a deprecated version will stay available to existing users for three months. After the three month deprecation period the version is considered _retired_.

Retired versions are no longer supported, and are likely to become unstable or be removed completely.

Deprecated versions will be maintained to ensure stability and availability, but they will not receive new features or major improvements.

In the future we plan to significantly extend the duration of this deprecation period, but at the moment the API is rapidly evolving to support new carbon assets and features that are not always backwards-compatible, so we have elected for a shorter 3 month deprecation period.

## Canary versions

Occasionally, we will give a customer early access to a feature or improvements via unreleased “canary” or “staging” URLs.

Please note that these URLs do not include guarantees for stability, compatibility or long-term support and may become unstable within weeks or months. Users are discouraged from sharing these URLs externally, and are encouraged to migrate to the next major version as soon as it is available.

## Upgrading

See individual version pages for release notes and other pertinent upgrade information.
