# Alacritty issue 35 / PR 54 installed-snap desktop evidence

Eight cases: baseline/patched x Fira Code/default monospace x Ubuntu/Kubuntu font rules.

- Ubuntu 24.04.5 GNOME/Wayland VM, 2 vCPU, 2 GiB, llvmpipe/Mesa 25.2.8.
- Baseline is Store revision 179, locally installed as x1. Store SHA3-384 and installed snap match.
- Patched is a normal managed-LXD build from signed commit e0c96807455bc3fa64e6080bdff0c734c628ee12, installed as x2. Build provider limited to one CPU/2 GiB and CARGO_BUILD_JOBS=1; no source/recipe edits.
- Patched snap SHA256: d0eb44e51b45b01785eabe75237d090712c9d86fe00eecb06abf2056872e2101.
- Bundled Fontconfig, FreeType, PNG binaries byte-identical across both artifacts; live maps confirm bundled libraries used.
- Fira Code 6.2-2 at size 12 with reporter's full Alacritty configuration, Noto Color Emoji 2.047, Noto core, DejaVu, Font Awesome, OpenSymbol and MesloLGS Nerd Font v3.5.1. Exact reporter font hashes were not supplied.
- No custom user Fontconfig configuration. Ubuntu rules are unchanged. Kubuntu profile adds only the unmodified distribution file 56-kubuntu-noto.conf extracted from kubuntu-settings-desktop 1:24.04.11; identical in current 1:24.04.13. Both downloaded debs checked against guest APT SHA256 metadata. This simulates KDE-associated font defaults, NOT a full KDE desktop or the reporter's exact host rules.
- Rule SHA256: 029d6d3fcf3c200537e2099d93fd924815752df7528827f3764709c63304271e.

## Results

With Kubuntu rules, baseline chooses DejaVu Sans for U+1F62D (monochrome); patched chooses Noto Color Emoji (colour), with either Fira Code or generic monospace. Fira Code cells remain 10x20. Default primary changes from DejaVu Sans Mono (9x18) to Noto Sans Mono (9x22), matching the direction in reporter logs. With Ubuntu's stock rules, crying stays monochrome in both packages.

This verifies restoration of host font preferences for this reproduction, not a universal emoji renderer fix, every sequence, or AMD/KDE graphics support.

Common unrelated diagnostics: Mesa's Vulkan/Zink attempt fails before successful llvmpipe OpenGL; both packages show an sctk_adwaita titlebar warning. First launch of local x2 also logged desktop-helper MIME-cache failure. Final repeated captures succeed and retain the common titlebar warning in untouched originals.

## Images

Comparison images are side-by-side crops of successive full desktop captures in the same VM, baseline left and patched right. Crops exclude unrelated desktop area and bottom warning bars. No glyphs or result labels were synthesized or retouched. Untouched 1280x800 captures follow.

- [untouched-desktop: 20261020-alacritty-issue35-baseline-fira-stock.png](./20261020-alacritty-issue35-baseline-fira-stock.png)
- [untouched-desktop: 20261020-alacritty-issue35-patched-fira-stock.png](./20261020-alacritty-issue35-patched-fira-stock.png)
- [comparison-crops: 20261020-alacritty-issue35-comparison-fira-stock.png](./20261020-alacritty-issue35-comparison-fira-stock.png)
- [untouched-desktop: 20261020-alacritty-issue35-baseline-fira-kubuntu.png](./20261020-alacritty-issue35-baseline-fira-kubuntu.png)
- [untouched-desktop: 20261020-alacritty-issue35-patched-fira-kubuntu.png](./20261020-alacritty-issue35-patched-fira-kubuntu.png)
- [comparison-crops: 20261020-alacritty-issue35-comparison-fira-kubuntu.png](./20261020-alacritty-issue35-comparison-fira-kubuntu.png)
- [untouched-desktop: 20261020-alacritty-issue35-baseline-default-stock.png](./20261020-alacritty-issue35-baseline-default-stock.png)
- [untouched-desktop: 20261020-alacritty-issue35-patched-default-stock.png](./20261020-alacritty-issue35-patched-default-stock.png)
- [comparison-crops: 20261020-alacritty-issue35-comparison-default-stock.png](./20261020-alacritty-issue35-comparison-default-stock.png)
- [untouched-desktop: 20261020-alacritty-issue35-baseline-default-kubuntu.png](./20261020-alacritty-issue35-baseline-default-kubuntu.png)
- [untouched-desktop: 20261020-alacritty-issue35-patched-default-kubuntu.png](./20261020-alacritty-issue35-patched-default-kubuntu.png)
- [comparison-crops: 20261020-alacritty-issue35-comparison-default-kubuntu.png](./20261020-alacritty-issue35-comparison-default-kubuntu.png)

## Reproduction evidence

[Logs, live maps, configurations, commands, fixture and provenance](./20261020-alacritty-issue35-evidence.zip)

## Image SHA256

```text
50aa4f49d49fe230b3287fcbdfb215b127a688a53f7a0b9379f38a539b40afda  20261020-alacritty-issue35-baseline-fira-stock.png
1f713cf21abb5565f9b05cd64a03b70be2fb871dc6cf1e72fa6804ef64e96d2a  20261020-alacritty-issue35-patched-fira-stock.png
bad7a2319d3c50b33084cd39509aaca32463c090581294e5b89fd49527a380bc  20261020-alacritty-issue35-comparison-fira-stock.png
d05778bf54db32614d874755116efa42008b01ca51425af4d50a6359e32e158c  20261020-alacritty-issue35-baseline-fira-kubuntu.png
4430b03801e8d9627aae75634d50dc941221b102d6e0857bd000414fc3928a92  20261020-alacritty-issue35-patched-fira-kubuntu.png
8619501fe43a7de2b3b581ad68d23384a3246264ae224d7979aef51ea9ff861f  20261020-alacritty-issue35-comparison-fira-kubuntu.png
8d596a5950ce55015a475ac8f2c668e2ec577246c9ff867b02298aaa445f4c35  20261020-alacritty-issue35-baseline-default-stock.png
e8db4f6baaa9c7ec7a6674141b698a153d1d30c393f7a8f8600e8cf28349ea54  20261020-alacritty-issue35-patched-default-stock.png
2b309c1d5c37015f05f7b4923796c57833e9fb424e852fcd132e2958cc82f787  20261020-alacritty-issue35-comparison-default-stock.png
33f5be469dd43644fc2c927b87de700ad67812c6ef7f859d87cf109b28549b53  20261020-alacritty-issue35-baseline-default-kubuntu.png
7687b16841682a37ef8863aa45021a0625c6976d7df0f0a5ddf12acc9ec50338  20261020-alacritty-issue35-patched-default-kubuntu.png
bdd44e356a79fc98b000169eb4323559e5a6462ce42375011b6cc234585b1b55  20261020-alacritty-issue35-comparison-default-kubuntu.png
```
