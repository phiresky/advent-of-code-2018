# Day 3

## Part 1

```js
claims = document.body.textContent.trim().split("\n");
claimed = new Set();
dupes = new Set();
for (const claim of claims) {
  const coords = claim.match(/(?<x>\d+),(?<y>\d+): (?<w>\d+)x(?<h>\d+)/).groups;
  for (const k in coords) coords[k] = +coords[k];
  for (let x = coords.x; x < coords.x + coords.w; x++) {
    for (let y = coords.y; y < coords.y + coords.h; y++) {
      const inx = x * 1e6 + y;
      if (claimed.has(inx)) dupes.add(inx);
      else claimed.add(inx);
    }
  }
}
dupes.size;
```

## Part 2 + Part 1

```js
claims = document.body.textContent.trim().split("\n");
claimed = new Map();
dupes = new Set();
candidates = [];
for (const claim of claims) {
  const coords = claim.match(
    /#(?<id>\d+) @ (?<x>\d+),(?<y>\d+): (?<w>\d+)x(?<h>\d+)/
  ).groups;
  for (const k in coords) coords[k] = +coords[k];
  let broken = false;
  for (let x = coords.x; x < coords.x + coords.w; x++) {
    for (let y = coords.y; y < coords.y + coords.h; y++) {
      const inx = x * 1e6 + y;
      if (claimed.has(inx)) {
        dupes.add(inx);
        broken = true;
      }
      claimed.set(inx, coords.id);
    }
  }
  if (!broken) candidates.push({ id: coords.id, count: coords.w * coords.h });
}
console.log(dupes.size);
for (const { id, count } of candidates) {
  let hasc = 0;
  for (const oid of claimed.values()) if (oid === id) hasc++;
  if (hasc === count) throw id;
}
```
