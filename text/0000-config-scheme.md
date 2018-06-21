- Feature Name: config-scheme
- Start Date: 2018-06-21
- RFC PR:
- Redox Issue:

# Summary
[summary]: #summary

Design a scheme that allows reading from (and writing to) configuration files.

# Motivation
[motivation]: #motivation

- Language-independent (compared to the usage of various language-specific libraries to deal with configuration)
- Redox could have a bit more sway to standardize on some configuration file format 
- Much easier to describe configuration changes to users

# Detailed design
[design]: #detailed-design

Consider some app has a config file at `.config/some-app/settings.ini` with the following contents:

    [Bargle]
    foo=baz

Now such a config scheme could allow reading and writing this configuration using the URL

    config:some-app/settings.ini#Bargle/foo

where `settings` is the filename, and `Bargle` is the section in the file, and `foo` is a key in that section.

Asking users to change some configuration would change from something like

  > "enable 'show hidden files' in your file manager, browse to .config/some-app, open the file settings.ini in your editor and search for the "debug" key and replace the part after the "=" with "true"

to

  > echo "true" > config:some-app/settings.ini#debug

I believe it would be manageable to pick the most common configuration formats, focused on maximizing the coverage of applications in the wild.

# Drawbacks
[drawbacks]: #drawbacks

- Writing to config files while preserving comments, indentation and other stylistic features might be hard/time-consuming/annoying to implement.
- It could turn out that the number of different file formats that would be needed for a reasonable coverage of applications is substantially higher than expected.

# Alternatives
[alternatives]: #alternatives

Status quo – don't ship an OS-provided standard for dealing with configuration and let each library/framework/... come up with its own approach.

# Unresolved questions
[unresolved]: #unresolved-questions

A `config:` scheme would need to support common config formats like ini, yaml, json, xml, toml – there are many different formats (including home-grown ones) and it's not obvious how many different format would need to be supported to provide a substantial coverage of applications.
