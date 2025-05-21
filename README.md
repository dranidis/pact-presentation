# Repo with modules for pact presentation

After cloning the repo, run:

```
git submodule init
git submodule update
```

## Start the pact broker
```
cd pact-broker-docker
docker compose up
```

## Run consumer/provider tests
Each project contains a README with instructions how to use.