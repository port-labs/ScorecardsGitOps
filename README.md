# example-scorecards-gitops
This repository serves as an example on how you can control your scorecards in Port with GitOps approach using a github workflow.

## Getting started
1. Add the following secrets to your repository:
* `PORT_CLIENT_ID`
* `PORT_CLIENT_SECRET`

2. Add the `sync-scorecards.yaml` workflow into your .github/workflows folder
3. Create a folder named `scorecards` in your root directory of the repository.
4. Create a `<blueprint identifier>.json` file with the desired scorecards in an array.

## How it works?
The workflow is listening for push events on files inside the scorecards folder, and will attempt to parse the files inside and send them to Port. The content of the file needs to be an array of scorecard objects (The file needs to be an array even if you manage only one scorecard).

For each blueprint you wish to manage the scorecards using this method, create a file named `<blueprint identifier>.json` file in that folder, for example `service.json` (where `blueprint identifier` needs to be equal to the blueprint identifier in Port).


## Notes
1. If you wish to change the location of the scorecards folder in your directory, you will need to update the workflow in the relevant locations ([here](https://github.com/port-labs/example-scorecards-gitops/blob/132aa54dbe9c9617575dd7f240fe42c00b70575e/.github/workflows/Sync-Scorecards.yaml#L6) and [here](https://github.com/port-labs/example-scorecards-gitops/blob/132aa54dbe9c9617575dd7f240fe42c00b70575e/.github/workflows/Sync-Scorecards.yaml#L32)).
2. Warning - This method is using a GitOps approach, which means that the files will act as the source of truth for your Scorecards configurations. Whenever you update the file, it will override the scorecards that already exist in Port, so make sure you create/update scorecards from the repository to not lose your progress.

## Example
Disclaimer: As mentioned in Note 2, this example overrides the current scorecards set on a blueprint. If you are using this method, add all of your existing scorecards to the file before commiting an update.

In this repository you can find a working example. In the scorecards folder, you can find a file named `service.json` which controls the scorecards of your `service` blueprint in Port. Scorecard example:

```json
service.json

[
    {
  "identifier": "Ownership",
  "title": "Ownership",
  "levels": [
    {
      "color": "paleBlue",
      "title": "Basic"
    },
    {
      "color": "bronze",
      "title": "Bronze"
    },
    {
      "color": "silver",
      "title": "Silver"
    },
    {
      "color": "gold",
      "title": "Gold"
    }
  ],
  "rules": [
    {
      "identifier": "hasTeam",
      "title": "Has Team",
      "level": "Bronze",
      "query": {
        "combinator": "and",
        "conditions": [
          {
            "operator": "isNotEmpty",
            "property": "$team"
          }
        ]
      }
    }
  ]
}
]

```

Copy the file into your scorecards folder, and change its name to the blueprint identifier in Port. After commiting the file to the main branch in your repository, the scorecard will be updated into Port.
