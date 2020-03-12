# kb_templater_pl

Module for rendering templates using [Template Toolkit](http://www.template-toolkit.org). This is intended for use with KBase apps (hence the module naming) but can be used anywhere.

## Installation

* Copy `Templater.pm` into `lib/Bio/KBase` of your app
* Copy the contents of `t` into the `test` directory
* Copy the `templates` directory into your root directory
* Edit your `cpanfile` to include the module prerequisites:

```
requires 'Cpanel::JSON::XS', '>= 4.19';
requires 'JSON::MaybeXS', '>= 1.004000';
requires 'Path::Tiny', '>= 0.112';
requires 'Template::Plugin::JSON', '>= 0.08';
requires 'Template', '>= 2.26';
requires 'Test::Most', '>= 0.35';
requires 'Test::Output', '>= 1.031';
requires 'Try::Tiny', '>= 0.30';
```

If your app does not follow modern Perl conventions for specifying prerequisites, directory organisation, and basic testing, please read the advice in [ModernPerl.md](ModernPerl.md).

## Usage

### In an app:

```
package MyApp;

use strict;
use warnings;
use feature qw( say );
use Bio::KBase::Templater;

my $data_for_template = {
  title => '2020 Spring Sale',
  price => {
    gold        => 100,
    platinum    => 200,
    kryptonite  => 300,
  }
};

# save the rendered template as a string
my $string;
Bio::KBase::Templater::render_template(
    '/path/to/template/file.tt',
    { template_data => $data_for_template },
    \$string,
);

say $string;

# or to a file
Bio::KBase::Templater::render_template(
    '/path/to/template/file.tt',
    { template_data => $data_for_template },
    '/path/to/output/file.txt',
);

```

### Sample template

```
[% WRAPPER standard_page.tt %]
<h3>[% title %]</h3>

<table>
  <thead>
    <tr>
      <th>Substance</th><th>Cost per ounce</th>
    </tr>
  </thead>
  <tbody>
  [% FOR substance IN price.keys %]
    <tr>
      <td>[% substance %]</td>
      <td>[% price.$substance %]</td>
    </tr>
  [% END %]
  </tbody>
</table>
[% END %]
```
