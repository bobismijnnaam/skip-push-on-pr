Example workflow:

```yaml
name: "JS Actions"

on: [push, pull_request]

jobs:
  before-work:
    runs-on: "ubuntu-latest"
    outputs:
      must-skip: ${{ steps.skip-check.outputs.must-skip }}
    steps:
      - uses: actions/checkout@v2
        with: 
          repository: bobismijnnaam/skip-push-on-pr
          ref: main
          path: skip-push-on-pr
      - id: skip-check
        uses: ./skip-push-on-pr
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  work:
    needs: before-work
    if: ${{ needs.before-work.outputs.must-skip != 'true' }}

    runs-on: "ubuntu-latest"
    steps:
      - name: "important work"
        run: 'echo "veeeeery important"'
```
