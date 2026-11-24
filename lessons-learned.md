# Lessons learned

What broke, and what I changed.

## Silent configuration

Open WebUI stores many settings in its database after first launch, so edits to the environment file were ignored.

**Now:** I verify effective settings at the data layer instead of trusting the UI.

## Format mismatch

The chat interface and the reranker server spoke different request formats.

**Fix:** a small translation proxy bridged them.

## Hardware stability

Crashes traced back to Intel 13th and 14th gen CPU degradation.

**Fix:** I updated the microcode and had the earlier CPU replaced.

## Fragile boot chain

On the Jetson, upgrading the wrong packages can break boot.

**Fix:** I hold those packages.

## Platform choice

I tried TrueNAS first, then rebuilt on Proxmox with one container per service.

[Add one sentence on why.]
