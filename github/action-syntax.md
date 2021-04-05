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

#


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
