
<script>
  import { onMount } from 'svelte';
  let canvasEl;

  onMount(() => {
    const c = canvasEl;
    const ctx = c.getContext('2d');
    const wh = 128;
    const w2h = wh * wh;
    c.width = c.height = wh;
    const img = ctx.createImageData(wh, wh);
    const id = img.data;
    let t = 0;
    const inc = 1 / wh;
    const arr = Array.from({ length: w2h }, () => Math.random() * 1.5 - 0.5);

    function hue(val) {
      return 255 * Math.min(Math.max(val, 0), 1);
    }
    function ease(x) {
      return x > 0.2 ? 0 : cubicInterp(1, 0, x * 6);
    }
    function cubicInterp(a, b, t) {
      t = t * t * t * (6 * t * t - 15 * t + 10);
      return a + (b - a) * t;
    }
    function n(x, y) {
      const i = Math.abs(x * wh + y) % w2h;
      return arr[i];
    }
    function oct(x, y) {
      return perlin(x * 3, y * 4) + perlin(x * 4, y * 5) * 0.5;
    }
    function perlin(x, y) {
      const nx = Math.floor(x);
      const ny = Math.floor(y);
      return cubicInterp(
        cubicInterp(n(nx, ny), n(nx + 1, ny), x - nx),
        cubicInterp(n(nx, ny + 1), n(nx + 1, ny + 1), x - nx),
        y - ny
      );
    }

    function draw() {
      t += inc;
      for (let x = 1; x >= 0; x -= inc) {
        for (let y = 1; y >= 0; y -= inc) {
          const idx = (y * wh + x) * wh * 4;
          const dx = x;
          const dy = y;
          const dist = Math.sqrt(dx * dx + dy * dy);
          const ax = oct(x, y);
          const ay = oct(x + 2, y + t / 3);
          const bx = oct(x + dist * 0.3 + ax / 22 + 0.7, y + ay / 5 + 2);
          const by = oct(x + ax / 3 + 4 * t, y + ay / 3 + 5);
          const nVal = oct(x + bx / 5, y + by / 2) * 0.7 + 0.15;
          const d = ax * by / 3;
          const e = ay * bx / 2;

          id[idx + 0] = hue(nVal + d / 10); //red
          id[idx + 1] = hue(nVal / 2 + e / 5 + d / 10); //green
          id[idx + 2] = hue(d / 10 + e / 10); //blue
          id[idx + 3] = hue(1 - ease(dist) * (e + d) * 5); //alpha
        }
      }
      ctx.putImageData(img, 0, 0);
      requestAnimationFrame(draw);
    }

    draw();
  });
</script>

<canvas bind:this={canvasEl} style="position:absolute;top:0;left:0;width:100%;height:100%;z-index:-1;display:block;"></canvas>

