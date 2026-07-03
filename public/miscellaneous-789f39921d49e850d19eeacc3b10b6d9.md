# Miscellaneous

## Force Install

```none
pip install --upgrade --no-deps --force-reinstall [package name]
```

## Future API Improvements

The following items were identified during the interparticle-interference verification work
(see `modeling-vs-model_free/explorations/interparticle_quadratic_verification.ipynb`)
and are recorded here for future consideration.

### 1. Convenience Wrapper for `compute_scd`

**Problem**: Computing the SCD (Score of Concentration Dependency) currently requires constructing
a `DecompositionProxy` object and calling `compute_scds_impl` from `molass.Backward.RankEstimator`.
There is no simple function that accepts a data path or `SecSaxsData` and returns an SCD score.

**Proposed fix**: Add a convenience function (e.g., in `molass.InterParticle` or `molass.Backward`)
that wraps the full pipeline:

```python
def compute_scd_from_data(ssd_or_path, debug=False):
    """Compute SCD directly from a SecSaxsData object or data folder path."""
    ...
```

This would also unify with the placeholder in `molass.InterParticle.ConcDepend`,
which currently returns 0 (the real implementation is in `molass_legacy.Conc.ConcDepend`).

### 2. High-Level API Exports from `molass.__init__`

**Problem**: `molass.__init__.py` only exports `get_version`. Users must discover
and import individual modules (e.g., `from molass.DataObjects import SecSaxsData`).
Tutorial notebooks, the tutorial book, and test scripts all use these explicit imports,
but newcomers have no single entry point.

**Proposed fix**: Re-export the most common user-facing symbols from `molass.__init__`:

```python
# molass/__init__.py additions
from molass.DataObjects import SecSaxsData
from molass.LowRank import Decomposition
from molass.Guinier import RgCurve
```

**Caveat**: This changes the public API surface. Should be coordinated with a minor version bump
and verified against all tutorial/test imports to ensure no circular-import issues.