# Repo with modules for pact presentation

Presentation:
https://docs.google.com/presentation/d/15oVJ9X_VIkvsS8XWgyINT70bhExjGzn8mG3pR6j99Ps/edit?usp=sharing

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

Visit `http://localhost:9292/`.

## Run consumer/provider tests

Each project contains a README with instructions how to use.
