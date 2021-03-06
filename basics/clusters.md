# NAIS clusters

NAIS is a platform spread over two different suppliers. One set of clusters on-premise, and another set running in the
cloud (namely [Google Cloud Platform](../gcp/README.md)).

## On-premise

The on-premise clusters are split into two zones, _selvbetjeningsonen_ (SBS), _fagsystemsonen_ (FSS).

| cluster | ingresses | 
| :--- | :--- |
| dev-fss | nais.preprod.local |
| prod-fss | nais.adeo.no |
| dev-sbs | nais.oera-q.local |
| prod-sbs | nais.oera.no, tjenester.nav.no |

Example: If your app is named `myapp`, then the URL for `dev-fss` would be `https://myapp.nais.preprod.local/`.

{% hint style="info" %}
Remember `https://` when calling on-premise URLs!
{% endhint %}

{% hint style="warning" %}
We are working on moving away from the `preprod` prefix, so use `dev` where possible. Read more about the decision over
at [pig-kubernetes-ops](https://github.com/navikt/pig/blob/master/kubeops/adr/000-preprod-rename.md).
{% endhint %}

## Cloud - Google Cloud Platform \(GCP\)

In the cloud there are no zones. Instead, we rely on a [zero-trust](https://github.com/navikt/pig/blob/master/kubeops/doc/zero-trust.md) model with a service-mesh.
The applications running in GCP needs [access policy rules defined](../gcp/access-policy.md) for every other service they receive requests from or sends requests to.

| cluster | ingresses |
| :--- | :--- |
| dev-gcp | dev-adeo.no, dev-gcp.nais.io, dev-nav.no|
| prod-gcp | adeo.no, prod-gcp.nais.io, nav.no|
| labs-gcp | labs.nais.io |

### The different domains

In GCP we support three different domain names, dependent on what your need is.

* Use **(dev-)adeo.no** if you only need access from "utviklerimage"/VDI, typical use-cases is "saksbehandlere"
* Use **(dev|prod)-gcp.nais.io** or **dev-nav.no** if you need access from laptop (via ScaleFT/NAVTunnel)
* Use **nav.no** if it should be accessible for the world wide web
* Use **(dev-gcp.)navlabs.no** for external demo/testing purposes (only available in dedicated cluster: `labs-gcp`)
