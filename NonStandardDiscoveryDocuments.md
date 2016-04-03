# Introduction #

By default the library uses the API name and version number and looks up the discovery document that describes the API on `https://www.googleapis.com`. There are several ways to over-ride that behavior.

# URI Template #

You can supply a different URI Template to the library that it will then use to look up APIs. The template variable names must be 'api' and 'apiVersion'.

```
DISCOVERY_URI = ('http://localhost:3990/discovery/v1/apis/'
  '{api}/{apiVersion}/rest')

service = build("plus", "v1", discoveryServiceUrl=DISCOVERY_URI)
```


# Local Discovery Docs #

The second way is to keep a copy of the discovery document locally and pass
that in when building a service:

```
# Load the local copy of the discovery document
f = file(os.path.join(os.path.dirname(__file__), "plus.json"), "r")
discovery = f.read()
f.close()

# Construct a service from the local documents
service = build_from_document(discovery,
    base="https://www.googleapis.com/",
    http=http)
```

Note that you need to also provide the base parameter, which is the base URI to
which all the API calls are relative.