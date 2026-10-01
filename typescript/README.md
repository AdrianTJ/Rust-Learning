# TypeScript track: workspace

Your code goes in `harness/` here. Steps are in [`../CHECKLIST.md`](../CHECKLIST.md),
section **T**.

## Setup

```bash
mkdir -p harness && cd harness   # from typescript/
npm init -y
npm pkg set type=module
npm install -D typescript @types/node tsx
npm install zod @anthropic-ai/sdk dotenv
```

`tsconfig.json` — the version from the source curriculum, plus the two flags it
was missing:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "types": ["node"],
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist"
  },
  "include": ["src/**/*"]
}
```

Run with `npx tsx src/index.ts`. No build step.

> **First-hour wall:** with `module: NodeNext` and `"type": "module"`, relative
> imports need a `.js` extension even though the file is `.ts` —
> `import { runLoop } from "./loop.js"`. It looks wrong. It is correct. This
> trips up everyone once.
