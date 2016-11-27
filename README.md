# elasticsearch-release

## Updating

### Updating the JRE

1. Remove the old openjdk section from `config/blobs.yml`
2. Pull and deploy the latest [`cf-release`](https://github.com/cloudfoundry/cf-release.git)
3. In the the `cf-release` working directory copy the filename and the `sha` attribute from the `openjdk`, `trusty` blob in `config/blobs.yml`
4. Again in the `cf-release` working directory copy and rename the blob in the `.blobs/` directory as in:
```
cp .blobs/d77ee059e289ae52fb87812f1a2932268427dc26 ~/Downloads/openjdk-1.8.0_101-x86_64-trusty.tar.gz
```
5. Add the blob to the `java` directory as in:
```
bosh add blob ~/Downloads/openjdk-1.8.0_101-x86_64-trusty.tar.gz java
```
6. Upload the blobs as in:
```
bosh upload blobs
```

### Updating Elasticsearch

1. Remove the old elasticsearch section from `config/blobs.yml`
2. Download the latest release from https://www.elastic.co/downloads/elasticsearch
3. Add the blob to the `elasticsearch` directory as in:
```
bosh add blob ~/Downloads/elasticsearch-5.0.1.tar.gz elasticsearch
```
4. Upload the blobs as in:
```
bosh upload blobs
```

## Create a new release
1. `bosh create release --force --with-tarball`

## Create a final release
1. `bosh create release --final --with-tarball`
2. Commit the changes from the previous step
3. Create a release on github and upload the tarball to the release

## Deploying

1. `bosh upload release https://github.com/myshkin5/elasticsearch-release/releases/download/2/elasticsearch-2.tgz`
2. `curl --output elastic-search-bosh-lite.yml https://raw.githubusercontent.com/myshkin5/elasticsearch-release/master/templates/bosh-lite.yml`
3. Edit manifest and replace `PLACEHOLDER-DIRECTOR-UUID` with bosh-lite director UUID (from `bosh status --uuid`).
4. `bosh -d elastic-search-bosh-lite.yml deploy`
