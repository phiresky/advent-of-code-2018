Paste these solutions directly into the dev console (ctrl+shift+i) at the input website (e.g. https://adventofcode.com/2018/day/3/input )

# Day 1

```js
eval(document.body.textContent)
```
## Part 2

```js
set = new Set();
document.body.textContent
  .repeat(1000)
  .trim()
  .split("\n")
  .map(x => +x)
  .reduce((sum, b) => {
    if (set.has(sum)) throw sum;
    set.add(sum);
    return sum + b;
  });
```


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

# Day 4

```js
sleepysum = {}
sleepytime = {}
guard = 0
for(p of document.body.textContent.trim().split("\n").sort()) { 
  [_, minute, tag, gid ] = p.match(/:(..)\] (.)(?:uard #(\d+))?/)
  if (gid) guard = gid;
  if (tag == 'f') start = +minute
  if (tag == 'w') {
    end = +minute
    sleepysum[guard] = (sleepysum[guard]||0) + (end - start)
    tgt = sleepytime[guard] || (sleepytime[guard] = [])
    for(let i = start; i < end; i++) tgt[i] = (tgt[i]||0) + 1
  }
}
[sleepyid] = Object.entries(sleepysum).sort((a,b) => b[1] - a[1])[0]
min = sleepytime[sleepyid].indexOf(Math.max(...sleepytime[sleepyid].filter(p=>p)))
console.log(sleepyid * min)

// part 2
[mostsle, max, min] = Object.entries(sleepytime).map(([gid, mins]) => {x = Math.max(...mins.filter(p=>p)); return [gid, x, mins.indexOf(x)]}).sort((a,b) => b[1] - a[1])[0]
console.log(mostsle, min)
```
