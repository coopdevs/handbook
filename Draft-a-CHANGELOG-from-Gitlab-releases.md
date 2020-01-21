## Intro

We are moving our release history from Git[lab|hub] releases to [CHANGELOG.md](https://keepachangelog.com/en/1.1.0/) format.

In order to draft a history from Gitlab releases, we can consume gitlab's api and print to a CHANGELOG file picking only the info we are interested in.

## Code

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

## Post-processing in vim

In vim, you may want to adapt the description of each release

### Remove CR chars
They appear as `@M` in vim
```vim
:%s/\r//g
```

### Create link to MR
Convert from `!72` to `[!72](https://gitlab.com/coopdevs/PROJECT_NAME/merge_requests/72)`. Please change `PROJECT_NAME`.
```vim
:%s;!\([0-9]\+\);[!\1](https://gitlab.com/coopdevs/PROJECT_NAME/merge_requests/\1);g
```

### Change list style
In case you used asterysk style, you may should conform to the keepachangelog dash-style.
Replace with a dash all asterisks that begin a line and are followed by a space:
```vim
:%s/^* /- /
```