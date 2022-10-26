# Github Actions Demo 

## [Demo](cicdtest-f3fb7.web.app)

Implement CI/CD with Github Actions. 

## Testing using jest
```
npm test
```

## Add integrate action CI (Continuous integration)
``integrate.yml`` file in .github/workflows
```
name: Node Continuous Integration

on:
  pull_request:
    branches: [ main ]


jobs:
  test_pull_request:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: npm ci
      - run: npm test
      - run: npm run build
```

## Deploy with firebase hosting CD (Continuous delivery)

### Firebase init project
```
firebase init
```

### Deploy in firebase
```
firebase deploy --only hosting
```

### Create firebase token
```
firebase login:ci
```

Add token to secret action
add name to secret "FIREBASE_TOKEN"

### add deploy action
``deploy.yml`` file in .github/workflows
```
name: Firebase Continuous Deployment

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - uses: actions/setup-node@master
        with:
          node-version: 12
      - run: npm ci
      - run: npm run build
      - uses: w9jds/firebase-action@master
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```
