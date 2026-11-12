# Buildable Mautic service for Kubernetes on Wodby

Run Mautic with your own plugins and themes on Kubernetes with Wodby.

This repository defines the Wodby service manifest for Mautic built from your
Git repository: a Mautic Composer project whose `composer.lock` pins Mautic and
its plugins and themes. The build installs it on
[`wodby/mautic-php`](https://github.com/wodby/mautic-php), which brings PHP,
Apache and Wodby's setup for Mautic.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `mautic-buildable` |
| Based on | [`mautic`](https://github.com/wodby/service-mautic): links, settings, volumes, workers, actions and resources |
| Versions | PHP `8.4` |
| Type | Application service |
| Image | Built from your repository on top of [`wodby/mautic-php`](https://github.com/wodby/mautic-php) |
| Application build | Your Mautic Composer project; start from the [Mautic boilerplate](https://github.com/wodby/mautic-boilerplate) |
| Operations | Installs or updates Mautic and registers your plugins after every deployment |

To have Wodby keep Mautic up to date instead, use the [Mautic service](https://github.com/wodby/service-mautic): it
needs no repository or build.

## Use this service

Reference `mautic-buildable` from a Wodby stack. A service is a reusable component
and does not deploy by itself. The stack defines its links, settings, versions,
resources, and relationship to the rest of the application.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Validate the manifest with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
