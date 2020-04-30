[![GitHub release](https://img.shields.io/github/release/crazy-max/ghaction-virustotal.svg?style=flat-square)](https://github.com/crazy-max/ghaction-virustotal/releases/latest)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-virustotal--github--action-blue?logo=github&style=flat-square)](https://github.com/marketplace/actions/virustotal-github-action)
[![Test workflow](https://github.com/crazy-max/ghaction-virustotal/workflows/test/badge.svg)](https://github.com/crazy-max/ghaction-virustotal/actions?workflow=test)
[![Become a sponsor](https://img.shields.io/badge/sponsor-crazy--max-181717.svg?logo=github&style=flat-square)](https://github.com/sponsors/crazy-max)
[![Paypal Donate](https://img.shields.io/badge/donate-paypal-00457c.svg?logo=paypal&style=flat-square)](https://www.paypal.me/crazyws)

## About

GitHub Action to upload and scan files with [VirusTotal](https://www.virustotal.com).

If you are interested, [check out](https://git.io/Je09Y) my other :octocat: GitHub Actions!

![VirusTotal GitHub Action](.res/ghaction-virustotal.png)

## Usage

### Scan local files

This action can be used to scan local files with VirusTotal:

```yaml
name: build

on:
  pull_request:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      -
        # https://github.com/actions/checkout
        name: Checkout
        uses: actions/checkout@v2
      -
        # https://github.com/actions/setup-go
        name: Set up Go
        uses: actions/setup-go@v2
      -
        name: Build
        run: |
          GOOS=windows GOARCH=386 go build -o ./ghaction-virustotal-win32.exe -v -ldflags "-s -w"
          GOOS=windows GOARCH=amd64 go build -o ./ghaction-virustotal-win64.exe -v -ldflags "-s -w"
      -
        # https://github.com/crazy-max/ghaction-virustotal
        name: VirusTotal Scan
        uses: crazy-max/ghaction-virustotal@v1
        with:
          files: |
            ./ghaction-virustotal-win32.exe
            ./ghaction-virustotal-win64.exe
        env:
          VT_API_KEY: ${{ secrets.VT_API_KEY }}
```

### Scan assets of a published release

You can also use this action to scan assets of a published release on GitHub when a [release event](https://help.github.com/en/actions/reference/events-that-trigger-workflows#release-event-release) is triggered:

```yaml
name: released

on:
  release:
    types: [published]

jobs:
  virustotal:
    runs-on: ubuntu-latest
    steps:
      -
        # https://github.com/crazy-max/ghaction-virustotal
        name: VirusTotal Scan
        uses: crazy-max/ghaction-virustotal@v1
        env:
          VT_API_KEY: ${{ secrets.VT_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Customizing

### inputs

Following inputs can be used as `step.with` keys

| Name          | Type    | Default   | Description                      |
|---------------|---------|-----------|----------------------------------|
| `files`       | String  |           | Newline-delimited list of path globs for asset files to upload for analysis |

### environment variables

Following environment variables can be used as `step.env` keys

| Name           | Description                           |
|----------------|---------------------------------------|
| `VT_API_KEY  ` | [VirusTotal API key](https://developers.virustotal.com/v3.0/reference#authentication) required to upload assets |
| `GITHUB_TOKEN` | [GITHUB_TOKEN](https://help.github.com/en/actions/configuring-and-managing-workflows/authenticating-with-the-github_token) as provided by `secrets` |

## How can I help?

All kinds of contributions are welcome :raised_hands:! The most basic way to show your support is to star :star2: the project, or to raise issues :speech_balloon: You can also support this project by [**becoming a sponsor on GitHub**](https://github.com/sponsors/crazy-max) :clap: or by making a [Paypal donation](https://www.paypal.me/crazyws) to ensure this journey continues indefinitely! :rocket:

Thanks again for your support, it is much appreciated! :pray:

## License

MIT. See `LICENSE` for more details.
