# LinkAI SME Connect — Starter Files

Paste each file at the exact path shown. Follow the "Install commands" section at the bottom to wire everything up.

---

## Table of Contents

1. [README.md](#1-readmemd)
2. [.env.local.example](#2-envlocalexample)
3. [.gitignore](#3-gitignore)
4. [components/branding/Logo.tsx](#4-componentsbrandinglogotsx)
5. [lib/supabase/client.ts](#5-libsupabaseclientts)
6. [lib/supabase/server.ts](#6-libsupabaseserverts)
7. [lib/supabase/middleware.ts](#7-libsupabasemiddlewarets)
8. [middleware.ts (root)](#8-middlewarets-root)
9. [ai/embeddings/bge.ts](#9-aiembeddingsbg ets)
10. [ai/matching/score.ts](#10-aimatchingscorets)
11. [database/schema.sql](#11-databaseschemasql)
12. [types/index.ts](#12-typesindexts)
13. [app/api/match/route.ts](#13-appapimatchroutets)
14. [app/api/embed/route.ts](#14-appapiembedroutets)
15. [scripts/embed-seed.ts](#15-scriptsem bed-seedts)
16. [Install commands](#16-install-commands)

---

## 1. README.md

**Path:** `README.md`

```markdown
# LinkAI SME Connect

> AI-powered SME-to-Corporate Opportunity Matching Platform connecting South African youth-owned SMEs with procurement opportunities, funding programmes, investors, and business support services.

Built for the **YES AI Hackathon 2026** by **LinkAI Innovators**.

![Status](https://img.shields.io/badge/status-MVP%20in%20progress-yellow)
![Next.js](https://img.shields.io/badge/Next.js-15-black)
![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-green)
![Hugging Face](https://img.shields.io/badge/Hugging%20Face-BGE%20Embeddings-orange)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## The Problem

South African SMEs — especially youth-owned — struggle to discover:

- Procurement opportunities
- Supplier development programmes
- Grants & funding
- Investors
- Business support services

Corporates, funders and investors, in turn, struggle to discover suitable SMEs.

## The Solution

LinkAI SME Connect uses **explainable AI** to match SMEs to opportunities semantically (not just by keyword), and gives corporates a ranked, trust-scored pipeline of vetted SMEs.

## Core Features

- 🎯 **Semantic Matching** — BGE embeddings + cosine similarity + pgvector
- 🏢 **SME & Corporate Dashboards**
- 📊 **AI Readiness Score** — shows SMEs what to improve
- 🔍 **Explainable AI** — every match shows *why* it scored high
- 📈 **Analytics** — pipeline health, sector coverage, match quality
- 🔒 **Supabase Auth** — email/password + magic link

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | Next.js 15, React, TypeScript, Tailwind CSS, shadcn/ui, Framer Motion, Lucide |
| Backend | Next.js App Router, Server Actions, API Routes |
| Auth | Supabase Auth |
| Database | Supabase PostgreSQL + pgvector |
| Storage | Supabase Storage |
| AI | Hugging Face Inference API |

## Hugging Face Models

| Model | Purpose |
|---|---|
| `BAAI/bge-small-en-v1.5` | Semantic SME ↔ opportunity matching (**start here**) |
| `facebook/bart-large-mnli` | Sector classification |
| `facebook/bart-large-cnn` | SME profile summaries |
| `microsoft/Phi-3-mini-4k-instruct` | AI recommendations & readiness advice |

## Getting Started

```bash
git clone https://github.com/YOUR_USERNAME/linkai_sme_connect.git
cd linkai_sme_connect
npm install
cp .env.local.example .env.local   # fill in keys
npm run dev
```

## Environment Variables

See `.env.local.example`. You need:

- Supabase URL + anon key + service role key
- Hugging Face fine-grained token (`HUGGINGFACE_API_KEY`)

## Project Structure

```
app/          Next.js routes (marketing, auth, dashboard, api)
components/   UI + branding + shared
features/     Feature-scoped logic (auth, matching, readiness…)
ai/           Embeddings, matching, classification, summaries, recommendations
database/     SQL schema + seed
supabase/     Migrations
lib/          Supabase clients, utils
types/        Shared TS types
```

## Roadmap

- [x] Repo foundation & structure
- [ ] Landing page
- [ ] Supabase Auth
- [ ] SME profile builder
- [ ] Opportunity creation
- [ ] BGE embedding matching engine
- [ ] Readiness score
- [ ] Explainable match UI
- [ ] Analytics dashboard
- [ ] Vercel deploy

## License

MIT © LinkAI Innovators
```

---

## 2. .env.local.example

**Path:** `.env.local.example`

```env
# --- Supabase ---
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# --- Hugging Face ---
# Fine-grained token with "Make calls to Inference Providers" scope
HUGGINGFACE_API_KEY=hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxx

# --- App ---
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## 3. .gitignore

**Path:** `.gitignore`

```gitignore
# dependencies
/node_modules
/.pnp
.pnp.*

# next.js
/.next/
/out/

# production
/build

# env
.env
.env.local
.env.*.local
!.env.local.example

# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# vercel
.vercel

# typescript
*.tsbuildinfo
next-env.d.ts

# os
.DS_Store
Thumbs.db
```

---

## 4. components/branding/Logo.tsx

**Path:** `components/branding/Logo.tsx`

```tsx
'use client';

import Image from 'next/image';
import { useState } from 'react';

type LogoProps = {
  size?: number;
  showWordmark?: boolean;
  className?: string;
};

export function Logo({ size = 36, showWordmark = true, className = '' }: LogoProps) {
  const [imgError, setImgError] = useState(false);

  return (
    <div className={`flex items-center gap-2 ${className}`}>
      {!imgError ? (
        <Image
          src="/branding/linkai_sme_solution.svg"
          alt="LinkAI SME Connect"
          width={size}
          height={size}
          onError={() => setImgError(true)}
          priority
        />
      ) : (
        <div
          aria-hidden
          style={{ width: size, height: size }}
          className="rounded-lg bg-gradient-to-br from-emerald-500 to-blue-600 flex items-center justify-center text-white font-bold"
        >
          LA
        </div>
      )}

      {showWordmark && (
        <span className="font-semibold tracking-tight text-foreground">
          LinkAI <span className="text-emerald-600">SME</span>
        </span>
      )}
    </div>
  );
}
```

> **Action:** drop your final SVG at `public/branding/linkai_sme_solution.svg`. The component picks it up automatically.

---

## 5. lib/supabase/client.ts

**Path:** `lib/supabase/client.ts`

```ts
import { createBrowserClient } from '@supabase/ssr';

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
  );
}
```

---

## 6. lib/supabase/server.ts

**Path:** `lib/supabase/server.ts`

```ts
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';

export async function createClient() {
  const cookieStore = await cookies();

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll();
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options),
            );
          } catch {
            // setAll called from a Server Component — safe to ignore
          }
        },
      },
    },
  );
}
```

---

## 7. lib/supabase/middleware.ts

**Path:** `lib/supabase/middleware.ts`

```ts
import { createServerClient } from '@supabase/ssr';
import { NextResponse, type NextRequest } from 'next/server';

export async function updateSession(request: NextRequest) {
  let response = NextResponse.next({ request });

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return request.cookies.getAll();
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value }) =>
            request.cookies.set(name, value),
          );
          response = NextResponse.next({ request });
          cookiesToSet.forEach(({ name, value, options }) =>
            response.cookies.set(name, value, options),
          );
        },
      },
    },
  );

  const {
    data: { user },
  } = await supabase.auth.getUser();

  // Protect dashboard routes
  if (!user && request.nextUrl.pathname.startsWith('/dashboard')) {
    const url = request.nextUrl.clone();
    url.pathname = '/login';
    return NextResponse.redirect(url);
  }

  return response;
}
```

---

## 8. middleware.ts (root)

**Path:** `middleware.ts`

```ts
import { type NextRequest } from 'next/server';
import { updateSession } from '@/lib/supabase/middleware';

