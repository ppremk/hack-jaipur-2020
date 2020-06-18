# Requriements

The team would like: 
 - [ ] branch protections
 - [ ] required review approvals
 - [ ] easy way to see when enough approvals has been achieved
 - [ ] matrix build
 - [ ] save build artifacts
 - [ ] dedicated test job
 
 # CI Workflow (node.js.yml)
 
```
name: Node CI

on: [push]

jobs:
      - uses: actions/upload-artifact@master
    needs: build
          name: webpack artifacts
          path: public/

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: npm install and build webpack
        run: |
          npm install
          npm run build

  test:
    - uses: actions/download-artifact@master
      with:
        name: webpack artifacts
        path: public
    - name: npm install, and test
    runs-on: ubuntu-latest

    strategy:
      matrix:
        os: [ubuntu-latest, windows-2016]
        node-version: [8.x, 10.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - name: npm install, and test
      run: |
        npm install
        npm test
      env:
        CI: true
```

# Approval Workflow (approval-workflow.yml)
```
name: Team awesome's approval workflow
on: pull_request_review
jobs:
  labelWhenApproved:
    name: Label when approved
    runs-on: ubuntu-latest
    steps:
    - name: Label when approved
      uses: pullreminders/label-when-approved-action@master
      env:
        APPROVALS: "2"
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        ADD_LABEL: "approved"
        REMOVE_LABEL: "awaiting%20review"
```
