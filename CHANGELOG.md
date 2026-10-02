# Changelog

All notable changes to this project are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed
- CI: test Mongoid 7.5 with Ruby driver 2.26 against MongoDB 8.0 (Ruby 2.7 / Rails 6.1).

## [2.1.0] - 2026-10-01
DOGOnews fork. Minor version because the supported Mongoid range is unchanged (`>= 5`, as in 2.0.0) and the public
API did not change.

### Added
- GitHub Actions test matrix (`.github/workflows/test.yml`), seven rows from Ruby 2.7 / Rails 6.1 /
  Mongoid 7.5 / MongoDB 6.0 to Ruby 3.4 / Rails 8.0 / Mongoid 9.0 / MongoDB 8.0, all passing without library
  changes. The `Gemfile` selects Mongoid and Rails from `MONGOID_VERSION` / `RAILS_VERSION` (default Mongoid 7.5,
  no Rails pin).
- GitHub Release workflow (`.github/workflows/release.yml`): pushing a `vX.Y.Z` tag creates a GitHub Release
  with this file's section as the notes.

### Changed
- The gemspec `homepage` points to this fork.
- README rewritten for the maintained fork (supported versions, cipher setup, field types, limitations); history
  moved to this file, now in Keep a Changelog format.

### Removed
- Travis CI configuration and the per-combination gemfiles in `spec/gemfiles`.

## [2.0.0] - 2018-05-02
### Changed
- Runtime dependency `mongoid >= 5` (was `>= 3`); requires Ruby 2.0 or newer (was 1.9).

### Removed
- Mongoid 3 and 4 support (use 1.x), including the patched uniqueness validator for them. Encrypted fields cannot
  use case-insensitive uniqueness validations.

## [1.3.7] - 2018-05-02
Tagged upstream but never released to RubyGems.
### Added
- Mongoid 7 and later: the uniqueness validator patch is loaded for every Mongoid version from 4 on.

## [1.3.6] - 2017-07-08
### Fixed
- Gem load error: `Mongoid::VERSION::MAJOR` no longer exists (#28).
- Backwards support for Ruby < 2 kept; updated for changes to the Gibberish gem and to rake (`last_comment`
  removed).

## [1.3.5] - 2016-09-13
### Added
- Mongoid 6 support (#25).

## [1.3.4] - 2015-06-11
### Added
- Mongoid 5 support (#21).

### Fixed
- `field_type` is checked for `nil` (#22).

## [1.3.3] - 2015-01-19
### Removed
- The deprecated `Validator#setup` method (#19).

## [1.3.2] - 2014-11-18
### Fixed
- Updated for changes in ActiveModel 4.2 (#17).

### Added
- Test gemfiles for Mongoid 4, Mongoid 4 with Rails 4.1, and Mongoid 4 with Rails 4.2.

## [1.3.1] - 2014-04-21
### Fixed
- Updated for changes in Mongoid 4 and ActiveModel 4 (#14).

## [1.3.0] - 2013-12-09
Breaking change - please read.
### Added
- Mongoid 4 support (#11).

### Changed
- `EncryptedHash` stringifies its keys before storing, consistent with Mongoid's behaviour (#12).

## [1.2.2] - 2013-08-13
### Added
- Aliased fields (`:as`) work with the uniqueness validator
  ([pull request #10](https://github.com/KoanHealth/mongoid-encrypted-fields/pull/10), @johnnyshields).

## [1.2.1] - 2013-04-22
### Added
- The uniqueness validator works with encrypted fields; it raises if the case-insensitive option is used on an
  encrypted field.

## [1.2.0] - 2013-03-19
### Added
- `EncryptedHash` ([pull request #4](https://github.com/KoanHealth/mongoid-encrypted-fields/pull/4), @ashirazi).

## [1.1.0] - 2013-01-31
Breaking changes - please read.

During performance testing, the [encrypted-strings](https://github.com/pluginaweek/encrypted_strings) gem was found
to be very slow under load, and it patches `String#==`, so it has been removed as the default cipher option.

[Gibberish](https://github.com/mdp/gibberish) was found to be very fast, but uses a unique salt for each encryption.
That is normally great for security, but becomes problematic for searching encrypted fields in Mongoid, because
each search would generate a unique value that would not match what is stored in the database.

The **examples** folder shows implementations using either gem, but only a class that implements **encrypt** and
**decrypt** is required, so any gem can be used.

### Removed
- The included cipher implementations.
- The [encrypted-strings](https://github.com/pluginaweek/encrypted_strings) gem dependency.

## [1.0.0] - 2012-12-23
### Changed
- First release to RubyGems; runtime dependencies specified (`mongoid ~> 3`, `encrypted_strings ~> 0.3`).

## [0.0.6] - 2012-11-14
### Changed
- Minor refactoring.

## [0.0.5] - 2012-11-10
### Added
- `EncryptedDateTime` and `EncryptedTime`; logging.

## [0.0.4] - 2012-11-10
### Changed
- Field classes renamed from `Encrypted::String` / `Encrypted::Date` to `EncryptedString` / `EncryptedDate`;
  encrypted values carry an identifier so encrypted strings can be recognised.

## [0.0.3] - 2012-11-09
### Changed
- Namespaces changed; the `encrypted_strings` dependency is hidden from consumers of the gem; mutable types
  allowed.

## [0.0.2] - 2012-11-08
### Added
- `EncryptedDate`; specs with a test model.

## [0.0.1] - 2012-11-04
### Added
- Initial version by Jerry Clinesmith (Koan Health): `EncryptedString`.

[Unreleased]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v2.1.0...HEAD
[2.1.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.7...v2.0.0
[1.3.7]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.6...v1.3.7
[1.3.6]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.5...v1.3.6
[1.3.5]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.4...v1.3.5
[1.3.4]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.3...v1.3.4
[1.3.3]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.2...v1.3.3
[1.3.2]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.1...v1.3.2
[1.3.1]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.3.0...v1.3.1
[1.3.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.2.2...v1.3.0
[1.2.2]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.2.1...v1.2.2
[1.2.1]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.6...v1.0.0
[0.0.6]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.5...v0.0.6
[0.0.5]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.4...v0.0.5
[0.0.4]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.3...v0.0.4
[0.0.3]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.2...v0.0.3
[0.0.2]: https://github.com/joe1chen/mongoid-encrypted-fields/compare/v0.0.1...v0.0.2
[0.0.1]: https://github.com/joe1chen/mongoid-encrypted-fields/releases/tag/v0.0.1
