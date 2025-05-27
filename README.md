# Repo with modules for pact demonstration

I have presented and demonstated Pact on Tuesday, May 27, 2025 at Thessaloniki Software Testing & QA Meetup: https://www.linkedin.com/events/7326541118811082754 https://www.meetup.com/thessaloniki-software-testing-and-qa/events/307732820/?recId=[…]55de7-39c8-4fb3-b2d6-0d5039f987b1&eventOrigin=find_page%24all

Slides:
https://docs.google.com/presentation/d/15oVJ9X_VIkvsS8XWgyINT70bhExjGzn8mG3pR6j99Ps/present

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
