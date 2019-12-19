# CocoaPodsBitriseIssue

See: https://github.com/bitrise-steplib/steps-cocoapods-install/issues/43

This repo demonstrates an issue with [bitrise-steplib/steps-cocoapods-install](https://github.com/bitrise-steplib/steps-cocoapods-install) where it doesn't use bundler to call `pod install`. This is normally not too much of a problem, but if the `Podfile` is using plugins, then without bundler, `pod install` might not find those plugins (which are themselves gems).

Note: You likely need a clean ruby environment for this to manifest. Also, if you use something like `rbenv-bundler`, then you probably won't see this issue since `rbenv-bundler` uses `bundle exec` implicitly.

## Set up environment

```sh
$ brew bundle

$ gem install bundler -v 1.17.3
$ bundle install
```

## Run bitrise workflow (error shows up here)

```sh
$ bitrise run primary
```

Output:

<details>

```sh
  ██████╗ ██╗████████╗██████╗ ██╗███████╗███████╗
  ██╔══██╗██║╚══██╔══╝██╔══██╗██║██╔════╝██╔════╝
  ██████╔╝██║   ██║   ██████╔╝██║███████╗█████╗
  ██╔══██╗██║   ██║   ██╔══██╗██║╚════██║██╔══╝
  ██████╔╝██║   ██║   ██║  ██║██║███████║███████╗
  ╚═════╝ ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝╚══════╝╚══════╝

  version: 1.32.0

INFO[16:41:55] bitrise runs in Secret Filtering mode
INFO[16:41:55] Running workflow: primary

Switching to workflow: primary

+------------------------------------------------------------------------------+
| (0) cocoapods-install@1.7.2                                                  |
+------------------------------------------------------------------------------+
| id: cocoapods-install                                                        |
| version: 1.7.2                                                               |
| collection: https://github.com/bitrise-io/bitrise-steplib.git                |
| toolkit: go                                                                  |
| time: 2019-07-25T16:41:55-07:00                                              |
+------------------------------------------------------------------------------+
|                                                                              |
INFO[16:41:55]  * [OK] Step dependency (go) installed, available.

Configs:
- SourceRootPath: /Users/kyle/code/bugreports/CocoaPodsBitriseIssue
- PodfilePath:
- IsUpdateCocoapods: false
- InstallCocoapodsVersion:
- Verbose: true
- IsCacheDisabled: false

System installed cocoapods version:
$ pod "--version"
1.7.5

Searching for Podfile
Found Podfile: /Users/kyle/code/bugreports/CocoaPodsBitriseIssue/Podfile

Determining required cocoapods version
Searching for Podfile.lock
Found Podfile.lock: /Users/kyle/code/bugreports/CocoaPodsBitriseIssue/Podfile.lock
Required CocoaPods version (from Podfile.lock): 1.7.5

Install cocoapods version
Checking cocoapods 1.7.5 gem
Installed

Installing Pods
$ pod "_1.7.5_" "install" "--no-repo-update" "--verbose"

[!] Invalid `Podfile` file: undefined method `keep_source_code_for_prebuilt_frameworks!' for #<Pod::Podfile:0x00007ff3f94a9670>.

 #  from /Users/kyle/code/bugreports/CocoaPodsBitriseIssue/Podfile:6
 #  -------------------------------------------
 #  # Defined in cocoapods-binary plugin
 >  keep_source_code_for_prebuilt_frameworks!
 #
 #  -------------------------------------------

/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:301:in `rescue in block in from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:295:in `block in from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:50:in `instance_eval'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:50:in `initialize'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:293:in `new'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:293:in `from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:259:in `from_file'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/config.rb:199:in `podfile'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command.rb:150:in `verify_podfile_exists!'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command/install.rb:45:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/claide-1.0.2/lib/claide/command.rb:334:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command.rb:52:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/bin/pod:55:in `<top (required)>'
/Users/kyle/.rbenv/versions/2.5.3/bin/pod:23:in `load'
/Users/kyle/.rbenv/versions/2.5.3/bin/pod:23:in `<main>'
Command failed, error: exit status 1, retrying without --no-repo-update ...
$ pod "_1.7.5_" "repo" "update"
Updating spec repo `master`
  $ /usr/bin/git -C /Users/kyle/.cocoapods/repos/master fetch origin --progress
  remote: Enumerating objects: 28, done.
  remote: Counting objects: 100% (28/28), done.
  remote: Compressing objects: 100% (18/18), done.
  remote: Total 18 (delta 12), reused 0 (delta 0), pack-reused 0
  From https://github.com/CocoaPods/Specs
     e9fee01c69a..56970d7a91b  master     -> origin/master
  $ /usr/bin/git -C /Users/kyle/.cocoapods/repos/master rev-parse --abbrev-ref HEAD
  master
  $ /usr/bin/git -C /Users/kyle/.cocoapods/repos/master reset --hard origin/master
  HEAD is now at 56970d7a91b [Add] BoundlessKitEffect 0.0.1-beta
$ pod "_1.7.5_" "install" "--verbose"

[!] Invalid `Podfile` file: undefined method `keep_source_code_for_prebuilt_frameworks!' for #<Pod::Podfile:0x00007fd603955728>.

 #  from /Users/kyle/code/bugreports/CocoaPodsBitriseIssue/Podfile:6
 #  -------------------------------------------
 #  # Defined in cocoapods-binary plugin
 >  keep_source_code_for_prebuilt_frameworks!
 #
 #  -------------------------------------------

/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:301:in `rescue in block in from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:295:in `block in from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:50:in `instance_eval'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:50:in `initialize'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:293:in `new'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:293:in `from_ruby'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-core-1.7.5/lib/cocoapods-core/podfile.rb:259:in `from_file'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/config.rb:199:in `podfile'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command.rb:150:in `verify_podfile_exists!'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command/install.rb:45:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/claide-1.0.2/lib/claide/command.rb:334:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/lib/cocoapods/command.rb:52:in `run'
/Users/kyle/.rbenv/versions/2.5.3/lib/ruby/gems/2.5.0/gems/cocoapods-1.7.5/bin/pod:55:in `<top (required)>'
/Users/kyle/.rbenv/versions/2.5.3/bin/pod:23:in `load'
/Users/kyle/.rbenv/versions/2.5.3/bin/pod:23:in `<main>'
Command failed, error: exit status 1
|                                                                              |
+---+---------------------------------------------------------------+----------+
| x | cocoapods-install@1.7.2 (exit code: 1)                        | 27 sec   |
+---+---------------------------------------------------------------+----------+
| Update available: 1.7.2 -> 1.8.0                                             |
| Issue tracker: https://github.com/bitrise-io/steps-cocoapods-install/issues  |
| Source: https://github.com/bitrise-io/steps-cocoapods-install                |
+---+---------------------------------------------------------------+----------+


+------------------------------------------------------------------------------+
|                               bitrise summary                                |
+---+---------------------------------------------------------------+----------+
|   | title                                                         | time (s) |
+---+---------------------------------------------------------------+----------+
| x | cocoapods-install@1.7.2 (exit code: 1)                        | 27 sec   |
+---+---------------------------------------------------------------+----------+
| Update available: 1.7.2 -> 1.8.0                                             |
| Issue tracker: https://github.com/bitrise-io/steps-cocoapods-install/issues  |
| Source: https://github.com/bitrise-io/steps-cocoapods-install                |
+---+---------------------------------------------------------------+----------+
| Total runtime: 27 sec                                                        |
+------------------------------------------------------------------------------+


Submitting anonymized usage informations...
For more information visit:
https://github.com/bitrise-io/bitrise-plugins-analytics/blob/master/README.md
```

</details>

## See error manually

```sh
$ pod install
```

Output:

<details>

```sh
[!] Invalid `Podfile` file: undefined method `keep_source_code_for_prebuilt_frameworks!' for #<Pod::Podfile:0x00007fc10bbcd698>.

 #  from /Users/kyle/code/bugreports/CocoaPodsBitriseIssue/Podfile:6
 #  -------------------------------------------
 #  # Defined in cocoapods-binary plugin
 >  keep_source_code_for_prebuilt_frameworks!
 #
 #  -------------------------------------------
```

</details>

## Call `pod install` using `bundle exec` for it to correctly see cocoapods-binary plugin

```sh
$ bundle exec pod install
```

Output:

<details>

```sh
🚀  Prebuild frameworks

🤖  Pod Install
Analyzing dependencies
Downloading dependencies
Using PromiseKit (6.10.0)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There is 1 dependency from the Podfile and 1 total pod installed.
```

</details>


## (Optional) If you want to regenerate project

<details>

```sh
$ mint run xcodegen xcodegen generate
```

</details>
