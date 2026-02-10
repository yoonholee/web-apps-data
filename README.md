# web-apps-data

Static data files for [yoonholee.com/web-apps](https://www.yoonholee.com/web-apps/). Hosted here because the files are too large for the main site repo.

## How it works

1. Data lives in this repo, organized by app: `{app-name}/data.json` + `{app-name}/traj/`
2. Web apps fetch data at runtime from `raw.githubusercontent.com`:
   ```
   https://raw.githubusercontent.com/yoonholee/web-apps-data/main/{app-name}/{path}
   ```
3. `raw.githubusercontent.com` serves with `Access-Control-Allow-Origin: *` headers, so browser `fetch()` works cross-origin.

## Apps using this repo

| App | Metadata | Trajectories | Size |
|-----|----------|--------------|------|
| terminal-bench | `data.json` (4.1MB) | 90 task JSONs in `traj_all/` | 547MB |
| appworld-explorer | `data.json` (478KB) | 585 files in `traj/` | 87MB |
| gaia-explorer | `data.json` (176KB) | 1014 files in `traj/` | 10MB |

## Adding data for a new app

```bash
# From the main site repo
mkdir -p /tmp/web-apps-data/{app-name}
cp data/data.json /tmp/web-apps-data/{app-name}/
cp -r data/traj /tmp/web-apps-data/{app-name}/

cd /tmp/web-apps-data
git add {app-name}
git commit -m "Add {app-name} data"
git push
```

Then update the app's fetch URL:
```js
const DATA_BASE = "https://raw.githubusercontent.com/yoonholee/web-apps-data/main/{app-name}";
fetch(`${DATA_BASE}/data.json`)
```

## Limits

- GitHub: 100MB max per file, ~5GB recommended max repo size
- All trajectory files here are 2.4-15MB each (well under limit)
- Current total: ~645MB
