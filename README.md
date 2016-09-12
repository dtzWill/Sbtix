# Sbtix

## What?

Sbtix generates a Nix definition that represents your SBT project's dependencies. It then uses this to build a Maven repo containing the stuff your project needs, and feeds it back to your SBT build.

## Why?

Currently, this should mean that you won't have to redownload the world for each rebuild.

Additionally, this means that Nix can do a better job of enforcing purity where required. Ideally the build script itself should not communicate with the outer world at all, since otherwise Nix does not allow proxy settings to propagate.

## Why not? (caveats)

* Alpha quality, beware (and please report any issues!)
* Private (password-protected) Maven stores are currently unsupported (see issue #3)
* Plugins and SBT itself are not cached by Sbtix (see issue #2)
* POMs are redownloaded several times (see issue #1)
* You must use the Coursier dependency resolver instead of Ivy (because SBT's Ivy resolver does not report the original artifact URLs)

## How?

Add the SBT plugin to your project and run `$ sbt genNix`. Do check the generated `repo.nix` into your source control. Then copy `sbtix.nix` and `default.nix` from `src/sbt-test/sbtix/simple` and customize to your needs. Finally, run `nix-build` to build!

### Authentication

In order to use a private repository, add your credentials to `coursierCredentials`. Note that the key should be the name of the repository, see `src/sbt-test/sbtix/simple/build.sbt` for an example! Also, you must currently set the credentials for each project, `in ThisBuild` doesn't work currently. This is for consistency with Coursier-SBT.
