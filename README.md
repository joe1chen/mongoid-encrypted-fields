# mongoid-encrypted-fields

[![CI RSpec Test](https://github.com/joe1chen/mongoid-encrypted-fields/actions/workflows/test.yml/badge.svg?branch=master)](https://github.com/joe1chen/mongoid-encrypted-fields/actions/workflows/test.yml)

Encrypted field types for **Mongoid**. Values are encrypted before they are written to MongoDB and decrypted
transparently when read; equality queries encrypt the search value first, so `where(ssn: '123456789')` just works.

This is the [DOGOnews](https://www.dogonews.com)-maintained fork of
[KoanHealth/mongoid-encrypted-fields](https://github.com/KoanHealth/mongoid-encrypted-fields). The original
authors stopped using MongoDB and asked for a new maintainer; this fork keeps the gem working on current Ruby,
Rails, Mongoid and MongoDB versions.

## Supported versions

Tested on every push by the [GitHub Actions matrix](https://github.com/joe1chen/mongoid-encrypted-fields/actions/workflows/test.yml)
([workflow](.github/workflows/test.yml)):

| Ruby | Rails | Mongoid | MongoDB |
|---|---|---|---|
| 2.7 | 6.1 | 7.5 | 6.0 |
| 3.0 | 6.1 | 8.0 | 6.0 |
| 3.1 | 7.0 | 8.1 | 7.0 |
| 3.2 | 7.1 | 8.1 | 7.0 |
| 3.2 | 7.2 | 9.0 | 7.0 |
| 3.3 | 7.2 | 9.0 | 8.0 |
| 3.4 | 8.0 | 9.0 | 8.0 |
| 2.7 | 6.1 | 7.5 (driver 2.26) | 8.0 |

The gemspec allows `mongoid >= 5`. Mongoid 3/4 and Rails 3.2 were supported by the original gem's 1.x releases.

## Installation

This fork is not published to RubyGems; install it from GitHub, pinned to a release tag
([releases](https://github.com/joe1chen/mongoid-encrypted-fields/releases)):

```ruby
# Gemfile
gem 'mongoid-encrypted-fields', github: 'joe1chen/mongoid-encrypted-fields', tag: 'v2.1.0'
```

## Usage

### 1. Configure a cipher

The gem does not ship a cipher — you "bring your own". Any object that responds to `encrypt(string)` and
`decrypt(string)` works. Set it once at boot (in Rails, e.g. `config/initializers/mongoid_encrypted_fields.rb`):

```ruby
Mongoid::EncryptedFields.cipher = GibberishCipher.new(ENV['MY_PASSWORD'], ENV['MY_SALT'])
```

Ready-made examples are in [`examples/`](examples):
[`GibberishCipher`](examples/gibberish_cipher.rb) (uses the [gibberish](https://github.com/mdp/gibberish) gem),
[symmetric](examples/encrypted_strings_symmetric_cipher.rb) and
[asymmetric](examples/encrypted_strings_asymmetric_cipher.rb) ciphers based on
[encrypted_strings](https://github.com/pluginaweek/encrypted_strings).

> Keep the password/salt (or key) stable: changing them makes existing encrypted values unreadable.

### 2. Use encrypted types on fields

```ruby
class Person
  include Mongoid::Document

  field :name,      type: String
  field :ssn,       type: Mongoid::EncryptedString
  field :birthdate, type: Mongoid::EncryptedDate
end
```

Available types: `Mongoid::EncryptedString`, `Mongoid::EncryptedDate`, `Mongoid::EncryptedDateTime`,
`Mongoid::EncryptedTime`, `Mongoid::EncryptedHash`.

### 3. Read, inspect and query

```ruby
person = Person.new(ssn: '123456789')
person.ssn            # => "123456789"            (decrypted value)
person.ssn.encrypted  # => "<encrypted string>"   (what is stored)
person[:ssn]          # => "<encrypted string>"   (raw attribute)

Person.where(ssn: '123456789').count   # the value is encrypted before querying
```

## Limitations

- One cipher for all encrypted fields.
- Queries support **equality only** (no ranges, regex or partial matches on encrypted values).
- Uniqueness checks on encrypted fields must be **case-sensitive** — a case-insensitive comparison can't work on
  ciphertext.

## Development

```bash
# needs a MongoDB on localhost:27017 (e.g. docker run -p 27017:27017 mongo:8.0)
MONGOID_VERSION=9.0 RAILS_VERSION=8.0 bundle install
MONGOID_VERSION=9.0 RAILS_VERSION=8.0 bundle exec rspec spec
```

`MONGOID_VERSION` and `RAILS_VERSION` select the versions in the `Gemfile` (defaults: Mongoid 7.5, no Rails pin).
To add a combination to CI, add a row to `matrix.include` in `.github/workflows/test.yml`.

## History

Jerry Clinesmith's original (2012, Koan Health) was maintained by KoanHealth through 2.0.0 (2013–2018: Mongoid 3–7,
`EncryptedHash`, the uniqueness validator; 2.0.0 dropped Mongoid 3 and 4) and continued by DOGOnews in this fork:
2.1.0 (2026: Mongoid 7.5–9.x on current Ruby/Rails/MongoDB, tested by a GitHub Actions matrix that replaces the old
Travis setup and per-combination gemfiles).
See [CHANGELOG.md](CHANGELOG.md).

## Related articles

- [Storing Encrypted Data in MongoDB](http://jerryclinesmith.me/blog/2013/03/29/storing-encrypted-data-in-mongodb/)
- [Transparently Adding Encrypted Fields to a Rails App using Mongoid](http://blog.thesparktree.com/post/69538763994/transparently-adding-encrypted-fields-to-a-rails-app)

## Copyright

(c) 2012 Koan Health. Licensed under the MIT license — see [LICENSE.txt](LICENSE.txt).
