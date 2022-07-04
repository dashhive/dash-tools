# dash-utils

Meta repo for various Dash tools

# Developing

Clone all the code in at once:

```bash
git clone --recursive https://github.com/dashhive/dash-utils
```

For the convenience of the casual git user, these submodules use (unauthenticated) https.

If you'd like to develop and contribute, update your git config to automatically substitute the proper ssh url automatically:

```bash
git config --global \
  url."ssh://git@github.com/dashhive/".insteadOf \
  "https://github.com/dashhive/"
```
