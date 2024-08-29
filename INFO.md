# What is Skaffold? 

Skaffold is a command line tool that facilitates continuous development for container based & Kubernetes applications. Skaffold handles the workflow for building, pushing, and deploying your application, and provides building blocks for creating CI/CD pipelines. This enables you to focus on iterating on your application locally while Skaffold continuously deploys to your local or remote Kubernetes cluster, local Docker environment or Cloud Run project.

# Skaffold Workflow and Architecture 
Skaffold simplifies your development workflow by organizing common development stages into one simple command. Every time you run skaffold dev, the system

- Collects and watches your source code for changes
- Syncs files directly to pods if user marks them as syncable
- Builds artifacts from the source code
- Tests the built artifacts using container-structure-tests or custom scripts
- Tags the artifacts
- Pushes the artifacts
- Deploys the artifacts
- Monitors the deployed artifacts
- Cleans up deployed artifacts on exit (Ctrl+C)

# Bootstrap Skaffold configuration
Open your new skaffold.yaml, generated at skaffold/examples/buildpacks-node-tutorial/skaffold.yaml. All of your Skaffold configuration lives in this file.

# Using Skaffold for continuous development
Skaffold speeds up your development loop by automatically building and deploying the application whenever your code changes.

Commands: 
    skaffold dev                    # Skaffold CD
    minikube tunnel -p custom       # open new terminal to browse to app
    app.listen(port, () => console.log(`Example app listening on port ${port}! This is version 2.`)) # mod skaffold/examples/buildpacks-node-tutorial/src/index.js ; line 10

# Use Skaffold for continuous improvement
Your CI pipelines can run skaffold build to build, tag, and push your container images to a registry.

Commands: 
    export STATE=$(git rev-list -1 HEAD --abbrev-commit)
skaffold build --file-output build-$STATE.json  # build, tag, push container images to a registry
    test:
- image: skaffold-buildpacks-node
  custom:
    - command: echo This is a custom test commmand! # add config to skaffold.yaml to run tests against images before deploying them
    skaffold test --build-artifacts build-$STATE.json # execute the test

# Use Skaffold for continuous delivery

Commands: 
    skaffold deploy -a build-$STATE.json  # simple deployment
    skaffold render -a build-$STATE.json --output render.yaml --digest-source local # render a hydrated manifest
    Open render.yaml # check out manifest > skaffold apply render.yaml # apply manifest

Need to know:
CI = Continuos Integration
    Foundation layer, automating the early stages of development. Devs commit code to repo, trigger automation, build and test processes. Helps to catch bugs and prevents integration issues between parts of code. 

CD = Continuous Delivery
    Automation portion that focuses on the entire software release processes, not just build and test phases. Every change that passes tests will essentially be ready for deployment to prod. 

CD = Continuous Deployment (confused)
    Any changes ready for prod; WITHOUT manual approval


