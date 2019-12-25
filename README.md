# GitHub Action to Sync DigitalOcean Space 🔄 

This simple action uses the [vanilla AWS CLI](https://docs.aws.amazon.com/cli/index.html) to sync a directory (either from your repository or generated during your workflow) with a remote DigitalOcean space.

**Performing this action deletes any files in the bucket that are *not* present in the source directory.**

## Usage

### `workflow.yml` Example

Place in a `.yml` file such as this one in your `.github/workflows` folder. [Refer to the documentation on workflow YAML syntax here.](https://help.github.com/en/articles/workflow-syntax-for-github-actions)

```
name: Sync to DigitalOcean Spaces
on: push

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - uses: LibreTexts/do-space-sync-action@master
      with:
        args: --acl public-read
      env:
        SOURCE_DIR: './public'
        DEST_DIR: 'production'
        SPACE_NAME: ${{ secrets.Spaces_Name }}
        SPACE_REGION: ${{ secrets.Spaces_Region}}
        SPACE_ACCESS_KEY_ID: ${{ secrets.Spaces_Key }}
        SPACE_SECRET_ACCESS_KEY: ${{ secrets.Spaces_Secret }}
```


### Required Environment Variables

| Key | Value | Type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `SOURCE_DIR` | The local directory you wish to sync/upload. For example, `./public`. | `env` | **Yes** |
| `DEST_DIR` | The remote directory you wish to sync/upload to. For example, `production`. | `env` | **Yes** |
| `SPACE_REGION` | The region where you created your space in. For example, `sfo1`. [Full list of regions here.](https://www.digitalocean.com/docs/platform/availability-matrix/) | `env` | **Yes** |


### Required Secret Variables

The following variables should be added as "secrets" in the action's configuration.

| Key | Value | Type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `SPACE_NAME` | The name of the space you're syncing to. For example, `development`. | `secret` | **Yes** |
| `SPACE_ACCESS_KEY_ID` | Your Spaces Access Key. [More info here.](https://www.digitalocean.com/community/tutorials/how-to-create-a-digitalocean-space-and-api-key) | `secret` | **Yes** |
| `SPACE_SECRET_ACCESS_KEY` | Your Spaces Secret Access Key. [More info here.](https://www.digitalocean.com/community/tutorials/how-to-create-a-digitalocean-space-and-api-key) | `secret` | **Yes** |


## License

This project is distributed under the [MIT license](LICENSE.md).

## Credits

This project is forked from https://github.com/jakejarvis/s3-sync-action with small changes to make the s3 commands work with DigitalOcean.