export async function middleware(request: NextRequest) {
  return await updateSession(request);
}

export const config = {
  matcher: [
    '/((?!_next/static|_next/image|favicon.ico|branding|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',
  ],
};
```

---

## 9. ai/embeddings/bge.ts

**Path:** `ai/embeddings/bge.ts`

```ts
/**
 * BAAI/bge-small-en-v1.5 embedding client via Hugging Face Inference API.
 * BGE recommends prefixing queries with "Represent this sentence for searching relevant passages: ".
 */

const MODEL_ID = 'BAAI/bge-small-en-v1.5';
const HF_URL = `https://api-inference.huggingface.co/models/${MODEL_ID}`;

const QUERY_PREFIX =
  'Represent this sentence for searching relevant passages: ';

export type Embedding = number[];

async function callHF(inputs: string | string[]): Promise<number[][]> {
  const res = await fetch(HF_URL, {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${process.env.HUGGINGFACE_API_KEY}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ inputs, options: { wait_for_model: true } }),
  });

  if (!res.ok) {
    const text = await res.text();
    throw new Error(`HF embedding failed (${res.status}): ${text}`);
  }

  const data = (await res.json()) as number[] | number[][];
  return Array.isArray(data[0]) ? (data as number[][]) : [data as number[]];
}

export async function embedText(text: string): Promise<Embedding> {
  const [vec] = await callHF(text);
  return vec;
}

