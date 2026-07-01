# La Suite numérique with Helm Charts

Services operated by LaSuite are deployed to a Kubernetes cluster and packaged
as Helm charts.

## Publishing Helm charts

Helm charts are published using GitHub pages in the project's repository.
Publication is achived using [our
fork](https://github.com/numerique-gouv/helm-gh-pages) of the
`stefanprodan/helm-gh-pages` GitHub Action:

```yaml
name: Release Chart

# [...]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      # [...]
      - name: Publish Helm charts
        uses: numerique-gouv/helm-gh-pages@2cf477ae49d7c70037ceb1685803f4f7bad9b981 # add-overwrite-option
        with:
          charts_dir: ./src/helm
          linting: on
          token: ${{ secrets.GITHUB_TOKEN }}

```

💡 Note that you should create the `gh-pages` branch to be able to make your
first Helm Chart release or else the job will fail. To do so, you can use the
following Git commands:

```sh
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "🚀(project) create Helm chart repository"
git push origin gh-pages
```

Now, you need to make sure GitHub pages are active for your repository and
deployed from the `gh-pages` branch (`Settings` > `Pages` > `Deploy from a
branch` and set the branch value to `gh-pages`).


## Handling secrets encryption using `sops`

We use [SOPS](https://getsops.io/) to encrypt secrets used in our Helm charts.
It should be installed on your system along with
[age](https://github.com/FiloSottile/age) and the [age Yubikey
plugin](https://github.com/str4d/age-plugin-yubikey). 

### Configure SOPS

Age keys are used to encrypt data, if you haven't generated a key yet, this can
be achieved using the `age-keygen` command:

```sh
age-keygen -o ${HOME}/.config/sops/age/keys.txt
```

If you edit your age key file (`${HOME}/.config/sops/age/keys.txt`), you will
find the public key that you should declare in your deployment project's SOPS
configuration file:

```yaml 
creation_rules:
  - path_regex: env.d/staging
    key_groups:
    - age: 
      - agerewrewrllplgrepgokg34kf034fkdqkdfledkf03dwqdqwdldwpdlq[d9d0 # johndoe
```

### Encrypt secrets

If you need to encrypt a new secret from a clear text file, it needs to be
encrypted using:

```bash
sops encrypt env.d/staging/secrets.yaml > env.d/staging/secrets.enc.yaml

# Remove original (clear text) file
rm env.d/staging/secrets.yaml
```

And then when you need to modify (or create) a secret, use:

```bash
sops edit env.d/staging/secrets.env.yaml
```

### Rotate encryption keys

If you add new contributors that should be able to read or update Helm Chart
secrets, you should edit the `.sops.yaml` configuration file for your project
and then you can use the `updatekeys` command to encrypt secrets using those
new keys / recipients: 

```bash
# Update all encrypted files using new keys
find . -name "*.enc.*" -exec sops updatekeys -y '{}' \;
```
