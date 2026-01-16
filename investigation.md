## Investigation Summary

Initial reports described a broken preview UI.
By comparing working vs failing conversations, I identified a
strong correlation between content length and UI breakage.

The issue was reproducible independently of content source,
indicating a layout-level failure rather than a data issue.
