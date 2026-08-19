---
'@posthog/rrweb': patch
---

Stop replay playback from failing on Safari when a recording adopts constructed stylesheets. A seek can rebuild the replayer iframe while a retry still holds sheets built against the old document, which Safari rejects. The replayer now re-homes each such sheet on the adopting document and degrades to unstyled shadow content if adoption is still rejected, instead of throwing.
