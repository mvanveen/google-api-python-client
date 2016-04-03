# How to become a contributor and submit patches #

## Contributor License Agreements ##

We'd love to accept your code patches! However, before we can take them, we have to jump a couple of legal hurdles.

Please fill out either the individual or corporate Contributor License Agreement.

  * If you are an individual writing original source code and you're sure you own the intellectual property, then you'll need to sign an [individual CLA](http://code.google.com/legal/individual-cla-v1.0.html).
  * If you work for a company that wants to allow you to contribute your work to Google APIs Client Library for Python, then you'll need to sign a [corporate CLA](http://code.google.com/legal/corporate-cla-v1.0.html).

Follow either of the two links above to access the appropriate CLA and instructions for how to sign and return it. Once we receive it, we'll add you to the official list of contributors and be able to accept your patches.

## Submitting Patches ##

  1. Join Google API Client for Python  [discussion group](https://groups.google.com/group/google-api-python-client/).
  1. Decide which code you want to submit. A submission should be a set of changes that addresses one issue in the Google API Client for Python [issue tracker](http://code.google.com/p/google-api-python-client/issues/list). Please don't mix more than one logical change per submittal, because it makes the history hard to follow. If you want to make a change that doesn't have a corresponding issue in the issue tracker, please create one.
  1. Also, coordinate with team members that are listed on the issue in question. This ensures that work isn't being duplicated and communicating your plan early also generally leads to better patches.
  1. Ensure that your code adheres to the PEP 8 style, with the exception of spacing, which should be two spaces.
  1. Ensure that there are unit tests for your code.
  1. Sign a Contributor License Agreement.
  1. Use the `upload-diffs.py` script to upload your patches to [codereview.appspot.com](http://codereview.appspot.com) for review.


## Submitting a sample ##

Same procedure as for submitting patches, but with the additional provisions:

  1. Add a README file following the format below.
  1. Start with the Google+ sample as a template for how your sample should be structured.

### README File Format ###

The information for the SampleApps wiki page is built from data found in all the README
files in the samples. The format of the README file is:

```
   Description is everything up to the first blank line.

   api: plus  (Use the name in discovery).
   keywords: appengine (such as appengine, oauth2, cmdline)
   author: Your name (email address is optional)

   The rest of the file is ignored when it comes to building the index.
```

The list of valid keywords is defined here:

> http://code.google.com/p/google-api-python-client/source/browse/samples-index.py#51

### Using `upload-diffs.py` and the code review site ###

**Note** that if you have 2-factor authentication turned on for your account then you will need to go to https://google.com/accounts, Select Security Tab, then Edit "Authorizing applications and sites" and add an application specific password for upload-diffs.py.

In most cases running

```
$ python upload-diffs.py
```

will Do The Right Thing without any additional input. `upload-diffs.py` is a streamlined version of `upload.py` created by the [Rietveldt project](http://code.google.com/p/rietveld), and the documents on their wiki plus `upload-diffs.py --help` can help if you need more detailed instructions. There is also an article [here](http://code.google.com/p/rietveld/wiki/CodeReviewHelp) on how to go through the code review process.

Once the code review has started you may be asked to make changes to your code. Once you have made those changes run `upload-diffs.py` again to update the patch. You will need to know the codereview issue number. Assuming it is 123456:

```
$ python upload-diffs.py -i 123456
```


### Reviewing ###

If you are doing a review the easiest way to work with patches is [mercurial queues](http://mercurial.selenic.com/wiki/MqExtension). If we assume the codereview issue is again 123456, in the upper right-hand corner of the code review page is a "Download raw patch set" link, which is the URL of the patchset to import:

```
$ hg qimport http://codereview.appspot.com/download/issue123456_10000.diff
```

You can then `hg qpush` the patch into your repository and when you are done `hq qpop` it back out again. Once the patch is in good shape you can `hg qfinish` it to commit it to the repository.

## Submitting Your Approved Code ##

First pull and rebase to avoid branch/merge issues:

```
$ hg pull --rebase
```

When you submit make sure to add a link to where the code was reviewed in the commit message, along with a good description of the change:

```
This change blah blah blah.

Reviewed in http://codereview.appspot.com/123456/.
```

If there is an associated issue also add this line
to the commit message, which will automatically mark
the issue as closed.

```
Fixes issue #nnnn.
```

Once the issue is committed, go back and add a link to the commit to
the review, and mark the review as completed.