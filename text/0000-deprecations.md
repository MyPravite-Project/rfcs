- Feature Name: deprecations
- Start Date: 2016-02-15
- RFC PR: (leave this empty)
- Grunt Issue: (leave this empty)

# Summary
[summary]: #summary

Create a deprecation API and mark all the APIs in Grunt 1.x.x that are deprecated.

# Motivation
[motivation]: #motivation

Many APIs have been deprecated but it's not entirely clear nor communicated to
the end users. During 1.x.x, we can add a deprecation API that hooks into any
deprecated API and logs a message informing the end user.

# Detailed design
[design]: #detailed-design

A tool to mark an API as deprecated could be:

```js
grunt.deprecate(grunt.util._.str, 'grunt.util._ has been deprecated. Please use lodash directly: https://gruntjs.com/migration-guide#lodash');
```

Then if any plugin or Gruntfile uses that API, it will log the message:

```shell
WARN: grunt.util._ has been deprecated. Please use lodash directly: https://gruntjs.com/migration-guide#lodash
```

These warnings will persist until the API is removed in the next major version
release. Deprecations can be added on minor versions.

> Would be nice to display the file location and line the deprecation originated
> from as well.

### Disable Deprecation Notices

All deprecation notifications can be disabled with a flag:

```js
grunt.deprecate.disable = true;
```

For users not ready to fix the deprecations or annoyed by deprecation messages
originating from other plugins they cannot yet easily resolve.

### 1.x.x Deprecations

The following APIs are suggested to be deprecated in `1.x.x` and then removed
in `2.x.x`:

* All functions on `grunt.util._`
* All functions on `grunt.util.async`
* All functions on `grunt.util.namespace`
* All functions on `grunt.util.hooker`
* `grunt.util.spawn`
* `grunt.util.exit`
* `grunt.util.callbackify`
* `grunt.util.kindOf`
* `grunt.util.toArray`
* `grunt.util.repeat`
* `grunt.util.pluralize`
* `grunt.util.recurse`
* All functions on `grunt.file.glob`
* All functions on `grunt.file.minimatch`
* All functions on `grunt.file.findup`
* `grunt.file.readYAML`
* `grunt.file.readJSON`
* All functions on `grunt.event`

# Drawbacks
[drawbacks]: #drawbacks

* Providing a way to disable deprecations may defeat the purpose of this API if
  users choose to ignore it.

# Alternatives
[alternatives]: #alternatives

* Just remove the APIs without notifying end users. Some of the APIs have been
  deprecated for quite some time and communicated via blog posts and docs.

# Unresolved questions
[unresolved]: #unresolved-questions

* Easier way to deprecate every function on `grunt.util._.*` and `grunt.util.async.*`
  without having to list every function?
* Any more APIs we should deprecate? Any less?
* Should this be a private Grunt API?