---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

Gitlab 에서 자동으로 Release Note 를 생성하는 방법을 알아보자.

## Tutorial

https://about.gitlab.com/blog/2023/11/01/tutorial-automated-release-and-release-notes-with-gitlab/

## Example

아래는 태그 생성 시 MR label 에 따라 Release Note 를 생성하는 스크립트다.

```shell
#!/bin/bash

API="$CI_API_V4_URL/projects/$CI_PROJECT_ID/merge_requests?state=merged&milestone=$CI_COMMIT_TAG"
MERGE_REQUESTS_JSON="merge_requests.json"

# Create release_notes.md
rm -f "$RELEASE_NOTE"
touch "$RELEASE_NOTE"

# Get MRs
curl \
--location "$API" \
--header "PRIVATE-TOKEN: $ACCESS_TOKEN" \
--output "$MERGE_REQUESTS_JSON"

# Add release notes
## Features
FEAT_COUNT=$(jq -r '[.[] | select(.labels[] | contains("feat"))] | length' "$MERGE_REQUESTS_JSON")
if [ "$FEAT_COUNT" -gt 0 ]; then
  cat <<EOF >> "$RELEASE_NOTE"
# :rocket: Features
$(jq -r '.[] | select(.labels[] | contains("feat")) | ["* ", .title, "(",  .reference, ") ", "(by @", .author.username, ")"] | add' "$MERGE_REQUESTS_JSON")

EOF
fi

...
```

![](/images/Release_Note_Generator_Sample.png)