# Data

Video clips and processed tracks are **not** stored in git (too large). Put them in the
shared Drive folder and keep this folder layout locally:

```
data/raw/<YYYY-MM-DD>/<clip_id>.mov     # original 240 fps phone clips
data/processed/tracks/<clip_id>.csv     # per-frame ball position (tracker output)
```

**Ground truth lives in the shared sheet** (Balls, Drop tests, Friction, Clips tabs).
Use the same Ball IDs everywhere, e.g. `VB-REG`, `VB-FOAM`, `BB-REG`, `BB-SAND`, `SC-REG`, `SC-WT`.

Clip IDs: `<ball_id>_<type>_<NNN>`, e.g. `VB-FOAM_throw_007`. Log every clip in the Clips tab
the day it is filmed, including its split (pilot / train / test). The test split is frozen on
23 Dec and never touched for tuning afterwards.
