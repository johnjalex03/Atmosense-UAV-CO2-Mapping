# Remaining To-Dos

Delete this file once you're done. Everything below is marked in `README.md` as an
`<!-- HTML comment -->` — search for `<!--` to jump between them.

## 1. Fix before publishing

- [ ] **Your two diagrams disagree on the Foxglove Bridge port.** `ROS2_Nodes.png` says WebSocket port
      **8675**; `Telemetry.png` says **8765**. Foxglove Bridge's default is 8765, so the node-graph
      diagram likely has a digit transposition. Worth fixing — it's the kind of detail a sharp reviewer
      spots. The README deliberately cites no port number.
- [ ] **Confirm `flight.webp` animates on GitHub.** It is an animated WebP, which autoplays like a
      GIF but keeps far more detail per byte. If it renders as a still after you push, rebuild it as
      a GIF with the recipe in §4 and accept the size or resolution hit. Everything else animated in
      the README is a GIF and is not at risk.
- [ ] Delete `assets/demos/NTU_flight.jpg`, it is stale and unreferenced.
- [ ] **`Z2` vs `Z1` naming.** The enclosure source is `Z2_exploded_view.mov` while the PCB silkscreen
      reads `Z1 Sensor HAT`. The README treats Z2 as the enclosure revision and Z1 as the board. If
      that's wrong, fix the Hardware section — it's the kind of inconsistency that reads as sloppiness.

## 2. Facts still blank

- [ ] **Total payload mass** (Hardware table) — this is the number backing "mounts to any UAV with
      sufficient lift." Without it the claim is an assertion
- [ ] Battery / UPS capacity
- [ ] Advisor name
- [ ] Results table — 5 metrics
- [ ] **"What the data showed"** — 3 bullets. The highest-value section in the whole README. Suggested
      from your own NTU Day 2 plots: CO₂ spanning ~410–475 ppm inside one 10×5×3 m greenhouse, and σ
      ranging ~2–12 ppm. **Verify both against your data before publishing**
- [ ] Report / poster / slides — drop PDFs in `docs/` and fix the links
- [ ] Full demo video URL (see §4)

## 3. Optional additions

- [ ] **CFD velocity-field plot** — the Rotor Downwash section has the text but no figure. Your
      teammate's portfolio has the SolidWorks side-view plot. Save it to
      `assets/results/cfd-velocity-field.png` and uncomment the line in that section
- [ ] A photo of the payload **bracket-mounted to the Mavic 3** on the ground — the in-flight GIF shows
      it, but a clear static shot makes the "it's a payload" point instantly
- [ ] SLAM trajectory vs. ground-truth plot, if you produced one

## 4. Video hosting

The README embeds **GIFs** because that is the only format that autoplays inline on GitHub —
`<video>` tags are stripped by GitHub's HTML sanitizer, and a relative path to an `.mp4` renders
nothing. The four GIFs were generated from your source videos:

| GIF | From | Notes |
|:--|:--|:--|
| `dashboard.gif` | `dashboard.mov` | 14s from 0:01, 720px wide, 10fps |
| `gazebo_sim.gif` | `GAZEBO.mov` | 13s from 0:49, cropped to the RViz window, 1100px, 10fps |
| `flight.webp` | `greenhouse_flight.mp4` | 12s from 0:02, 540x960, 12fps, lossy q70 (see below) |
| `concentration.gif` | `07_concentration_3d.mp4` | full 60s sped 3× |
| `uncertainty.gif` | `07_uncertainty_3d.mp4` | full 60s sped 3× |
| `exploded.gif` | `Z2_exploded_view.mov` | cropped to subject, slowed 2.5×, 1.6s hold on the final frame |

The exploded-view source is 5120×2146 with the subject occupying only the middle ~45%, so it's cropped
to `2304:2146:1408:0` before scaling. The original is 1.04s, too fast to read, hence the slowdown and
the held final frame.

**The flight clip is WebP, not GIF, because handheld greenhouse footage is GIF's worst case.** At 480px
wide the GIF was 24 MB. The same clip as lossy animated WebP is 7 MB at higher resolution with no
banding. To rebuild it:

```bash
ffmpeg -ss 2 -t 12 -i assets/demos/greenhouse_flight.mp4 -vf "fps=12,scale=540:-1:flags=lanczos" -f image2 /tmp/wf/f%04d.png && img2webp -loop 0 -lossy -q 70 -m 4 -d 83 /tmp/wf/f*.png -o assets/demos/flight.webp
```

Screen recordings compress well as GIF because they are mostly flat colour, which is why the dashboard,
Gazebo, and field GIFs stay small. Camera footage does not.

To regenerate any of them with different trims:

```bash
ffmpeg -ss 2 -t 10 -i IN.mp4 -vf "fps=10,scale=300:-1:flags=lanczos,palettegen=stats_mode=diff:max_colors=128" -y /tmp/pal.png && ffmpeg -ss 2 -t 10 -i IN.mp4 -i /tmp/pal.png -lavfi "fps=10,scale=300:-1:flags=lanczos[x];[x][1:v]paletteuse=dither=bayer:bayer_scale=5:diff_mode=rectangle" -y OUT.gif
```

**The source videos are gitignored on purpose.** `dashboard.mov` (67MB) and `greenhouse_flight.mp4`
(53MB) both exceed GitHub's 50MB warning threshold, and committed binaries bloat every clone forever.
They stay on your disk; only the GIFs ship.

For a full-quality playable video, either:
- **Inline player:** open a new Issue in the repo, drag the `.mp4` into the comment box, wait for
  upload, copy the generated `https://github.com/user-attachments/...` URL, paste it on its own line
  in the README. Close the issue without submitting. GitHub renders a real player — but note it
  **won't autoplay**, which is why the GIFs stay.
- **YouTube/Drive link** from the `[Full Demo Video]` line in Team & Docs.

## 5. Repo settings on GitHub

- [ ] **About** section (top right): short description + demo video link + topics:
      `ros2` `slam` `uav` `gaussian-processes` `deep-kernel-learning` `precision-agriculture`
      `robotics` `orb-slam3` `pytorch` `kicad`
- [ ] Pin the repo to your GitHub profile
- [ ] Add a LICENSE if you want it reusable — confirm with your advisor first, since capstone IP terms
      vary by program
- [ ] If you later add code, check no credentials or facility data land in the history

## 6. Sanity check

- [ ] View the README on a **phone** — the `<table>` galleries are what break first
- [ ] Confirm all 11 images render (broken images look worse than no images)
- [ ] Read only the first screen. Does someone stopping there know what this is and that it worked?
