# Changelog (Stable)
All stable and notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

!!! note
    To view documentation for a specific version, please click the 'docs | passing' badges under the specific changelog entry. 

[View Beta Releases](beta.md){: .md-button }

## [v0.7.6] - 2021-07-13
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.6)](https://dbtvault.readthedocs.io/en/v0.7.6/?badge=v0.7.6)

- Updated to dbt 0.20.0 and incorporated `adapter.dispatch` changes [(#32)](https://github.com/Datavault-UK/dbtvault/issues/32)

## [v0.7.5] - 2021-06-10
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.5)](https://dbtvault.readthedocs.io/en/v0.7.5/?badge=v0.7.5)

### New structures
- Multi-Active Satellites [Read More](../tutorial/tut_multi_active_satellites.md)

### Bug Fixes
- Fixed a bug where an Effectivity Satellite with multiple DFKs or SDKs would incorrectly handle changes in the corresponding link records, meaning
one to many relationships were not getting handled as intended.

### Improvements
- Added support for multiple `order_by` or `partition_by` columns when creating ranked columns in the `stage` or `ranked_columns` macros.
- Performance improvement for the Satellite macro, which aims to reduce the number of records handled in the 
  initial selection of records from the source data.

## [v0.7.4] - 2021-03-27
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.4)](https://dbtvault.readthedocs.io/en/v0.7.4/?badge=v0.7.4)

### Bug Fixes
- Fixed NULL handling bugs in Hubs, Links and Satellites [(#26)](https://github.com/Datavault-UK/dbtvault/issues/26)
- Fixed a bug where Effectivity Satellites would incorrectly end-date (with auto-end-dating enabled) records other than the
  latest, resulting in duplicate end-date records for previously end-dated records.

### Improvements
- Added check for matching primary key when inserting new satellite records in the sat macro. This removes the requirement to
add the natural key to the hashdiff, but it is still recommended. [Read More](../best_practices.md#hashdiff-components)

### Quality of Life
- Payload in Transactional (Non-Historised) links now optional
- Effective From in Satellites now optional


## [v0.7.3] - 2021-01-28
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.3)](https://dbtvault.readthedocs.io/en/v0.7.3/?badge=v0.7.3)

- Updated dbt to v0.19.0
- Updated dbt utils to 0.6.4


## [v0.7.2] - 2021-01-26
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.2)](https://dbtvault.readthedocs.io/en/v0.7.2/?badge=v0.7.2)

### New

- Derived columns can now be provided lists, for creating composite column values. [(#20)](https://github.com/Datavault-UK/dbtvault/issues/20)
  [Docs](../macros.md#stage-macro-configurations)
  
- The hashed_columns exclude flag in staging can now be provided without a list of columns, and dbtvault will hash everything. [Docs](../macros.md#stage-macro-configurations)

- Rank Load Materialisation: Iteratively load your vault structures over a configured ranking [Read More](../macros.md#vault_insert_by_rank)

- The stage macro now has a new `ranked_columns` configuration section to support the above materialisation. [Read More](../macros.md#stage-macro-configurations)

### Improved

- Optimised Satellite SQL for larger loads (billions) seen in the wild. 
- For non-hashdiff composite hashed_columns: If all components of the key are NULL, then the whole key will evaluate as NULL. 
  [Read more](../best_practices.md#how-do-we-hash)
- Hashing concatenation now uses `CONCAT_WS` instead of `CONCAT`; this is more concise.
- The stage macro has received a big overhaul, and the SQL should now be more efficient and easier to read.
- Optimised table macro SQL across to board by reducing the number of CTEs

### Fixed

- Fixed multiple (minor) bugs in the stage macro [(#21)](https://github.com/Datavault-UK/dbtvault/issues/21)
- Fixed and improved the `adapter.dispatch` implementation [(#22)](https://github.com/Datavault-UK/dbtvault/issues/22)
- Fixed a bug in the vault_insert_by_period materialisation [(#19)](https://github.com/Datavault-UK/dbtvault/issues/19)

### Docs

- Added examples of different ways to provide metadata to the [metadata reference](../metadata.md)
- Added a short guide on [extending dbtvault](../extending.md)
- Updated all SQL snippets to reflect changes

## [v0.7.1] - 2020-12-18
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.1)](https://dbtvault.readthedocs.io/en/v0.7.1/?badge=v0.7.1)

### New

- **exclude_columns** flag for hashdiffs - Inverse the columns selected for creating a hashdiff to select ALL columns except those listed in the metadata.
This is very useful for large multi-column hashdiffs. 
  
!!! note

    See the new [stage macro configurations](../macros.md#stage-macro-configurations) section of the macro docs for more information on the change above.

### Improved

- The stage macro now generates CTE-based SQL instead of one big block. This makes it easier to read and debug. 
  See [here](https://discourse.getdbt.com/t/why-the-fishtown-sql-style-guide-uses-so-many-ctes/1091) for more information on why we've moved to CTEs.
  
- Multi-dispatch implementation now supports a package override variable, providing a smoother experience for 
users wishing to override macro implementations. Documentation will be made available in due course.
  See [Issue #14](https://github.com/Datavault-UK/dbtvault/issues/14) for more details.

- Hashed columns now 'see' columns defined as derived columns. Allowing you to use them in your hashed column definitions.
  [Issue #9](https://github.com/Datavault-UK/dbtvault/issues/9)

### Fixed

- Fixed a bug in the [vault_insert_by_period](../macros.md#vault_insert_by_period) materialization which caused orphaned temporary relations 
  under specific circumstances. [Issue #18](https://github.com/Datavault-UK/dbtvault/issues/18)
  
- Stage macro conversion to CTE fixes [Issue #17](https://github.com/Datavault-UK/dbtvault/issues/17)

- dbt_utils dependency is now explicit [Issue #15](https://github.com/Datavault-UK/dbtvault/issues/15)

## [v0.7.0] - 2020-09-25
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.7.0)](https://dbtvault.readthedocs.io/en/v0.7.0/?badge=v0.7.0)

### New

- Effectivity Satellites: A newly supported Data Vault 2.0 structure.  
[Read more](../tutorial/tut_eff_satellites.md)   
[Macro Reference](../macros.md#eff_sat) 

- Period Load Materialisation: Iteratively load your vault structures over a configured period [Read More](../macros.md#vault_insert_by_period)
- dbt Docs: The built-in dbt docs site (`dbt docs serve`) now includes documentation for dbtvault*. 
- dbt v0.18.0 support [dbt v0.18.0 Release Notes](https://github.com/fishtown-analytics/dbt/releases/tag/v0.18.0)

!!! info
    *This is intended as quick reference and for completeness only, the online documentation is still the main reference documentation.
      
### Improved

- All table macros now make more use of CTEs to reduce nested SQL and improve readability and debugging potential. [Why CTE's?](https://discourse.getdbt.com/t/why-the-fishtown-sql-style-guide-uses-so-many-ctes/1091)
- All macros have had the licence header removed. This was a little messy and unnecessary.

### Removed

- Support for dbt versions prior to v0.18.0 [Upgrading to v0.18.0](https://docs.getdbt.com/docs/guides/migration-guide/upgrading-to-0-18-0/)

## [v0.6.2] - 2020-08-06
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.6.2)](https://dbtvault.readthedocs.io/en/v0.6.2/?badge=v0.6.2)

### Fixed

`dbt_project.yml`

`config-version: 1` caused an error in any dbt version prior to `0.17.x`. We only put this config
in for users making use of variables in their `dbt_project.yml` file. 

Note: If using `vars` in `dbt_project.yml`, you still need to specify `config-version: 1` in your own project's `dbt_project.yml`.
Guidance will be released for alternatives to model-scoped `dbt_project.yml` vars in the next major release of dbtvault (`0.7.x`)

Read more about the [config-version](https://docs.getdbt.com/docs/guides/migration-guide/upgrading-to-0-17-0/) setting.

## [v0.6.1] - 2020-06-24
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.6.1)](https://dbtvault.readthedocs.io/en/v0.6.1/?badge=v0.6.1)

### Added

- dbt 0.17.0 support 
**WARNING** This comes with a caveat that you must use `config-version: 1` in your `dbt_project.yml`

- All macros now support multiple dispatch. This update is to make way for additional platform support (BigQuery, Postgres etc.)

### Changed

- A hashdiff in the stage macro now uses `is_hashdiff` as a flag instead of `hashdiff`, 
this is to clarify this config option as a boolean flag.

### Improved
#### Macros
- Minor macro re-factors to improve readability

### Removed
#### Macros
- Cast macro (supporting) - No longer used. 
- Check relation (internal) - No longer used.

## [v0.6] - 2020-05-26
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.6)](https://dbtvault.readthedocs.io/en/v0.6/?badge=v0.6)

**MAJOR UPDATE**

We've added a whole host of interesting new features.

[Read our v0.5 to v0.6 migration guide](../migration_guides/migrating_v0.5_v0.6.md)

### Added

- Staging has now been moved to YAML format, meaning dbtvault is now entirely YAML and metadata driven.
See the new [stage](../macros.md#stage) macro and the [staging tutorial](../tutorial/tut_staging.md) for more details.

- Renamed `source` metadata configuration to `source_model` to clear up some confusion.
A big thank you to @balmasi for this suggestion.

- `HASHDIFF` aliasing is now available for Satellites
[Read More](../migration_guides/migrating_v0.5_v0.6.md#hashdiff-aliasing)

### Upgraded

- [hub](../macros.md#hub) and [link](../macros.md#link) macros have been given a makeover.
They can now handle multi-day loads, meaning no more loading from single-date views.
We'll be updating the other macros soon, stay tuned!

### Fixed 

- Fixed `NULL` handling when hashing. We broke this in v0.5 ([see related issue](https://github.com/Datavault-UK/dbtvault/issues/5))
[Read more](../best_practices.md#how-do-we-hash)

### Removed

- Deprecated macros (old table template macros) 
- A handful of now unused internal macros- Documentation website from main repository (this makes the package smaller!) 
[New docs repo](https://github.com/Datavault-UK/dbtvault-docs)

## [v0.5] - 2020-02-24

### Added

- Metadata is now provided in the `dbt_project.yml` file. This means metadata can be managed in one place. 
Read [Migrating from v0.4](../migration_guides/migrating_v0.4_v0.5.md) for more information.

### Removed

- Target column metadata mappings are no longer required.
- Manual column mapping using triples to provide data-types and aliases (messy and bad practice).
- Removed copyright notice from generated tables (we are open source, duh!)

### Fixed

- Hashing a single column which contains a `NULL` value now works as intended (related to: [hash](../macros.md#hash), 
[multi_hash](../macros.md), [staging](../macros.md#staging-macros)).   

## [v0.4.1] - 2020-01-08

### Added

- Support for dbt v0.15


## [v0.4] - 2019-11-27
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.4)](https://dbtvault.readthedocs.io/en/v0.4-pre/?badge=v0.4)

### Added

- Table Macros:
    - [Transactional Links](../macros.md#)

### Improved

- Hashing:
    - You may now choose between `MD5` and `SHA-256` hashing with a simple yaml configuration
    [Learn how!](../best_practices.md#choosing-a-hashing-algorithm-in-dbtvault)
    
### Worked example

- Transactional Links
    - Added a transactional link model using a simulated transaction feed.
    
### Documentation

- Updated macros, best practices, roadmap, and other pages to account for new features
- Updated worked example documentation
- Replaced all dbt documentation links with links to the 0.14 documentation as dbtvault
is using dbt 0.14 currently (we will be updating to 0.15 soon!)
- Minor corrections

## [v0.3.3-pre] - 2019-10-31

### Documentation

- Added full demonstration project/worked example, using snowflake. 
- Minor corrections

## [v0.3.2-pre] - 2019-10-28

### Bug Fixes

- Fixed a bug where the logic for performing a base-load (loading for the first time) on a union-based hub or link was incorrect, causing a load failure.

### Documentation

- Various corrections and clarifications on the macros page.

## [v0.3.1-pre] - 2019-10-25

### Error handling

- An exception is now raised with an informative message when an incorrect source mapping is 
provided for a model in the case where a source relation is also provided for a target mapping. 
This caused missing columns in generated SQL, and a misleading error message from dbt. 

## [v0.3-pre] - 2019-10-24

### Improvements

- We've removed the need to specify full mappings in the `tgt` metadata when creating table models.
Users may now provide a table reference instead, as a shorthand way to keep the column name 
and date type the same as the source.
The option to provide a mapping is still available.

- The check for whether a load is a union load or not is now more reliable.

### Documentation

- Updated code samples and explanations according to new functionality
- Added a best practises page
- Various clarifications added and errors fixed

## [v0.2.4-pre] - 2019-10-17

### Bug Fixes

- Fixed a bug where the target alias would be used instead of the source alias when incrementally loading a hub or link,
causing subsequent loads after the initial load, to fail.


## [v0.2.3-pre] - 2019-10-08

### Macros

- Updated [hash](../macros.md#hash) and [multi-hash](../macros.md)
    - [hash](../macros.md#hash) now accepts a third parameter, `sort`
    which will alpha-sort provided columns when set to true.
    - [multi-hash](../macros.md) updated to take advantage of
    the the [hash](../macros.md#hash) functionality.

### Documentation

- Updated [hash](../macros.md#hash) and [multi-hash](../macros.md) according to new changes.

## [v0.2.2-pre]  - 2019-10-08

### Documentation

- Finished Satellite page
- Added Union sections to Hub and Link pages
- Updated staging page with Satellite fields
- Renamed `stg_orders_hashed` back to `stg_customers_hashed`

## [v0.2.1-pre] - 2019-10-07
[![Documentation Status](https://readthedocs.org/projects/dbtvault/badge/?version=v0.2.1-pre)](https://dbtvault.readthedocs.io/en/v0.2.1-pre/?badge=v0.2.1-pre)

### Documentation

- Minor additions and corrections to documentation:
    - Fixed website URL in footer
    - Added contribution page to docs
    - Corrected version in dbt_project.yml

## [v0.2-pre] - 2019-10-07

[Feedback is welcome!](https://github.com/Datavault-UK/dbtvault/issues)
 
### Improved
Read the linked documentation for more detail on how to take advantage of
the new and improved features.

- Table Macros:
    - All table macros now no longer require the `tgt_cols` parameter.
    This was unnecessary duplication of metadata and removing this now makes
    creating tables much simpler.
    
- Supporting Macros:
    - [add_columns](../macros.md)
        - Simplified the process of adding constants.
        - Can now optionally provide a [dbt source](https://docs.getdbt.com/docs/using-sources) to automatically
        retrieve all source columns without needing to type them all manually.
        - If not adding any calculated columns or constants, column pairs can be omitted, enabling you to provide the 
        source parameter above only.
    - [hash](../macros.md#hash) now alpha-sorts columns prior to hashing, as
    per best practises. 
   
- Staging Macros:
    - staging_footer renamed to [from](../macros.md) and functionality for adding constants moved to
    [add_columns](../macros.md)
    - [multi-hash](../macros.md)
        - Formatting of output now more readable
        - Now alpha-sorts columns prior to hashing, as
          per best practises. 

## [v0.1-pre] - 2019-09 / 2019-10

### Added

- Table Macros:
    - Hub
    - Link
    - Satellite

- Supporting Macros:
    - cast
    - hash
    - prefix

- Staging Macros:
    - add_columns
    - multi_hash
    - staging_footer

### Documentation
   
- Numerous changes for version 0.1 release
