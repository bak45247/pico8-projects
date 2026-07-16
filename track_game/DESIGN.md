# Track & Field Game — Control Design

## General Philosophy

In PICO-8, you don't have analog sticks or precise movement controls. The best approach is **timing-based actions** (like a rhythm game) combined with simple directional input. This keeps it accessible but skillful.

---

## High Jump (Vertical Jump / Fosbury Flop)

| Phase | Input | Description |
|-------|-------|-------------|
| Run up | →/← arrows | Approach from either side of the bar |
| Take-off | ↑ (jump) or R | Timing is everything — too early = short arc, too late = over-rotating |
| Tuck | Z or A | Mid-air tuck to clear the bar. Hold longer = wider clearance angle |

**Idea:** Add a "run speed" meter. Press → while running to accelerate. Jump at the right moment. Tucking mid-air lets you clear taller bars but risks under-rotating (faceplant). It's about finding the sweet spot between approach speed and tuck timing.

---

## Long Jump

| Phase | Input | Description |
|-------|-------|-------------|
| Run | → | Sprint phase — pressing when you see "GO" starts your run |
| Plant foot & take off | A (left) or B (right) | Left = left-foot plant, Right = right-foot plant. Each side has different arc characteristics |
| Flight | ↑ | Jump height — more aggressive jump = longer hang time but risks landing short |
| Extend arms/legs mid-air | ←/→ | Slight forward lean to push the landing spot further out |

**Idea:** The angle of take-off matters. A left-foot plant gives you a 15° arc, right foot ~20°. Mid-air control is limited but pressing ← during flight does a quick hip tuck (better hang time). Pressing → extends your body forward (pushes landing further out).

---

## Pole Vault

This one's the most interesting — it has natural multi-phase structure:

| Phase | Input | Description |
|-------|-------|-------------|
| Approach | Hold any arrow | Run with pole. Speed affects vault height potential |
| Plant & plant | A (left) or B (right) | Plant left/right side of the pit. Right plant = deeper bend in pole, more energy stored |
| Swing up | ↑ / Z | Start pulling yourself up off the plant position |
| Over the bar | Hold spacebar | The higher you pull, the further over. But too long and you under-rotate at the top |
| Release | R (at top) | Let go to drop down safely into the pit. Too early = hitting the bar, too late = fall back |

**Idea:** Add a "pole bend" visual indicator that fills as you approach. You *must* plant in time with it — like a rhythm gate. This creates natural tension: if you rush, pole is too straight and doesn't store energy. If you wait, the meter expires and the vault fails.

---

## Triple Jump (Hop-Step-Jump)

This has three distinct phases and is great for layered input:

| Phase | Input | Description |
|-------|-------|-------------|
| Hop (Phase 1) | ↑ or jump button | Take off from standing/start run. Timing affects hop distance |
| Step (Phase 2) | →/← during flight of phase 1 | Press to extend the step — forward lean pushes you further, backward leans reduce momentum |
| Jump (Phase 3) | R at end of phase 2 window | Release into final flight mode. Earlier release = less energy out of step |

**Idea:** Triple jump can be broken down if you mistime. You start in "hop mode" automatically, then transition to "step mode," then must commit to "jump mode." Each phase has a timing window — pressing too early or too late causes the previous phase to fail and the next one resets (you lose distance). It's a cascade of mini-time-bonuses: perfect hop → +10%, step within window → another %, commit jump in time → bonus %.

---

## Controller Layout Suggestion

Here's what I'd recommend for consistent play across all events:

```
Arrows       = Movement / Direction
A/B          = Plant foot (jump) or Release
R            = Commit/Turn (vault release, triple jump transition)
Z            = Tuck/Swing up (helpful in high vault & vault)
Space        = Hold to clear bar (high jump) / Release (pole vault)
←/→ mid-air  = Lean control during flight phase
```

---

## Notes for PICO-8 Implementation

- PICO-8 is limited to ~16×16 pixel sprites and a small palette, so keep visual complexity tight. Use simple geometric shapes — circles, lines, dots — instead of trying to draw realistic athletes.
- Sound matters: simple beep/boop from PICO-8's audio channel can be used for rhythm feedback (timing hits = nice pitch).
- Screen mode 1 or mode 4 give you the most horizontal space. Mode 6 is good if you want full-screen use of all four keys at once.
