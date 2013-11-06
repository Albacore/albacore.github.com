---
layout: post
title: "Legacy Prerelease Announcement"
---

We are finalizing work on the "legacy" Albacore gem. The internals of Albacore 
are pretty messy and need a refresh. We'll be talking more about this later. 
For now, know that we are working to stabilize and release a proper 1.0 version
of Albacore and it will be the _last_ legacy release (except critical bug fixes 
and patches, more on that process later, as well).

Right now, Albacore has a nasty Rubyzip dependency bug that will cause Albacore
not to load. You need to install this version to work around that.

`> gem install albacore --version '~> 1.0.rc'`

or, in your Gemfile

`gem "albacore", "~> 1.0.rc"`

We've been sitting on a handful of new features and other fixes

 * Added the `tag` parameter to the FluentMigrator task
 * Added the `force` parameter to the Unzip task to allow overriding destination files
 * Added the `target_platform` parameter in the ILMerge task
 * Added support for renaming directories in the Output task
 * Added support for adding comments to the top of an Assembly Info file
 * Fixed a file name bug in the Zip task
 * Fixed a `reference` file bug in the NuSpec task
 * Fixed a VB.NET assembly attribute bug in the AssemblyInfo task
 * Fixed the RubyZip 1.0.0 dependency breaking-change bug
 * Changed the Zip task to preserve the directory structure by default. You have to call the `flatten_zip` method explicitly now.
 * Dropped Ruby 1.8.7 and IronRuby support, added Ruby 2.0.0, and targeted the JRuby "head" version
