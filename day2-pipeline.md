# Day 2: Full CI Pipeline — Git → Maven → Docker

## What I built
- Jenkins pipeline that clones a GitHub repo, builds it with Maven, then builds a Docker image
- Container running and reachable on port 9090

## Steps
1. Cloned my Git repo as a pipeline stage
2. Installed Maven in Jenkins (Manage Jenkins → Tools), referenced it via `tools { maven 'Ps-maven' }` in the Jenkinsfile
3. Ran `mvn clean package` as a build stage — succeeded
4. Attempted to build a Docker image from the pipeline — hit an error
5. Root cause: Docker wasn't installed on the Jenkins host itself — installed Docker on the EC2 instance directly via SSH
6. Restarted Jenkins, reran the pipeline — Docker image build stage succeeded
7. Added a container-run stage mapping port 9090 (host) → 8080 (container), since 8080 was already taken by Jenkins itself
8. Opened port 9090 in the EC2 Security Group's inbound rules (same pattern as opening 8080 earlier)
9. Verified the running container by hitting it directly in the browser

## What I'd do differently
- [optional: e.g. "install Docker before starting the pipeline, not after hitting the error" — genuine reflection, if you have one]
