---
{"dg-publish":true,"permalink":"/garden-notes/github-actions-diff-directory/","tags":["note","seedling"],"created":"2023-02-20T09:50","updated":"2024-11-29T18:10"}
---

# Github Actions Diff Directory

Example github action that triggers a job when change in a folder is detected:

```yaml
name: Validate terraform and check terraform file formatting

on: [pull_request]

jobs:
  generate_matrix:
    name: Generate matrix with modified dirs
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 2

      - name: Diff dirs
        id: diff-dirs
        run: |
          echo "matrix=$(git --no-pager diff --name-only HEAD^ HEAD | awk -F '/' '{print $1}' | uniq | grep -E '.*-preprod-.*|.*-prod-.*' | jq -R -s -c 'split("\n") | map(select(length > 0))')" >> $GITHUB_OUTPUT
    outputs:
      matrix: ${{ steps.diff-dirs.outputs.matrix }}

  validate:
    name: Validate terraform
    runs-on: ubuntu-latest
    needs: generate_matrix
    if: ${{ needs.generate_matrix.outputs.matrix != '[]' }}
    strategy:
      matrix:
        path: ${{ fromJson(needs.generate_matrix.outputs.matrix) }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: terraform validate
        uses: dflook/terraform-validate@v1
        with:
          path: ${{ matrix.path }}

  check_format:
    name: Check terraform formatting
    runs-on: ubuntu-latest
    needs: generate_matrix
    if: ${{ needs.generate_matrix.outputs.matrix != '[]' }}
    strategy:
      matrix:
        path: ${{ fromJson(needs.generate_matrix.outputs.matrix) }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: terraform fmt
        uses: dflook/terraform-fmt-check@v1
        with:
          path: ${{ matrix.path }}

  validate_result:
      name: Validate terraform - result
      if: ${{ always() }}
      runs-on: ubuntu-latest
      needs: validate
      steps:
        - run: |
            validate="${{ needs.validate.result }}"
            if [[ $validate == "success" ]] || [[ $validate == "skipped" ]]; then
              exit 0
            else
              exit 1
            fi

  check_format_result:
      name: Check terraform formatting - result
      if: ${{ always() }}
      runs-on: ubuntu-latest
      needs: check_format
      steps:
        - run: |
            check_format="${{ needs.check_format.result }}"
            if [[ $check_format == "success" ]] || [[ $check_format == "skipped" ]]; then
              exit 0
            else
              exit 1
            fi
```


---
- https://github.com/orgs/community/discussions/25669
- https://github.com/dorny/paths-filter/issues/66
