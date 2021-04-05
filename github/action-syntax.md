# "If" in steps
steps:
 - name: My first step
   if: ${{ github.event_name == 'pull_request' && github.event.action == 'unassigned' }}
   run: echo This event is a pull request that had an assignee removed.
   
# "If" in jobs
jobs:
  PR-Comment:
    if: github.actor == 'user1' || github.actor == 'user2'
    runs-on: ubuntu-latest
    steps:
    - name: PR Comment
    run: echo This event will be triggered for user1 or user2


# if with contains-method
if: contains('["user1","user2"]', github.actor)

# Github "context" ([sourse:](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions#github-context) )
- github.action
- github.action_path
- github.actor
- github.base_ref
- github.event (...)
- github.event_name
- github.event_path
- github.head_ref
- github.job
- github.ref
- github.repository (owner/repositoryName)
- github.repository_owner
- github.run_id
- github.run_number
- github.sha (commit sha)
- github.token
- github.workflow
- github.workspace


# Github event triggers "on" ([sourse:](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)
- check_run
- check_suite
- create (someone creates a branch or tag)
- delete (someone deletes a branch or tag)
- deployment
- deployment_status
- fork
- gollum (someone creates or updates a Wiki page)
- issue_comment (types: -> created -> edited -> deleted)
- issues [event type](https://docs.github.com/en/developers/webhooks-and-events/issue-event-types)
- label (types - >created -> edited -> deleted)
- milestone
- page_build
- project (created/ updated/ closed/ reopened/ edited/ deleted)
- project_card
- project_column
- public (someone makes a private repository public)
- pull_request (types: assigned/ opened/ synchronize/ reopened/ assigned/ unassigned/ labeled/ unlabeled/ edited/ closed/ reopened)
- pull_request_review (submitted, edited, dismissed)
- pull_request_review_comment (created/ edited/ deleted)
- pull_request_target (* specially used -> look up)
- push
- registry_package
- release
- status (anytime the status of a Git commit changes)
- watch (type:  started)
- workflow_run (types: completed/ requested [source](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_run))



#review on pull request


# all issue event types (go to link to see which "on:" to use)([sorce](https://docs.github.com/en/developers/webhooks-and-events/issue-event-types))
- added_to_project
- assigned
- automatic_base_change_failed
- automatic_base_change_succeeded
- base_ref_changed
- closed
- commented
- committed
- connected
- convert_to_draft
- converted_note_to_issue
- cross-referenced
- demilestoned
- deployed
- deployment_environment_changed
- disconnected
- head_ref_deleted
- head_ref_restored
- labeled
- locked
- mentioned
- marked_as_duplicate
- merged
- milestoned
- moved_columns_in_project
- pinned
- ready_for_review
- referenced
- removed_from_project
- renamed
- reopened
- review_dismissed
- review_requested
- review_request_removed
- reviewed
- subscribed
- transferred
- unassigned
- unlabeled
- unlocked
- unmarked_as_duplicate
- unpinned
- unsubscribed
- user_blocked


# issues ex:
- list repository issues: 
  `await octokit.request('GET /repos/{owner}/{repo}/issues', {
  owner: 'octocat',
  repo: 'hello-world'
})`

- create issues:  
 `await octokit.request('POST /repos/{owner}/{repo}/issues', {
  owner: 'octocat',
  repo: 'hello-world',
  title: 'title'
})`

- update an issue
 `await octokit.request('PATCH /repos/{owner}/{repo}/issues/{issue_number}', {
  owner: 'octocat',
  repo: 'hello-world',
  issue_number: 42,
  title: 'title'
})`

- Add assignees to an issue
`await octokit.request('POST /repos/{owner}/{repo}/issues/{issue_number}/assignees', {
  owner: 'octocat',
  repo: 'hello-world',
  issue_number: 42,
  assignees: [
    'assignees'
  ]
})`

-


# Comments for pull_request vs on issieus (!pull_requests)
on: issue_comment

jobs:
  pr_commented:
    # This job only runs for pull request comments
    name: PR comment
    if: ${{ github.event.issue.pull_request }}
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "Comment on PR #${{ github.event.issue.number }}"

  issue_commented:
    # This job only runs for issue comments
    name: Issue comment
    if: ${{ !github.event.issue.pull_request }}
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "Comment on issue #${{ github.event.issue.number }}"
# script directly in jaml (with "actions/github-script@v2" )
    steps:
    - name: PR Comment
      uses: actions/github-script@v2
      with:
        github-token: ${{secrets.GITHUB_TOKEN}}
        script: |
          github.issues.createComment({
            issue_number: ${{ github.event.number }},
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: 'No need for JS files now'
          })

# use inputs
name: Using input
on:
  workflow_dispatch:
    inputs:
      name:
        description: 'Person to greet'
        required: true
        default: 'User'

jobs:
  say_hello:
    runs-on: ubuntu-latest
    steps:
    - run: |
        echo "Hello ${{ github.event.inputs.name }}!"
