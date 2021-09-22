# Processing AnyBody Models with GitHub actions

This is a "proof of concept" to enble processing and testing of AnyBody models on freely available compute resources in the cloud. This repository contains a simple toy model, which is automatically run/tested every time a new commit is pushed to the repository. 

This workflow uses GitHub actions to run AnyBody on GitHub's linux machines in the cloud. The only requirement is a 'floating' license for AnyBody. 

> **Note:** This is a *work in progress*. If you happen to need this kind of setup, please contact us on sales@anybodytech.com

The cloud workflow is defined in the [.github/workflows/anybodycon.yml file](.github/workflows/anybodycon.yml). 

To use AnyBody in GitHub actions you must specify our linux docker `container:` with AnyBody preinstalled:

```yaml
jobs:
  job-with-anybody:
    runs-on: ubuntu-latest
    container: ghcr.io/anybody/anybodycon-linux:latest
    
    steps:
      - uses: actions/checkout@v2
      - run: anybodycon -m macrofile.anymcr          
```

AnyBody can only run if you have a valid license. It requires a floating license and license server which is exposed to the internet. 

The license server's port and address is specified with an environment variable in the action yaml file: 

```yaml
env:
  RLM_LICENSE: <port>@<adress>
```

When the license server is publicly available on the internet, it must be secured with a password. 

The license server password is also defined with an environment variable (`RLM_LICENSE_PASSWORD`), but to prevent anyone from obtaining the password it should be stored with [GitHub's system for encrypted screets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).

With that in place the license server password can then be specified in the steps that launch AnyBody: 

```
  - name: Run AnyBodyCon
    env:
      RLM_LICENSE_PASSWORD: ${{ secrets.LICENSE_PASSWORD }}
    run: |
      anybodycon -m macrofile.anymcr
```

Github actions is free for public repositories, and [has a free teir for private repositories](https://github.com/features/actions#pricing-details). But the purpose of this setup is not to process very large datasets. But i allows you to run normal models and setup automatic tests of your models which are hosted on GitHub. 