export async function embedQuery(query: string): Promise<Embedding> {
  const [vec] = await callHF(QUERY_PREFIX + query);
  return vec;
}

export async function embedBatch(texts: string[]): Promise<Embedding[]> {
  const CHUNK = 32;
  const out: Embedding[] = [];
  for (let i = 0; i < texts.length; i += CHUNK) {
    const chunk = texts.slice(i, i + CHUNK);
    const vecs = await callHF(chunk);
    out.push(...vecs);
  }
  return out;
}

export function cosineSimilarity(a: Embedding, b: Embedding): number {
  if (a.length !== b.length) throw new Error('Vector length mismatch');
  let dot = 0;
  let na = 0;
  let nb = 0;
  for (let i = 0; i < a.length; i++) {
    dot += a[i] * b[i];
    na += a[i] * a[i];
    nb += b[i] * b[i];
  }
  return dot / (Math.sqrt(na) * Math.sqrt(nb) || 1);
}

/** Build a single searchable string from an SME profile. */
export function smeToText(sme: {
  name: string;
  sector: string;
  description: string;
  services: string[];
  provinces: string[];
  bbbeeLevel?: number | null;
}): string {
  return [
    `SME: ${sme.name}`,
    `Sector: ${sme.sector}`,
    `Description: ${sme.description}`,
    `Services: ${sme.services.join(', ')}`,
    `Provinces: ${sme.provinces.join(', ')}`,
    sme.bbbeeLevel ? `B-BBEE Level: ${sme.bbbeeLevel}` : '',
  ]
    .filter(Boolean)
    .join('\n');
}

/** Build a single searchable string from an opportunity. */
export function opportunityToText(op: {
  title: string;
  sector: string;
  description: string;
  requirements: string[];
  province?: string | null;
  valueZar?: number | null;
}): string {
  return [
    `Opportunity: ${op.title}`,
    `Sector: ${op.sector}`,
    `Description: ${op.description}`,
    `Requirements: ${op.requirements.join(', ')}`,
    op.province ? `Province: ${op.province}` : '',
    op.valueZar ? `Value: R${op.valueZar.toLocaleString()}` : '',
  ]
    .filter(Boolean)
    .join('\n');
}
```

---

## 10. ai/matching/score.ts

**Path:** `ai/matching/score.ts`

```ts
import { cosineSimilarity, type Embedding } from '@/ai/embeddings/bge';

export type MatchBreakdown = {
  semantic: number;   // 0..1
  sector: number;     // 0..1
  province: number;   // 0..1
  bbbee: number;      // 0..1
  total: number;      // 0..1
  reasons: string[];
};

type SMEInput = {
  sector: string;
  provinces: string[];
  bbbeeLevel?: number | null;
};

type OppInput = {
  sector: string;
  province?: string | null;
  minBbbeeLevel?: number | null;
};

