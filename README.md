# pullrequest-events-resource

# Source Configuration

```yaml
resource_types:
- name: pull-request-events
  type: docker-image
  source:
    repository: shinmyung0/pullrequest-events-resource
    tag: latest


resources:
- name: github-pr-events
  type: pull-request-events
  source:
    access_token: ((github-access-token))
    owner: ((repo-owner))
    repo: ((app-name))
    graphql_api: ((github-v4-api))
    base_branch: master
    first: 5
    states:
      - MERGED
      - CLOSED
```

* `graphql_api` : Github GraphQL API endpoint. By default it is `https://api.github.com/graphql`
* `owner` : Repo owner. **required**
* `repo` : Repository. **required**
* `access_token`: Github API access token, should have read-permissions on repo. **required**
* `first` : The number of pull request events to fetch from latest. default is `5`.
* `states` : List of pull request states to listen for. Only supports `MERGED` and `CLOSED`. Default is both.




# Behavior

- `check`: check for recently merged or closed pull requests

This will output as versions
```json
[
  {
    "id": "MDExOlB1bGxSZXF1ZXN0MTcxMjQ5NjI1",
    "cursor": "Y3Vyc29yOnYyOpK5MjAxOC0wMi0yNVQxMjozNDo0NC0wODowMM4KNQ/Z",
    "number": "1",
    "url": "https://github.com/shinmyung0/fixture-repo/pull/1",
    "baseBranch": "master",
    "headBranch": "test-merged-branch",
    "state": "merged",
    "timestamp": "2018-02-25T20:34:44Z"
  },
  {
    "id": "MDExOlB1bGxSZXF1ZXN0MTcxMjQ5NzQw",
    "cursor": "Y3Vyc29yOnYyOpK5MjAxOC0wMi0yNVQxMjozNjoxNi0wODowMM4KNRBM",
    "number": "2",
    "url": "https://github.com/shinmyung0/fixture-repo/pull/2",
    "baseBranch": "master",
    "headBranch": "test-closed-branch",
    "state": "closed",
    "timestamp": "2018-02-25T20:36:16Z"
  }
]

```


- `in`: get information about the recently closed pull requests which will be output as `$output_dir/pull_request` JSON file.

- `out`: noop