

# Design #

At the highest levels there are three kinds of objects: Credentials,
Flows and Storage.

## Flow ##
Flows are responsible for going through the steps of
an OAuth dance. Upon the successful completion a Flow returns a
Credentials object.

Credentials objects a method, `set_store()`, which
takes a callback for storing an updated Credential back to where it came from. For example,
if an OAuth 2.0 Credentials object is stored in a file then when the `access_token`
expires the client goes through the process of refreshing the `access_token` and the
whole Credentials object must be written back into that file. The argument to
`set_store` is a callable that takes an OAuth 2.0 Credentials object as its only argument.

## Credentials ##
A Credentials object is responsible for
authorizing an `httplib2.Http()` instance so that it can make
requests using said credentials.

## Storage ##
Storage objects can correctly save and restore Credentials to and from a specific medium. They have `get()` and `put(credentials)` methods for retrieving and storing Credentials respectively. The `put(credentials)` method is able to be passed in as the argument to the `Credentials.set_store()` method.

# Library #

The `oauth2client` library can be used as a standalone library against
any OAuth 2.0 service and has many features to make it easy to
with OAuth 2.0.

## Storage ##

The `oauth2client.file.Storage` and `oauth2client.appengine.StorageByKeyName`
allow for easy storage and retrieval of credentials. The storage
helper classes also call `set_store` on the credentials so they
can store themselves back to where they came from if their token
gets refreshed.

Here the file Storage reads the Credentials from the
file `moderator.dat` and sets up the `set_storage` callback
so that if the access token expires and a fresh access
token is acquired it is stored back in the file `moderator.dat`.

```
from oauth2client.file import Storage
import httplib2

credentials = Storage('moderator.dat').get()

http = httplib2.Http()
http = credentials.authorize(http)
```

### Creating a Storage ###

The supplied `file.Storage` and `StorageByKeyName` classes may not
be sufficient for your needs. To do that just subclass from
`oauth2client.client.Storage` and implement the `locked_get()`, `locked_put()`,
and `locked_delete()`
methods. The important point to remember is when implementing
`locked)get()`, after the Credentials has been instantiated, to call
`set_store(self)`, so that when the OAuth 2.0 credentials
have been refreshed they get stored back for the next use.
You may also override `acquire_lock()` and `release_lock()` to implement
the locking appropriate for the storage.

## Properties ##

The `oauth2client.appengine` and `oauth2client.django_orm` modules
contain helper properties to make it easy to store and retrieve
Credentials and Flows to the datastores in App Engine and Django
respectively. To store a Credential in App Engine is as easy as
adding a property to a db model:

```
from oauth2client.appengine import CredentialsProperty
from google.appengine.ext import db


class Credentials(db.Model):
  credentials = CredentialsProperty()
```

Similarly for Django:

```
from django.contrib.auth.models import User
from django.db import models
from apiclient.ext.django_orm import OAuthCredentialsField


class Credential(models.Model):
  id = models.ForeignKey(User, primary_key=True)
  credential = OAuthCredentialsField()
```