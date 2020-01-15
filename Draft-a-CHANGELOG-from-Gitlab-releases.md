We are moving our release history from Git[lab|hub] releases to [CHANGELOG.md](https://keepachangelog.com/en/1.1.0/) format.

In order to draft a history from Gitlab releases, we can consume gitlab's api and print to a CHANGELOG file picking only the info we are interested in.

```python
import requests
import json

# Get this id form the "details" or "project overview" page of the web ui.
PROJECT_ID="123456"

HEADER = """# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
"""

RELEASE_T="""## [{}] - {}
### Changed
{}

"""

r = requests.get("https://gitlab.com/api/v4/projects/" + PROJECT_ID + "/releases")
j = json.loads(r.text)                      
f = open("CHANGELOG.md", "w")                                                                                                                 

f.write(HEADER)

for release in j:
    f.write(RELEASE_T.format(release['name'], release['released_at'][:10], release['description']))

f.flush()         
f.close()

```

As you may see, we just dump all the description into "Changed" section. For a proper changelog, you need to manually distribute changes to Added, Changed or Removed. In a dash-style list.