export function scoreMatch(
  smeEmbedding: Embedding,
  oppEmbedding: Embedding,
  sme: SMEInput,
  opp: OppInput,
): MatchBreakdown {
  const reasons: string[] = [];

  // 1. Semantic similarity (weight 60%)
  const semantic = Math.max(0, cosineSimilarity(smeEmbedding, oppEmbedding));
  if (semantic > 0.7)
    reasons.push('Profile strongly matches the opportunity description.');
  else if (semantic > 0.5)
    reasons.push('Profile is relevant to the opportunity description.');

  // 2. Sector match (weight 20%)
  const sector =
    sme.sector.toLowerCase() === opp.sector.toLowerCase() ? 1 : 0;
  if (sector) reasons.push(`Sector match: ${sme.sector}.`);

  // 3. Province match (weight 10%)
  const province =
    opp.province &&
    sme.provinces
      .map((p) => p.toLowerCase())
      .includes(opp.province.toLowerCase())
      ? 1
      : opp.province
        ? 0
        : 0.5; // no province requirement = neutral
  if (province === 1)
    reasons.push(`Operating in the required province: ${opp.province}.`);

  // 4. B-BBEE (weight 10%)
  let bbbee = 0.5;
  if (opp.minBbbeeLevel && sme.bbbeeLevel) {
    bbbee = sme.bbbeeLevel <= opp.minBbbeeLevel ? 1 : 0;
    if (bbbee === 1)
      reasons.push(`Meets B-BBEE requirement (Level ${sme.bbbeeLevel}).`);
    else
      reasons.push(
        `Does not meet B-BBEE requirement (needs Level ${opp.minBbbeeLevel}).`,
      );
  }

  const total =
    semantic * 0.6 + sector * 0.2 + province * 0.1 + bbbee * 0.1;

  if (reasons.length === 0)
    reasons.push(
      'Low semantic overlap — consider enriching your profile.',
    );

  return { semantic, sector, province, bbbee, total, reasons };
}
```

---

## 11. database/schema.sql

**Path:** `database/schema.sql`

```sql
-- Enable pgvector for semantic matching
create extension if not exists vector;

-- ---------- Profiles ----------
create table if not exists profiles (
  id uuid primary key references auth.users on delete cascade,
  role text check (role in ('sme','corporate','admin')) not null default 'sme',
  full_name text,
  email text,
  created_at timestamptz default now()
);

