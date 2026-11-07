# Ocarina of Time visual compatibility correction

Orchid 0.8 removed the title-specific rendering path used by *The Legend of Zelda: Ocarina of
Time* (`0400486024DEC000`). Orchid 0.9 restored the general bindless-texture shader handling, but
the verified fog and exposure correction remained absent. This correction restores only those two
known shader edits on Vulkan.

It is enabled by default for this title. Set `EDEN_NX2_OOT_VISUAL_WORKAROUND=0` before starting
Orchid to disable it for an A/B comparison. Each matching shader logs the number of scalar edits.

## Guardrails

The correction requires the exact title ID, shader hash, shader stage and expected scalar shape.
If any of those differ, Orchid leaves the shader unchanged. No other title is affected.

| Shader | Hash | Correction |
|---|---:|---|
| Fog integration, compute | `780c0536b89fc89e` | Scale the integration slice distance by 0.25 before extinction and scattering accumulation. |
| Log-bloom final, fragment | `bded5ce917183214` | Scale exposure loads from constant buffer 3, offset `0x38`, by 0.65 before the filmic curve. |

## Limitations

- This is a compatibility correction for captured shaders, not a general solution for every game.
- Interiors use the same fog integration shader, so they also receive the reduced fog.
- A different game revision may use different shader hashes and will deliberately be left alone.
