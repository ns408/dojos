# brew - create your own tap

## Requirements

- macOS
- [brew installation]
- github.com account

## High-level

- create/find github repo where your application code is present. <br> Eg: https://github.com/ns408/example_c_language
- make your formula
- create the homebrew-tap repo.
  - two ways of doing that:
    - creating a single "homebrew-tap" repo. <br>(_easier to manage_)
    - creating per application "homebrew-$app" repos. <br>(_easier to manage history and reduces blast radius or unauthorised modifications to other than indended formulae_)
- tap the homebrew-tap repo
- install the program

## Runsheet

- make your formula
```shell
# setup editor
export EDITOR=$(which vi)
brew create https://github.com/ns408/example_c_language/archive/v1.0.tar.gz
```
- specify your formula 
```shell
cat > /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/example_c_language.rb <<EOF
class ExampleCLanguage < Formula
  desc "Example repository containing C language code"
  homepage "https://github.com/ns408/example_c_language"
  url "https://github.com/ns408/example_c_language/archive/v1.0.tar.gz"
  sha256 "1a94b0235514dbd709183aecfea75d4a0925e468b1eb9d51f9a388026d10c424"

  def install
    ENV.deparallelize
    system ENV.cc, "example.c", "-o", "example_c"
    bin.install "example_c"
  end

  test do
    system bin/"example_c"
  end
end
EOF
```
- or copy the formula from https://raw.githubusercontent.com/ns408/homebrew-examples/master/example_c_language.rb
- audit your formula
```shell
brew audit --new-formula example_c_language
```
- create a repository named *homebrew-tap* in your github.com account
- git clone the repo
- move the local formula file to git repo:
```shell
cd homebrew-tap
mv /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/example_c_language.rb .
```
- commit and push the change to the repo.
- enable the tap
```shell
brew tap $github_user/homebrew-tap
```
- brew install the app
```shell
brew install example_c_language
```
- run the application
```shell
example_c_language
```
- where this binary is placed
```shell
ls -la $(which example_c_language)
```

## Good to know

- debug installing a cask
```shell
brew intall example_c_language -d
```
- show all the taps
```shell
brew tap
```
- untap a tap. Eg:
```shell
brew untap ns408/examples
```

## Reference:

- [brew installation]
- [Formula-Cookbook]
- [make new formulae]

[brew installation]: https://docs.brew.sh/Installation
[Formula-Cookbook]: https://docs.brew.sh/Formula-Cookbook
[make new formulae]: https://docs.brew.sh/FAQ#can-i-make-new-formulae