-- ---------- SMEs ----------
create table if not exists smes (
  id uuid primary key default gen_random_uuid(),
  owner_id uuid references profiles(id) on delete cascade not null,
  name text not null,
  sector text not null,
  description text not null,
  services text[] default '{}',
  provinces text[] default '{}',
  bbbee_level int,
  website text,
  logo_url text,
  embedding vector(384),          -- BGE small = 384 dims
  readiness_score int default 0,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

-- ---------- Opportunities ----------
create table if not exists opportunities (
  id uuid primary key default gen_random_uuid(),
  corporate_id uuid references profiles(id) on delete cascade not null,
  title text not null,
  sector text not null,
  description text not null,
  requirements text[] default '{}',
  province text,
  value_zar numeric,
  min_bbbee_level int,
  closes_at timestamptz,
  embedding vector(384),
  created_at timestamptz default now()
);

-- ---------- Matches ----------
create table if not exists matches (
  id uuid primary key default gen_random_uuid(),
  sme_id uuid references smes(id) on delete cascade not null,
  opportunity_id uuid references opportunities(id) on delete cascade not null,
  score numeric not null,
  reasons jsonb default '[]',
  created_at timestamptz default now(),
  unique (sme_id, opportunity_id)
);

-- ---------- Vector index (cosine) ----------
create index if not exists smes_embedding_idx
  on smes using ivfflat (embedding vector_cosine_ops) with (lists = 100);
create index if not exists opps_embedding_idx
  on opportunities using ivfflat (embedding vector_cosine_ops) with (lists = 100);

-- ---------- RLS ----------
alter table profiles enable row level security;
alter table smes enable row level security;
alter table opportunities enable row level security;
alter table matches enable row level security;

-- profiles: user can read/update own row
create policy "profiles self read" on profiles
  for select using (auth.uid() = id);
create policy "profiles self update" on profiles
  for update using (auth.uid() = id);

-- smes: owner full access; authenticated can read
create policy "smes owner write" on smes
  for all using (auth.uid() = owner_id) with check (auth.uid() = owner_id);
create policy "smes auth read" on smes
  for select using (auth.role() = 'authenticated');

-- opportunities: corporate owner write; authenticated read
create policy "opps owner write" on opportunities
  for all using (auth.uid() = corporate_id)
  with check (auth.uid() = corporate_id);
create policy "opps auth read" on opportunities
  for select using (auth.role() = 'authenticated');

-- matches: read if you own the sme or opportunity
create policy "matches read own" on matches
  for select using (
    exists (
      select 1 from smes s
      where s.id = matches.sme_id and s.owner_id = auth.uid()
    )
    or exists (
      select 1 from opportunities o
      where o.id = matches.opportunity_id and o.corporate_id = auth.uid()
    )
  );
create policy "matches insert system" on matches
  for insert with check (auth.role() = 'authenticated');
```

> **Run this** in Supabase → SQL Editor. Then push your migrations into `supabase/migrations/` too.

---

## 12. types/index.ts

**Path:** `types/index.ts`

```ts
export type Role = 'sme' | 'corporate' | 'admin';

export interface SME {
  id: string;
  owner_id: string;
  name: string;
  sector: string;
  description: string;
  services: string[];
  provinces: string[];
  bbbee_level?: number | null;
  website?: string | null;
  logo_url?: string | null;
  readiness_score: number;
  created_at: string;
}

export interface Opportunity {
  id: string;
  corporate_id: string;
  title: string;
  sector: string;
  description: string;
  requirements: string[];
  province?: string | null;
  value_zar?: number | null;
  min_bbbee_level?: number | null;
  closes_at?: string | null;
  created_at: string;
}

export interface Match {
  id: string;
  sme_id: string;
  opportunity_id: string;
  score: number;
  reasons: string[];
  created_at: string;
}
```

---

## 13. app/api/match/route.ts

**Path:** `app/api/match/route.ts`

```ts
import { NextResponse } from 'next/server';
import { createClient } from '@/lib/supabase/server';
import { scoreMatch } from '@/ai/matching/score';

/**
 * POST /api/match
 * Body: { opportunityId?: string, smeId?: string }
 * Matches one side against the other. Stores results in `matches`.
 */
export async function POST(req: Request) {
  const { opportunityId, smeId } = await req.json();

  if (!opportunityId && !smeId) {
    return NextResponse.json(
      { error: 'opportunityId or smeId required' },
      { status: 400 },
    );
  }

  const supabase = await createClient();

  if (opportunityId) {
    // Match an opportunity against all SMEs
    const { data: opp } = await supabase
      .from('opportunities')
      .select('*')
      .eq('id', opportunityId)
      .single();
    if (!opp)
      return NextResponse.json(
        { error: 'Opportunity not found' },
        { status: 404 },
      );

    const { data: smes } = await supabase
      .from('smes')
      .select('*')
      .not('embedding', 'is', null);
    if (!smes) return NextResponse.json({ matches: [] });

    const results = smes.map((sme) => {
      const b = scoreMatch(
        sme.embedding,
        opp.embedding,
        {
          sector: sme.sector,
          provinces: sme.provinces ?? [],
          bbbeeLevel: sme.bbbee_level,
        },
        {
          sector: opp.sector,
          province: opp.province,
          minBbbeeLevel: opp.min_bbbee_level,
        },
      );
      return {
        sme_id: sme.id,
        opportunity_id: opp.id,
        score: b.total,
        reasons: b.reasons,
      };
    });

    const top = results.sort((a, b) => b.score - a.score).slice(0, 20);

    await supabase
      .from('matches')
      .upsert(top, { onConflict: 'sme_id,opportunity_id' });

    return NextResponse.json({ matches: top });
  }

  // match one SME against all opportunities
  const { data: sme } = await supabase
    .from('smes')
    .select('*')
    .eq('id', smeId)
    .single();
  if (!sme)
    return NextResponse.json({ error: 'SME not found' }, { status: 404 });

  const { data: opps } = await supabase
    .from('opportunities')
    .select('*')
    .not('embedding', 'is', null);
  if (!opps) return NextResponse.json({ matches: [] });

  const results = opps.map((opp) => {
    const b = scoreMatch(
      sme.embedding,
      opp.embedding,
      {
        sector: sme.sector,
        provinces: sme.provinces ?? [],
        bbbeeLevel: sme.bbbee_level,
      },
      {
        sector: opp.sector,
        province: opp.province,
        minBbbeeLevel: opp.min_bbbee_level,
      },
    );
    return {
      sme_id: sme.id,
      opportunity_id: opp.id,
      score: b.total,
      reasons: b.reasons,
    };
  });

  const top = results.sort((a, b) => b.score - a.score).slice(0, 20);

  await supabase
    .from('matches')
    .upsert(top, { onConflict: 'sme_id,opportunity_id' });

  return NextResponse.json({ matches: top });
}
```

---

## 14. app/api/embed/route.ts

**Path:** `app/api/embed/route.ts`

```ts
import { NextResponse } from 'next/server';
import { createClient } from '@/lib/supabase/server';
import {
  embedText,
  smeToText,
  opportunityToText,
} from '@/ai/embeddings/bge';

/**
 * POST /api/embed
 * Body: { type: 'sme' | 'opportunity', id: string }
 * Generates and stores the vector for a record.
 */
export async function POST(req: Request) {
  const { type, id } = await req.json();
  const supabase = await createClient();

  if (type === 'sme') {
    const { data: sme } = await supabase
      .from('smes')
      .select('*')
      .eq('id', id)
      .single();
    if (!sme)
      return NextResponse.json({ error: 'SME not found' }, { status: 404 });

    const vec = await embedText(smeToText(sme));
    await supabase.from('smes').update({ embedding: vec }).eq('id', id);
    return NextResponse.json({ ok: true, dims: vec.length });
  }

  if (type === 'opportunity') {
    const { data: opp } = await supabase
      .from('opportunities')
      .select('*')
      .eq('id', id)
      .single();
    if (!opp)
      return NextResponse.json(
        { error: 'Opportunity not found' },
        { status: 404 },
      );

    const vec = await embedText(opportunityToText(opp));
    await supabase
      .from('opportunities')
      .update({ embedding: vec })
      .eq('id', id);
    return NextResponse.json({ ok: true, dims: vec.length });
  }

  return NextResponse.json({ error: 'unknown type' }, { status: 400 });
}
```

---

## 15. scripts/embed-seed.ts

**Path:** `scripts/embed-seed.ts`

```ts
/**
 * Bulk-embed existing SMEs and opportunities.
 * Run: npx tsx scripts/embed-seed.ts
 */
import { createClient } from '@supabase/supabase-js';
import {
  embedBatch,
  smeToText,
  opportunityToText,
} from '../ai/embeddings/bge';

const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!,
);

