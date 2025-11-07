# Repo with modules for pact demonstration

I have presented and demonstated Pact

- on Saturday, November 8, 2025, at VOXXEDDAYS Thessaloniki 2025, https://m.devoxx.com/events/vdthess25/talks/1526/executable-agreements-code-collaborate-and-deploy-with-confidenc, Slides: https://docs.google.com/presentation/d/1mLhavgclnEea_wlcD2VWGBNcgJ_DBJB1aRWYgbST_jU/present

- on Tuesday, May 27, 2025 at Thessaloniki Software Testing & QA Meetup: https://www.linkedin.com/events/7326541118811082754 https://www.meetup.com/thessaloniki-software-testing-and-qa/events/307732820/?recId=[…]55de7-39c8-4fb3-b2d6-0d5039f987b1&eventOrigin=find_page%24all.
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

## Run can-i-deploy and record-deployment

Examples:

Consumer

```
pact-broker can-i-deploy -b "http://localhost:9292" --to-environment production --pacticipant "ProductsUI" --version 8f5ccfcb61da4cc4574a04d7ec69b7c2f90ac19f
```

```
pact-broker record-deployment --environment production -b http://localhost:9292 --pacticipant "ProductsUI" --version 8f5ccfcb61da4cc4574a04d7ec69b7c2f90ac19f
```

Provider

```
pact-broker can-i-deploy -b "http://localhost:9292" --to-environment production --pacticipant "ProductsAPI" --version 3ae3e99922d4bc611471365879e71af8b2872747
```

```
pact-broker record-deployment --environment production -b http://localhost:9292 --pacticipant "ProductsAPI" --version 3ae3e99922d4bc611471365879e71af8b2872747
```
