# test-github-collector

# Create a Apiconnect Discovery Action

The Apic Discovery Test repo allows you to test the discovery action from [here](https://github.com/ibm-apiconnect/apic-discovery-action). This is an example repo from where the API documents can be sent to the API connect manager using the discovery action and where they can be promoted as required to be managed by Apiconnect through their entire lifecycle.

See [discover-api.yml](.github/workflows/discover-api.yml)

The file has an example of sending the API documents in different ways to the host `d-j02.apiconnect.dev.automation.ibm.com` to the provider organisation name `niraimathi` when the [workflow file](.github/workflows/discover-api.yml) or any one of the mentioned API file content changes.
Different ways of sending the API documents are mentioned in the commented section 
```
API_FILES: gmail-api.json
API_FILES: gmail-api.json,mit-api.json,gmail-api.yaml,APIfolder/uber-api.json
API_FOLDERS: APIfolder
API_FOLDERS: APIfolder,APIfolderTwo
```
The job `check_changes_job` checks for the changes in the workflow file or mentioned API document and the job `run-discovery` sends the documents mentioned.
The `run-discovery` job uses the apic-discovery-action repo main branch in the ibm-apiconnect organization
 - uses: ibm-apiconnect/apic-discovery-action@main 
with the parameters supplied
```
      with:
        api_host: ${{ env.API_HOST }}
        provider_org: ${{ env.PROVIDER_ORG }}
        api_key: ${{ secrets.apicApikey }}
        if: env.API_FILES
        api_files: ${{ env.API_FILES }}
        else if: env.API_FOLDERS
        api_folders: ${{ env.API_FOLDERS }}
        resync_check: ${{ needs.check_changes_job.outputs.action_changed && true || false }}
```
The content can be modified according to the test requirement for sending the APIs

## More details on setting up a sample GitHub Action
For more details on how to set up a GitHub Action workflows in your Github repo in general see [the quickstart guide](https://docs.github.com/en/actions/quickstart).  