async function run() {
  const { data: smes } = await supabase
    .from('smes')
    .select('*')
    .is('embedding', null);
  if (smes?.length) {
    const vecs = await embedBatch(smes.map(smeToText));
    for (let i = 0; i < smes.length; i++) {
      await supabase
        .from('smes')
        .update({ embedding: vecs[i] })
        .eq('id', smes[i].id);
      console.log('SME embedded:', smes[i].id);
    }
  }

  const { data: opps } = await supabase
    .from('opportunities')
    .select('*')
    .is('embedding', null);
  if (opps?.length) {
    const vecs = await embedBatch(opps.map(opportunityToText));
    for (let i = 0; i < opps.length; i++) {
      await supabase
        .from('opportunities')
        .update({ embedding: vecs[i] })
        .eq('id', opps[i].id);
      console.log('Opp embedded:', opps[i].id);
    }
  }

  console.log('Done.');
}

run().catch(console.error);
```

---

## 16. Install commands

Run these once, in this order:

```bash
npx create-next-app@latest linkai_sme_connect \
  --typescript --tailwind --app --eslint \
  --src-dir=false --import-alias "@/*"

cd linkai_sme_connect

npx shadcn@latest init -d
npx shadcn@latest add button card input label textarea select badge table

npm i @supabase/supabase-js @supabase/ssr zustand framer-motion lucide-react
npm i -D tsx
```

Then paste files **1–15** at their paths, fill in `.env.local`, and push:

```bash
git add .
git commit -m "chore: scaffold LinkAI SME Connect MVP foundation"
git push
```

---

## What's working end-to-end after this

- ✅ Repo structure that matches the blueprint
- ✅ Logo loader with graceful fallback until SVG arrives
- ✅ Supabase browser / server / middleware clients (App Router ready)
- ✅ BGE embedding client with batch support + cosine similarity
- ✅ Explainable scoring (semantic + sector + province + B-BBEE, weighted, with reasons)
- ✅ `/api/embed` to vectorise any SME/opportunity
- ✅ `/api/match` to run explainable matches in both directions
- ✅ Postgres schema with pgvector indexes + RLS
- ✅ Seed script to bulk-embed existing rows

---

## Next files I can generate when you're ready

1. **Landing page** — `app/(marketing)/page.tsx` — hero, problem/solution, CTA
2. **Auth pages** — `app/(auth)/login`, `/register` + server actions
3. **SME profile builder** — multi-step form + auto-embed on save
4. **SME dashboard** — match cards with explainability panel
5. **Corporate dashboard** — post opportunity, ranked SME pipeline
6. **Readiness score** module
7. **Analytics dashboard** with charts

Reply **"continue with the landing page"** (or pick any of the above) and I'll generate the next batch.
