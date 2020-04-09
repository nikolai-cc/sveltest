# sveltest

Some first adventures wandering around in svelte.

[See live](https://sveltest.now.sh)

### WTF?
- Add n windows with the number input + go button.
- Bring window to front by clicking.
- Move windows areound by dragging anywhere except the bottom-right corner.
- Resize windows by dragging the coördinates in the bottom-right corner.
- Close windows by pressing the id in the top-right corner.
- Enable / disable drop-shadows and blur classes and see how that impacts performance.

Should perform reasonably well up to a couple 100 windows with mouse and touch devices, both new (with effects) and old (without effects).

Probably requires modern browser, haven't checked.

### Running the app

##### Dev

Install:
```bash
cd sveltest
npm install
```

Run:
```bash
npm run dev
```

#### Prod

Build:
```bash
npm run build
```

Run:
```bash
npm run start
```
