# Testcontainers
C'est une librairie java supportant JUnit et une base de données dockerisable

# .main.yml
``` yml
name: CI devops 2022 CPE
on:
  # actions déclenchants le workflow
  push:
    branches: master
  pull_request:
jobs:
  # distrib linux
  test-backend:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2.3.3
      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'
          distribution: 'adopt'
      - name: Build and test with Maven
        # run maven
        run: mvn clean verify --file ./back/simple-api-main
```