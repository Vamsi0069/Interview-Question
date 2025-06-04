####  Integrate SonarQube in CI/CD?

* Add SonarQube scanner to Jenkins pipeline:

```groovy
withSonarQubeEnv('SonarQube') {
  sh 'sonar-scanner'
}
```
