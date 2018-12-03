# Day 3

```js
claims = document.body.textContent.trim().split("\n")
claimed = new Set;
dupes = new Set
for(const claim of claims) {
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
dupes.size
```
