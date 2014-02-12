---
layout: post
title: "Legacy Release Candidates"
---

Release candidates 1 & 2 were discussed, indirectly, in the [pre-release announcement][1]. I'm about to push release candidate 3, which should be the final release candidate before the real and true legacy release! It's just that we broke an awful lot of the task APIs in the process. And I want to alert you to the changes before the push.

We didn't have to make breaking changes. We could have spent a few versions introducing aliases, parallel features, and the like. Then, obsolete and remove them over time. But, we're _way_ behind and, given the state of things, it was a reasonable option. There were a lot of inconsistent styles, naming, and patterns that were causing trouble.

I hope, now, you won't have to visit [the docs][2] every time you use a task, but, rather, once! You'll see the same "kinds" of breaking changes throughout, here is a summary

 * Changed boolean properties to a, usually, positively-named method (depends on defaults/language)
 
   ```ruby
   foo = true
   ```
   
   ```ruby
   do_foo
   ```
   
 * Changed all property/method names, where appropriate, to use underscores (`_`) where there would normally be a "space"
 
   ```ruby
   delaysign
   ```
   
   ```ruby
   delay_sign
   ```

 * Removed old, custom `options` parameters when the task supports the built-in, general `parameters` property
 
   ```ruby
   options = ["/foo"]
   ```
   
   ```ruby
   parameters = ["/foo"]
   ```

 * Quoted all paths and values that might have spaces (this is internal and will only be visible when `verbose` logging)
 
   ```
   $ rake foo 
   running command "foo.exe" -param1 path/to/foo with spaces
   ```
   
   ```
   $ rake foo
   running command "foo.exe" -param1 "path/to/foo with spaces"
   ```

## Breaking Changes

### aspnetcompiler

 * replaced the `clean`, `debug`, `delay_sign`, `fixed_names`, `force`, & `updateable`  properties with methods of the same name

### assemblyinfo

 * replaced the `com_visible` property with a method of the same name
 * removed the `use` method that allowed setting both `input_file` & `output_file` at the same time, use an assignment like `input_file = output_file = foo`

### csc

 * replaced the `debug` & `optimize` properties with methods of the same name
 * replaced the `delaysign` property with the method `delay_sign`
 * renamed the `keycontainer` and `keyfile` properties to use underscores: `key_container` & `key_file`
 * renamed the `output` property to `out` to match the CLI reference

### docu

 * renamed the `output_location` property to `output_path`

### fluentmigrator

 * removed the `show_help` method, use `$ fluentm.exe /?` on the command line
 * replaced the `output`, `verbose`, & `preview` properties with methods of the same name
 * renamed the `output` & `output_filename` properties to `out` & `output_path` to match the CLI reference

### ilmerge

 * removed the `resolver` property for resolving the installed command path 
 * added the `assemblies` array property instead of using the general `parameters` property (use `assemblies = ["foo", "bar"]` instead of `parameters = ["foo", "bar"]`)
 * renamed the `output` property to `out` to match the CLI reference
 * removed the automatic resolution of an installed ilmerge command, you must provide it yourself or put it on the `PATH`

### msbuild

 * removed infrequently used "special" switches like `max_cpu_count`, use `other_switches` instead (like `other_switches = {:max_cpu_count => 4}`)
 * removed support for "plain" switches (without values), use the general `parameters` property (like `parameters = ["/noconsolelogger"]`)

### msdeploy

 * removed the entire task... it's unmaintainable due to version/platform explosion! Use `exec` & `msbuild` to make and execute msdeploy packages. But, be careful, msdeploy always return exit code 0 (so it cannot be depended on to stop a task).

### mstest

 * replaced the `options` property with the general `parameters` property

### mspec

 * replaced the `html_output` property with a general `results_path` hash property that supports `:xml` & `:html` (like `results_path = {:html => "path/to/results"}`)

### nchurn
 
 * replaced all property-setting methods with properties of the same name (sorry, folks, I liked the style, too, but it has no place in the overall style yet!)
 * split the functionality of the `churn` method supporting integers XOR percentages into two properties: `churn` and `churn_percent`
 * changed the `env_path` property to an array to support multiple assignment and renamed it `env_paths`
 
### ncoverconsole

 * renamed the `cover_assemblies` property to `include_assemblies` (and included pairs `include_foo` & `exclude_foo`)
 * renamed the `testrunner` property to `test_runner`
 * replaced the `register_dll` property with a `no_registration` method
 
### nugetinstall

 * replaced the `no_cache`, `prerelease`, & `exclude_version` properties with methods of the same names

### nugetpack

 * replaced the `symbols` property with a method of the same name
 * renamed the `output` & `base_folder` properties to `output_directory` & `base_path` to match the CLI reference

### nugetpush

 * renamed the `apikey` property to `api_key`

### nunit

 * removed the `options` property, use the general `parameters` property instead
 
### nuspec

 * removed leftover property aliases: `licenseUrl`, `projectUrl`, `iconUrl`, `requireLicenseAcceptance`, use the underscore named properties instead
 * replaced the `require_license_acceptance` property with a method of the same name
 * changed the `authors`, `owners`, & `tags` properties to be arrays of the same names to support multiple values
 * renamed/repurposed the `pretty_formatting?` query method to `pretty_formatting` setter method
 * removed the `working_directory` property and, instead, use the `output_file`'s entire path

### output

 * renamed the `keep_to` property to `preserve`
 * renamed the `directories` property to `dirs`

### plink

 * replaced the `verbose` property with a method of the same name
 
### specflow

 * removed the `options` property, use the general `parameters` property instead
 * renamed the `projects` property to `project` and, in general, changed this task to work on one project at a time
 
### sqlcmd

 * replaced the `trusted_connection` & `batch_abort` properties with methods of the same name
 * set `trusted_connection` & `batch_abort` off by default, the user must explicitly require them
 * removed the automatic resolution of an installed sqlcmd command, you must provide it yourself or put it on the `PATH`

### vssget

 * removed the entire task... if you really need it, use `exec`, but I recommend switching to a modern source control tool :)

### xunit

 * this task now only runs on one `assembly` at a time (like the real CLI app), removed the `assemblies` property and `skip_test_failures` method
 * replaced the `html_output` property with a general `results_path` hash property that supports `:html` and `:xml` (like, `results_path = {:xml => path/to/xml/output}`)
 * removed the `options` property, use the general `parameters` property instead

### zip

 * shortened many property/method names... `directories_to_zip`, `additional_files`, & `flatten_zip` to `dir`, `files`, & `flatten`
 * combined the functionality of the `output_path` & `output_file` property so you can provide just an `output_path`


 [1]: http://albacorebuild.net/2013/09/09/LegacyPreRelease.html
 [2]: https://github.com/Albacore/albacore/wiki
