# ScorecardsGitOps
This repository serves as an example on how you can control your scorecards in Port with GitOps approach using a github workflow.

## Getting started
1. Add the following secrets to your repository:
* PORT_CLIENT_ID
* PORT_CLIENT_SECRET

2. Add the `scorecards.yaml` workflow into your .github/workflows folder
3. Create a folder named `scorecards` in your root directory of the repository.

## Using the workflow
The workflow is listening for push events on files inside the scorecards folder, and will attempt to parse the files inside and send them to Port.
For each blueprint you wish to manage the scorecards using this method, create a file named `blueprintIdentifier.json` file in that folder (where `blueprintIdentifier` needs to be equal to the blueprint identifier in Port).

Each file needs to be an array with all of the scorecards you wish to have on the blueprint.

## Notes
1. If you wish to change the location of the scorecards folder in your directory, you will need to update the workflow in the relevant locations ( [here](https://github.com/port-labs/ScorecardsGitOps/blob/132aa54dbe9c9617575dd7f240fe42c00b70575e/.github/workflows/Sync-Scorecards.yaml#L6) and [here](https://github.com/port-labs/ScorecardsGitOps/blob/132aa54dbe9c9617575dd7f240fe42c00b70575e/.github/workflows/Sync-Scorecards.yaml#L32)).
2. This method is using a GitOps approach, which means that the files will act as the source of truth for your Scorecards configurations. Whenever you update the file, it will override the scorecards that already exist in Port, so make sure you create/update scorecards from the repository to not lose your progress.

## Example
In this repository you can find a working example. In the scorecards folder, you can find a file named `repository.json` which controls the scorecards of your `repository` blueprint in Port.

Copy the file into your scorecards folder, and change its name to the blueprint identifier in Port. After commiting the file to the main branch in your repository, the scorecard will be updated into Port.
