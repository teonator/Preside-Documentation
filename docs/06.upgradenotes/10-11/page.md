---
id: 10-11-upgrade-notes
title: Upgrade notes for 10.10 -> 10.11
---

## Lucee version

Bugs in earlier versions of Lucee 5 mean that Preside 10.11 may refuse to start. The earliest known Lucee 5 version to work with Preside is Lucee **5.2.9.20**. However, we recommend running at least **5.3.3.63**. We no longer recommend running Lucee 4.5.

## CfConcurrent

A new mapping was added to **10.11.0**, `/cfconcurrent`. Unfortunately, this mapping actually already existed but pointed to an empty directory. This may cause the need for a Lucee restart after upgrading from a previous version.

If you see the error, `invalid component definition, can't find component [cfconcurrent.ExecutorService]`, you will need to restart Lucee.

## Asset file names

In 10.11.0, we introduced a feature to save assets and derivatives using a configured file name. By default this is set as `$slugify( title )` when the asset is uploaded. Content editors are able to edit this file name in the admin and this results in a file name change.

**Existing assets are not automatically renamed when upgrading**. If you want to automate this, you will need to provide a script that renames each asset that is not already renamed. This script should use the code below to ensure files are renamed and moved in the process:

```luceescript
assetManagerService.editAsset( id=asset.id, data={ file_name=myGeneratedFileName } );
```

## Asset queue

10.11.0 introduced the concept of the [[enabling-asset-queue|Asset processing queue]] however it is disabled by default. We highly encourage you to enable it and test as early as possible. See the full guide: [[enabling-asset-queue]].

## Cache configurations

Several changes were made to caching in Preside. Key headlines that you should be aware of:

1. Full page cache, `presidePageCache` changed from a memory storage to _disk_ storage that saves to the Lucee tmp directory by default
2. Several caches were removed entirely due to not really being caches
3. A configuration option was added to allow preside objects to each have their own query cache. This is disabled by default and we recommend turning it on and configuring. See: [Cache per object](https://docs.preside.org/devguides/dataobjects.html#cache-per-object).

We recommend reviewing `/preside/system/config/Cachebox.cfc` against your own `/application/config/Cachebox.cfc` to check for any issues that might arise from the changes.

