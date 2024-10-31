# py_oathtool2

py_oathtool2 is a Python script for easy OTP code generation on the command line for use with scripting or simply as a convenience. It is a maintained fork of https://github.com/matalo33/py_oathtool.

## Installation

`pip install py_oathtool2`

## Dependencies

`py_oathtool2` automatically copies codes to the clipboard. This depends on the following programs being installed:

* Linux with X11: `xclip`
* Linux with Wayland: `wl-copy`
* MacOS: `pbcopy` (installed by default)

## Configuration

Two pieces of information are required for each account:

* An account name / label
* Your 64 character oath secret provided by the 3rd party. This is typically a QR code, but websites often also offer the string.

The script will read these values from a config file sourced from, by default, `~/.otp-secrets.yaml` in the following format:
```
otpsecrets:
  github: IOOVV6U5AUHUISZKJNVCCG4JWUR5XDFSI7ND62A7QT5ZOEVYVA7JEEDKTG3ZM57B
  aws-account-dev: XQYNZOIA4PWCTJCB9654EQP5LUIP23BOW6J5ZIRZZSDHK24AUEDUSCONP3KQQY4N
  aws-account-prod: 57QPXJFJ4D2ILQBRZGSHKAZCJ2Y46C52FGVSZRYMY7UMWTIQI6I3GOJQZ4VJN2R4
```

Additionally, the following configuration options are supported in the `~/.otp-secrets.yaml` file:

* `holdoff`: Specify a different holdoff value to wait for the next code
* `use_clipboard`: Disable putting the code on the clipboard

## Usage

List all configured accounts with `-l`:
```
$ otp -l
github
aws-account-dev
aws-account-prod
```

Generate an OTP by providing the account name. The script will provide the OTP code and also put it on the clipboard:
```
$ otp aws-account-dev
129987 (10sec)
```

See all arguments with:
```
$ otp --help
```

## Shell Autocompletion

Autocompletion support can be added through use of the `-t` or `-l` flags. The former space-delimits accounts and the latter by newlines.


### zsh

For zsh support, add the following to your `.zshrc` file:

```
compdef _otp otp
_otp() {
    compadd `otp -t`
}
```

### fish

For fish support, add the following to your `config.fish`:

```
complete --command otp --no-files --arguments "(otp -l)"
```

## Development

After cloning use `pipenv` to install dependencies and run the script:

```
pipenv install --dev
pipenv run python src/py_oathtool2/otp.py [args]
```

### Running Tests

```
pipenv run python -m unittest test/test_otp.py
```

### Building & Installing Locally

```
pipenv run python -m build
pip install --force-reinstall dist/py_oathtool2-*.whl
```

### Upload to PyPI

```
rm dist/*
pipenv run python -m build
pipenv run python -m twine upload --config-file ~/.pypirc dist/*
```

## Disclaimer

Two factor auth is meant to provide an extra layer of account security and this tool does not exactly promote that concept. You are responsible for taking reasonable steps to protect your secrets file to prevent loss of secrets, and perhaps this is not the ideal two factor solution for your most important accounts.

## License

GPLv2
