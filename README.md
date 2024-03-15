# CocoaPods Azure Universal Packages plugin

Fork of the original gem to solve dependency issues and keep this tool alive. Maintainers at Microsoft seem to have completely abandoned it.

This project is a [CocoaPods](https://github.com/CocoaPods/CocoaPods) plugin that allows to dowload pods published as [Universal Packages](https://docs.microsoft.com/en-us/azure/devops/artifacts/quickstarts/universal-packages) in [Azure Artifacts](https://azure.microsoft.com/en-gb/services/devops/artifacts/) feeds.

## Getting started

Install the plugin by adding to your `Gemfile`
```Ruby
gem "cocoapods-azure-universal-packages2"
```

Under the hood the plugin uses the [Azure CLI](https://aka.ms/azcli) to download the Universal Packages, you can install it running
```shell
brew update && brew install azure-cli
```

Finally, ensure that you are logged in using
```shell
az login
```

## How it works

The plugin replaces the default CocoaPods HTTP downloader when a pod source URL points to a universal package and it uses the Azure CLI to perform the download.

_Note:_ The plugin will install the Azure CLI [DevOps extension](https://github.com/Azure/azure-devops-cli-extension) automatically when running `pod install` or `pod update` for the first time.

## Usage

Add to your Podfile
```Ruby
plugin 'cocoapods-azure-universal-packages2', {
    organization: '{{ORGANIZATION_URL}}'
}
```
replacing `{{ORGANIZATION_URL}}` with the base URL of your Azure Artifacts feed (for example: `https://pkgs.dev.azure.com/myorg`).

Then, in your podspec set the pod's source to
```Ruby
# For project scoped feeds:
spec.source = { http: '{{ORGANIZATION_URL}}/{{PROJECT}}/_apis/packaging/feeds/{{FEED}}/upack/packages/{{PACKAGE}}/versions/{{VERSION}}' }

# For organization scoped feeds:
spec.source = { http: '{{ORGANIZATION_URL}}/_apis/packaging/feeds/{{FEED}}/upack/packages/{{PACKAGE}}/versions/{{VERSION}}' }
```
where:
- `{{ORGANIZATION_URL}}` is the URL of your feed's organization
- `{{PROJECT}}` is the name of your feed's project (you must specify this only if your feed is a project scoped feed)
- `{{PACKAGE}}` is the name of your universal package
- `{{VERSION}}` is the version of your universal package

### Plugin parameters

| Parameter | Description |
| --------- | ----------- |
| `organization` | The URL of the Azure Artifacts feed's orgnization. Required unless `organizations` is specified. |
| `organizations` | An array of URLs of possible Azure Artifacts feeds' organizations. Required unless `organization` is specified. |
| `update_cli_extension` | Whether to update the Azure CLI DevOps extensions automatically. Default to `false`. |

## Run tests for this plugin

To run the tests, use
```shell
rake tests
```

## Contributing

Feel free to contribute by reporting issues or creating pull requests

## Licence

MIT
