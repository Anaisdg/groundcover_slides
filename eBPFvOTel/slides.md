---
theme: default
title: 'eBPF + OTel: Two Layers, One Observability Strategy'
info: |
  Live workshop — September 2, 2026.
  David Flanagan (CoreWeave / Rawkode Academy) + Anais Dotis-Georgiou (groundcover)
class: text-left
transition: none
mdc: true
aspectRatio: 16/9
canvasWidth: 980
fonts:
  sans: Inter
  serif: Space Grotesk
  weights: '300,400,500,600,700,800'
  provider: google
layout: gc-hero
headline: 'eBPF + OTel:<br/>Two layers, one<br/>observability strategy'
sub: '<span class="gc-hl-g">Neither layer replaces the other.</span><br/>eBPF gives you universal coverage with no code changes.<br/>OpenTelemetry gives you the application context eBPF can''t see.<br/>The engineering work is deciding where each one earns its overhead.'
logos:
  - LIVE WORKSHOP
  - SEPTEMBER 2, 2026
  - 9 AM PT / 12 PM ET
art: /hero.png
---

<!--
Opening. The framing line: past a certain size, "complete instrumentation" is not
a goal you're behind on — it's one you'll never reach, and outages happen in
exactly those gaps.
-->

---
layout: gc
eyebrow: Speakers
title: Who's running this
---

<div class="spk">
  <div class="spk__card">
    <img src="/david.jpg" alt="David Flanagan" />
    <div>
      <h3>David Flanagan</h3>
      <div class="spk__role">Senior Solutions Engineer, CoreWeave · founder, Rawkode Academy · <span class="spk__handle">@rawkode</span></div>
      <p>Teaches engineers the practical work behind Kubernetes, platform engineering, and cloud-native infrastructure.</p>
    </div>
  </div>
  <div class="spk__card">
    <img src="/anais.jpg" alt="Anais Dotis-Georgiou" />
    <div>
      <h3>Anais Dotis-Georgiou</h3>
      <div class="spk__role">Developer Advocate, groundcover</div>
      <p>Works on observability, AI, and statistics — distilling complex topics into content developer communities can actually use.</p>
    </div>
  </div>
</div>

<style>
.spk { display: grid; grid-template-columns: 1fr 1fr; gap: 26px; align-items: center;
       height: 100%; padding-bottom: 6%; }
.spk__card { display: grid; grid-template-columns: 108px 1fr; gap: 18px; align-items: start;
             background: #fff; border-radius: var(--gc-radius); padding: 18px 20px; }
.spk__card img { width: 108px !important; height: 108px !important; border-radius: 50%;
                 object-fit: cover; display: block; }
.spk__card h3 { font-size: 17px; font-weight: 700; letter-spacing: -.01em; margin: 2px 0 6px; }
.spk__role { font-size: 10px; font-weight: 600; color: var(--gc-green); line-height: 1.4; }
.spk__handle { color: var(--gc-muted); font-weight: 500; }
.spk__card p { font-size: 11px; line-height: 1.5; color: var(--gc-slate); margin: 9px 0 0; }
</style>

---
layout: gc
eyebrow: Agenda
title: Build it, compare it, prove it
sub: A working cluster first, the concepts second, and three real failures to settle the argument.
---

<Steps :items="[
  { title: 'Deploy',  detail: 'Run ShopIQ on Kubernetes with Helm, then install groundcover and watch the metrics land.' },
  { title: 'Compare', detail: 'What eBPF sees, what OTel sees, and where each one earns its overhead.' },
  { title: 'Prove',   detail: 'Three failures where eBPF fidelity is what makes the trace answerable.' },
]" />

<div class="mt-6 grid grid-cols-3 gap-4">
  <div class="rounded-xl bg-white p-4 text-[11px] leading-relaxed">
    <div class="gc-eyebrow">Example 01</div>
    <b class="block mt-1">The slow SQL query</b>
    <div class="mt-1" style="color:var(--gc-slate)">The bound parameter behind the tail latency — and a skill that stops the agent guessing.</div>
  </div>
  <div class="rounded-xl bg-white p-4 text-[11px] leading-relaxed">
    <div class="gc-eyebrow">Example 02</div>
    <b class="block mt-1">The table that isn't there</b>
    <div class="mt-1" style="color:var(--gc-slate)">An L7 failure instrumentation never reported, captured statement and all.</div>
  </div>
  <div class="rounded-xl bg-white p-4 text-[11px] leading-relaxed">
    <div class="gc-eyebrow">Example 03</div>
    <b class="block mt-1">The noisy neighbor</b>
    <div class="mt-1" style="color:var(--gc-slate)">Ruling out node contention before you blame the query.</div>
  </div>
</div>

<div class="mt-6 text-center">
  <span class="gc-pill--dark">Three failure modes where relying on one layer alone leaves you guessing.</span>
</div>

<!--
Sequence matters: the cluster is live before we argue about the concepts, so
every claim on the next slide can be checked against something running.
-->

---
layout: gc
eyebrow: Concepts
title: eBPF vs OTel
sub: Two different bargains with the same system. The question is never which one wins — it's which one you are paying for, and for what.
---

<div class="vs">
  <div class="gc-card gc-card--green vs__col">
    <h3 class="gc-card__title">eBPF sensor — and not OTel</h3>
    <ul class="gc-card__list">
      <li><b>100% sampling of error events</b> — there is always a sample, even where nobody instrumented</li>
      <li><b>Full request/response payloads</b> in spans, out of the box</li>
      <li><b>Zero instrumentation</b>, zero redeploys, zero SDK drift</li>
      <li><b>Automatic endpoint deduction</b> for APM measurements, with no code changes</li>
      <li><b>&lt;0.01% latency hit</b></li>
      <li><b>LLM observability</b> — full chat export of agent/model interaction: tokens, model, tool calls, skills</li>
    </ul>
  </div>
  <div class="gc-card gc-card--blue vs__col">
    <h3 class="gc-card__title">OTel — and not the eBPF sensor</h3>
    <ul class="gc-card__list">
      <li><b>Not limited to network boundaries</b> — custom spans can describe business-level procedures</li>
      <li><b>Distributed tracing</b> — the waterfall across services</li>
      <li>Relies on instrumentation, auto or manual</li>
      <li>No payloads unless you instrument for them</li>
      <li>5–15% latency, depending on sampling and other factors</li>
      <li>Sampling strategy management is varied and complex</li>
      <li>Auto-instrumentation gets spammy, which pushes teams to manual — more culture to maintain, and drift between signal and behavior</li>
    </ul>
  </div>
</div>

<div class="mt-5 text-center">
  <span class="gc-pill--dark">eBPF completes the picture OTel's distributed traces are drawing.</span>
</div>

<style>
.vs { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
.vs__col .gc-card__list li { font-size: 10.5px; line-height: 1.38; }
.vs__col .gc-card__list { gap: 8px; }
.vs__col { padding: 16px 18px 18px; }
.vs__col .gc-card__title { margin-bottom: 10px; font-size: 15px; }
</style>

<!--
Not a scoreboard. The eBPF column is coverage you get for free; the OTel column
is meaning you have to author. Both bills come due somewhere.
-->

---
layout: gc
eyebrow: eBPF coverage
title: What the sensor can actually see
sub: Protocols decoded on the wire, encrypted traffic included — with no code changes to any of it.
---

<div class="cap">
  <div class="cap__grid">
    <div class="gc-card gc-card--green cap__col">
      <h3 class="gc-card__title">Protocols decoded</h3>
      <div class="chips">
        <span>HTTP</span><span>gRPC</span><span>PostgreSQL</span><span>MySQL</span>
        <span>Redis</span><span>DNS</span><span>Kafka</span><span>MongoDB 3.6+</span>
        <span>AMQP 0-9-1</span><span>GraphQL</span><span>AWS S3</span><span>AWS SQS</span>
      </div>
      <p class="cap__note">Not just timings — the statement, the bound parameters, the status code, the payload on both sides of the call.</p>
    </div>
    <div class="gc-card gc-card--yellow cap__col">
      <h3 class="gc-card__title">Encryption it can read through</h3>
      <ul class="gc-card__list">
        <li><b>crypto/tls</b> — Go</li>
        <li><b>OpenSSL</b> — C, C++, Python</li>
        <li><b>NodeJS</b> — built-in TLS</li>
        <li><b>JavaSSL</b> — Java 11+, with the groundcover Java agent</li>
      </ul>
      <p class="cap__note">Say the caveat out loud: binaries compiled without debug symbols can't be decrypted.</p>
    </div>
    <div class="gc-card gc-card--blue cap__col">
      <h3 class="gc-card__title">What it costs to get there</h3>
      <ul class="gc-card__list">
        <li>Zero code changes, zero redeploys, no SDK in the request path</li>
        <li>Traces and metrics for every workload the moment the sensor lands</li>
        <li>Nothing to keep in version sync, nothing to drift</li>
        <li>Coverage whether or not anyone remembered to instrument</li>
      </ul>
    </div>
  </div>
  <a class="cap__src" href="https://docs.groundcover.com/capabilities/application-performance-monitoring-apm/supported-technologies" target="_blank">
    docs.groundcover.com › APM › Supported technologies ↗
  </a>
</div>

<style>
.cap { height: 100%; display: flex; flex-direction: column; }
.cap__grid { flex: 1; display: grid; grid-template-columns: 1.15fr 1fr 1fr; gap: 16px; min-height: 0; }
.cap__col { padding: 15px 16px 16px; }
.cap__col .gc-card__title { font-size: 14px; margin-bottom: 10px; }
.cap__col .gc-card__list { gap: 8px; }
.cap__col .gc-card__list li { font-size: 10.5px; line-height: 1.38; }
.cap__note { font-size: 9.5px; line-height: 1.4; color: var(--gc-slate); margin: auto 0 0; padding-top: 12px; }
.chips { display: flex; flex-wrap: wrap; gap: 6px; }
.chips span { background: #fff; border: 1px solid var(--gc-green); border-radius: 999px;
              padding: 4px 10px; font-size: 10px; font-weight: 600; }
.cap__src { display: block; margin-top: 14px; text-align: center; font-size: 10px;
            font-weight: 600; color: var(--gc-green); text-decoration: none;
            border: none !important; }
.cap__src:hover { text-decoration: underline; }
</style>

<!--
The point of this slide is that the coverage list is boring in the best way — it's
the long tail nobody instruments. PostgreSQL and DNS are the two that carry the
demo later. And TLS decryption is what makes the payment token story visible at all.
-->

---
layout: gc
eyebrow: eBPF coverage
title: And what comes out the other side
sub: Every decoded call becomes metrics with labels you never had to write.
---

<div class="cap">
  <div class="cap__grid met__grid">
    <div class="gc-card gc-card--green cap__col">
      <h3 class="gc-card__title">Golden signals, generated</h3>
      <ul class="met__code">
        <li>groundcover_resource_total_counter</li>
        <li>groundcover_resource_error_counter</li>
        <li>groundcover_resource_issue_counter</li>
        <li>groundcover_resource_success_counter</li>
        <li>groundcover_resource_latency_seconds</li>
      </ul>
      <p class="cap__note">Latency is a Summary — p50, p95, p99 ride along as a <b>quantile</b> label. A matching <b>workload</b> family covers everything a service handles.</p>
    </div>
    <div class="gc-card gc-card--yellow cap__col">
      <h3 class="gc-card__title">Everything under the app</h3>
      <ul class="gc-card__list">
        <li><b>Container, node and host</b> — CPU, memory, disk, I/O, network</li>
        <li><b>Pressure stalls</b> at 10s, 60s and 300s — how you rule out a noisy neighbor</li>
        <li><b>Kubernetes object state</b> — deployments, jobs, HPAs, PVCs, pod phase and restarts</li>
        <li><b>Kafka consumer lag</b> — in messages and in seconds</li>
      </ul>
    </div>
    <div class="gc-card gc-card--blue cap__col">
      <h3 class="gc-card__title">Labels you never wrote</h3>
      <div class="chips chips--blue">
        <span>clusterId</span><span>namespace</span><span>workload_name</span><span>pod_name</span>
        <span>remote_service_name</span><span>status_code</span><span>is_encrypted</span><span>is_cross_az</span>
      </div>
      <p class="cap__note">Per protocol you also get <b>clustered_path</b> and <b>method</b>, SQL <b>dialect</b> and <b>response_status</b>, Kafka <b>topic</b> and <b>partition</b>, DNS <b>query_type</b>. <b>is_cross_az</b> is the whole cross-AZ cost story in one label.</p>
    </div>
  </div>
  <a class="cap__src" href="https://docs.groundcover.com/use-groundcover/metrics-and-labels" target="_blank">
    docs.groundcover.com › Use groundcover › Metrics and labels ↗
  </a>
</div>

<style>
.cap { height: 100%; display: flex; flex-direction: column; }
.cap__grid { flex: 1; display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; min-height: 0; }
.cap__col { padding: 15px 16px 16px; }
.cap__col .gc-card__title { font-size: 14px; margin-bottom: 10px; }
.cap__col .gc-card__list { gap: 8px; }
.cap__col .gc-card__list li { font-size: 10.5px; line-height: 1.38; }
.cap__note { font-size: 9.5px; line-height: 1.4; color: var(--gc-slate); margin: auto 0 0; padding-top: 12px; }
.met__code { list-style: none !important; padding: 0 !important; margin: 0; display: grid; gap: 5px; }
.met__code li { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; font-size: 9.2px;
                background: #fff; border: 1px solid var(--gc-green); border-radius: 6px;
                padding: 5px 8px; list-style: none !important; }
.chips { display: flex; flex-wrap: wrap; gap: 6px; }
.chips--blue span { background: #fff; border: 1px solid var(--gc-blue); border-radius: 999px;
                    padding: 4px 9px; font-size: 9.5px; font-weight: 600; }
.cap__src { display: block; margin-top: 14px; text-align: center; font-size: 10px;
            font-weight: 600; color: var(--gc-green); text-decoration: none;
            border: none !important; }
.cap__src:hover { text-decoration: underline; }
</style>

<!--
This is the "you didn't write any of this" slide. Two labels to point at: is_encrypted,
because we just said we read through TLS, and is_cross_az, because that one label is a
cloud bill nobody was measuring.
-->

---
layout: gc
eyebrow: Architecture
---

<div class="a5">
  <div class="a5__diagram">
    <svg class="a5__wires" viewBox="0 0 900 335" fill="none">
      <polyline points="300,90 344,90 344,148 380,148" stroke="var(--gc-yellow-deep)" stroke-width="1.5" />
      <polyline points="300,248 344,248 344,203 380,203" stroke="var(--gc-ink)" stroke-width="1.5" />
      <line x1="620" y1="176" x2="690" y2="176" stroke="var(--gc-ink)" stroke-width="1.5" />
    </svg>
    <div class="a5__cluster">
      <div class="a5__tab">ShopIQ Cluster</div>
      <div class="a5__panel">
        <div class="a5__sigs">
          <span class="sig sig--y"><carbon:document /> Logs</span>
          <span class="sig sig--y"><carbon:chart-line /> Traces</span>
          <span class="sig sig--y"><carbon:flash /> Custom Metrics</span>
          <span class="sig sig--y"><carbon:chart-bar /> Infrastructure Metrics</span>
        </div>
        <div class="a5__sensor">eBPF Sensor</div>
      </div>
      <div class="a5__panel">
        <div class="a5__bar">OpenTelemetry</div>
        <div class="a5__bar">Data Sources</div>
        <div class="a5__sigs">
          <span class="sig"><carbon:document /> Logs</span>
          <span class="sig"><carbon:chart-line /> Traces</span>
          <span class="sig"><carbon:flash /> Custom Metrics</span>
          <span class="sig"><carbon:chart-bar /> Infrastructure Metrics</span>
        </div>
      </div>
    </div>
    <div class="a5__byoc">
      <div class="a5__byoctitle">groundcover<br/>BYOC Backend</div>
      <div class="a5__clouds"><span>AWS</span><span>GCP</span><span>Azure</span></div>
      <div class="a5__acct">
        <div class="a5__accttab">groundcover managed account</div>
        <div class="a5__k8s">
          <div class="a5__k8stitle"><carbon:kubernetes /> K8s Cluster</div>
          <div class="a5__plane">Managed<br/>Data Plane</div>
        </div>
      </div>
    </div>
    <div class="a5__caps">
      <span><carbon:flow /> APM</span>
      <span><carbon:layers /> Infrastructure Monitoring</span>
      <span><carbon:catalog /> Log Management</span>
      <span><carbon:application-web /> RUM</span>
      <span><carbon:chemistry /> Synthetic Monitoring</span>
      <span><carbon:machine-learning-model /> AI Observability</span>
      <span><carbon:dashboard /> Monitors &amp; Dashboards</span>
    </div>
  </div>
  <div class="a5__band">
    <div>
      <carbon:chip />
      <b>eBPF sensor: Zero instrumentation.</b>
      <p>Deploys instantly, 100% deep visibility. Edge processing.</p>
    </div>
    <div>
      <carbon:security />
      <b>BYOC: All inside your cloud.</b>
      <p>Fully managed, complete data sovereignty.</p>
    </div>
    <div>
      <carbon:network-3 />
      <b>Per node, not per GB ingested.</b>
      <p>No penalty for Enterprise-scale data. No sampling.</p>
    </div>
    <div>
      <carbon:magic-wand-filled />
      <b>Agent Mode.</b>
      <p>Investigate with your own LLM. Keep full control.</p>
    </div>
  </div>
</div>

<style>
.a5 { height: 100%; display: flex; flex-direction: column; }
.a5__diagram { position: relative; flex: 0 0 335px; }
.a5__wires { position: absolute; inset: 0; width: 100%; height: 100%; z-index: 0; }

/* left — the cluster */
.a5__cluster { position: absolute; left: 0; top: 0; width: 300px; height: 100%;
               border: 1px solid var(--gc-rule); border-radius: 14px; background: #fff;
               padding: 26px 10px 10px; display: flex; flex-direction: column; gap: 9px; }
.a5__tab { position: absolute; top: 0; left: 0; background: var(--gc-bg-3);
           border-radius: 14px 14px 0 0; padding: 5px 44px 5px 12px; font-size: 9px;
           font-weight: 500; color: var(--gc-ink); }
.a5__panel { background: var(--gc-bg-3); border-radius: 10px; padding: 10px;
             display: flex; flex-direction: column; justify-content: center; gap: 7px; flex: 1; }
.a5__panel + .a5__panel { flex: 1.25; }
.sig { display: flex; align-items: center; gap: 5px; font-size: 8.5px; color: var(--gc-ink);
       background: #fff; border: 1px solid var(--gc-rule); border-radius: 6px; padding: 7px; }
.sig svg { width: 10px; height: 10px; flex: 0 0 auto; color: var(--gc-slate); }
.sig--y { border-color: var(--gc-yellow); background: var(--gc-tint-yellow); }
.sig--y svg { color: var(--gc-yellow-dark); }
.a5__sigs { display: grid; grid-template-columns: 1fr 1fr; gap: 7px; }
.a5__sensor { border: 1px dashed var(--gc-yellow); border-radius: 7px;
              background: #fff; text-align: center; font-size: 9.5px; font-weight: 600;
              padding: 8px 0; }
.a5__bar { background: var(--gc-ink-2); color: #fff; border-radius: 6px; text-align: center;
           font-size: 9.5px; font-weight: 600; padding: 7px 0; }

/* middle — BYOC */
.a5__byoc { position: absolute; left: 380px; width: 240px; top: 56px;
            background: #E9EFFD; border: 1px solid var(--gc-blue); border-radius: 14px;
            padding: 14px 16px 16px; }
.a5__byoctitle { font-size: 14px; font-weight: 700; line-height: 1.2; }
.a5__clouds { display: flex; gap: 14px; margin-top: 9px; }
.a5__clouds span { font-family: 'Space Grotesk','Inter',sans-serif; font-size: 10px;
                   font-weight: 500; letter-spacing: .06em; color: var(--gc-slate); }
.a5__acct { margin-top: 11px; background: var(--gc-bg-2); border-radius: 10px; padding: 19px 9px 9px;
            position: relative; }
.a5__accttab { position: absolute; top: 0; left: 0; background: var(--gc-bg-2);
               border-radius: 10px 10px 0 0; padding: 4px 12px 3px; font-size: 7.5px;
               color: var(--gc-muted); }
.a5__k8s { background: var(--gc-bg-3); border-radius: 8px; padding: 8px; text-align: center; }
.a5__k8stitle { display: flex; align-items: center; justify-content: center; gap: 5px;
                font-size: 9px; color: var(--gc-slate); }
.a5__k8stitle svg { width: 11px; height: 11px; }
.a5__plane { margin-top: 7px; background: #C9D8FB; border-radius: 7px; padding: 9px 0;
             font-size: 11px; font-weight: 700; line-height: 1.25; }

/* right — the capability list */
.a5__caps { position: absolute; right: 0; top: 0; width: 210px; height: 100%;
            border: 1px solid var(--gc-rule); border-radius: 14px; background: #fff;
            padding: 11px; display: grid; grid-template-rows: repeat(7, 1fr); gap: 5px; }
.a5__caps span { display: flex; align-items: center; gap: 8px; background: var(--gc-bg-3);
                 border-radius: 7px; padding: 0 10px; font-size: 10px; font-weight: 600; }
.a5__caps svg { width: 13px; height: 13px; flex: 0 0 auto; }

/* bottom — the value band, full bleed */
.a5__band { flex: 1; margin: 14px -40px -30px; background: #F7E9B8;
            display: grid; grid-template-columns: repeat(4, 1fr); gap: 18px;
            padding: 10px 40px 12px; align-content: center; }
.a5__band > div { text-align: center; }
.a5__band svg { width: 15px; height: 15px; margin: 0 auto 4px; }
.a5__band b { display: block; font-size: 10px; letter-spacing: -.01em; }
.a5__band p { font-size: 8.5px; color: var(--gc-slate); margin: 3px 0 0; line-height: 1.35; }
</style>

<!--
Same architecture we ship: the sensor rides along in the cluster, the backend runs
in the customer's own cloud account, and the product surfaces on the right are all
fed from the same pipeline. The cluster on the left is the ShopIQ cluster — next
slide opens it up.
-->

---
layout: gc
eyebrow: The app
title: Inside the ShopIQ cluster
sub: One cluster, one namespace, eight services.
---

<div class="sq">
  <div class="sq__cluster">
    <div class="sq__tab"><carbon:kubernetes /> ShopIQ Cluster — namespace shopiq</div>
    <div class="sq__flow">
      <div class="sq__col sq__col--load">
        <div class="node node--gen"><b>k6</b><span>browse · checkout · chat</span></div>
        <div class="node node--gen"><b>browser-synthetic</b><span>Playwright · 4 VUs</span></div>
      </div>
      <div class="arw">→</div>
      <div class="sq__col sq__col--edge">
        <div class="node node--edge"><b>frontend:80</b><span>nginx · React SPA<br/>groundcover RUM SDK</span></div>
      </div>
      <div class="arw">→</div>
      <div class="sq__col sq__col--gw">
        <div class="node node--gw"><b>api-gateway:3000</b><span>W3C baggage<br/>shopiq.request.id</span></div>
      </div>
      <div class="arw">→</div>
      <div class="sq__col--svc">
        <div class="srow">
          <div class="node node--svc"><b>catalog-service:3001</b><span>catalog.search</span></div>
          <span class="arw arw--sm">→</span>
          <div class="node node--dep"><b>PostgreSQL · Redis</b></div>
        </div>
        <div class="srow">
          <div class="node node--svc"><b>inventory-service:3002</b><span>stock reservation</span></div>
          <span class="arw arw--sm">→</span>
          <div class="node node--dep"><b>PostgreSQL</b></div>
        </div>
        <div class="srow">
          <div class="node node--svc"><b>orders-service:3003</b><span>order.create · order.confirm</span></div>
          <span class="arw arw--sm">→</span>
          <div class="node node--dep"><b>payments-service:3004</b><span>payment.charge → mock-payment-gateway:3006</span></div>
        </div>
        <div class="srow">
          <div class="node node--svc"><b>chatbot-service:3005</b><span>invoke_agent · execute_tool</span></div>
          <span class="arw arw--sm">→</span>
          <div class="node node--dep"><b>mock LLM · 4-model pool</b><span>gpt-4o · gpt-4o-mini · claude-sonnet · claude-haiku</span></div>
        </div>
      </div>
    </div>
  </div>
  <div class="sq__pipe">
    <div>
      <carbon:code />
      <b>OTel SDK loads before any app code.</b>
      <p>Auto-instrumented HTTP, Express, PostgreSQL, Redis, DNS — plus hand-written spans for the business steps.</p>
    </div>
    <div>
      <carbon:chip />
      <b>eBPF sensor sees every hop anyway.</b>
      <p>Payloads and SQL statements included, with no SDK in the request path and nothing to redeploy.</p>
    </div>
    <div>
      <carbon:arrow-right />
      <b>Both land in groundcover.</b>
      <p>OTLP via the otel-collector DaemonSet, correlated end to end by shopiq.request.id.</p>
    </div>
  </div>
</div>

<style>
.gc-title { font-size: 25px; }
.gc-sub { font-size: 11px; margin-top: 7px; }
.gc-body { margin-top: 16px !important; }

.sq { height: 100%; display: flex; flex-direction: column; }
.sq__cluster { position: relative; flex: 0 0 240px; border: 1px solid var(--gc-rule);
               border-radius: 14px; background: #fff; padding: 24px 12px 12px; }
.sq__tab { position: absolute; top: 0; left: 0; background: var(--gc-bg-3);
           border-radius: 14px 14px 0 0; padding: 5px 22px 5px 12px; font-size: 9px;
           font-weight: 500; color: var(--gc-ink); display: flex; align-items: center; gap: 5px; }
.sq__tab svg { width: 11px; height: 11px; color: var(--gc-slate); }

.sq__flow { display: flex; align-items: stretch; gap: 5px; height: 100%; }
.sq__col { display: flex; flex-direction: column; justify-content: center; gap: 9px; }
.sq__col--load { flex: 0 0 110px; }
.sq__col--edge { flex: 0 0 104px; }
.sq__col--gw   { flex: 0 0 104px; }
.sq__col--svc  { flex: 1; display: grid; grid-template-rows: repeat(4, 1fr); gap: 7px; }
.srow { display: grid; grid-template-columns: 1fr 16px 1.15fr; align-items: center; }

.node { border: 1px solid var(--gc-rule); border-radius: 8px; background: #fff;
        padding: 7px 9px; }
.node b { display: block; font-size: 9.5px; font-weight: 700; letter-spacing: -.01em; }
.node span { display: block; font-size: 8px; line-height: 1.35; color: var(--gc-slate); margin-top: 2px; }
.node--gen  { background: var(--gc-bg-3); }
.node--edge,
.node--gw   { border-color: var(--gc-yellow); background: var(--gc-tint-yellow); }
.node--svc  { border-color: var(--gc-green); background: var(--gc-tint-green); }
.node--dep  { border-color: var(--gc-blue); background: var(--gc-tint-blue); }

.arw { display: flex; align-items: center; justify-content: center; flex: 0 0 15px;
       font-size: 12px; color: var(--gc-muted); }
.arw--sm { font-size: 11px; }

.sq__pipe { flex: 1; margin: 12px -40px -30px; background: #F7E9B8;
            display: grid; grid-template-columns: repeat(3, 1fr); gap: 22px;
            padding: 10px 40px 12px; align-content: center; }
.sq__pipe > div { text-align: center; }
.sq__pipe svg { width: 15px; height: 15px; margin: 0 auto 4px; }
.sq__pipe b { display: block; font-size: 10px; letter-spacing: -.01em; }
.sq__pipe p { font-size: 8.5px; color: var(--gc-slate); margin: 3px 0 0; line-height: 1.35; }
</style>

<!--
This is the app we just deployed. Green is ours to instrument, blue is what it
talks to, and the whole thing sits in one cluster so the failures later have a
shared blast radius. The yellow band is the point: two collection layers, one
backend, one request id stitching them together.
-->

---
layout: gc
eyebrow: The demo
title: Three failures, one argument
sub: eBPF supplies the ground truth. The agent turns it into an answer. A skill turns that answer into something the next investigation inherits.
---

<Cards :items="[
  {
    icon: '🐢',
    title: 'Slow SQL query',
    points: [
      '<b>Exists:</b> eBPF pulls the bound parameter out of the query — add <code>$1</code> as a column, Show Distribution, filter that user out and the tail latency disappears. Agent Mode is right there.',
      '<b>Build:</b> an SQL-troubleshooting skill that steers the agent to the grounded finding — one user_id on a hot shard — instead of a confident invented one.',
    ],
  },
  {
    icon: '🗄',
    title: 'Missing table',
    points: [
      '<b>Exists:</b> SQL error types on demand — 42P01 undefined_table, 55006 object_in_use, 40P01 deadlock — with the full statement, status and params on the span.',
      '<b>Build:</b> make it a believable incident (a stale schema after a migration), run the RCA, and capture it as a reusable skill.',
    ],
  },
  {
    icon: '🔊',
    title: 'Noisy neighbor',
    points: [
      '<b>Exists:</b> Node Resources and Noisy Neighbors panels inside the span Context tab, with the trace moment marked on the timeline.',
      '<b>Build:</b> a co-located PostgreSQL and a CPU-spiking neighbor timed to the slow windows, plus a skill that rules out node contention first.',
    ],
  },
]" />

<div class="mt-4 text-center">
  <span class="gc-pill--dark">A dashboard is an artifact of failure. The artifact of a good investigation is a skill.</span>
</div>

<style>
.gc-cards { height: auto; gap: 14px; }
.gc-cards :deep(.gc-card) { padding: 13px 15px 15px; }
.gc-cards :deep(.gc-card__icon) { font-size: 16px; margin-bottom: 7px; }
.gc-cards :deep(.gc-card__title) { font-size: 14px; margin-bottom: 8px; }
.gc-cards :deep(.gc-card__list) { gap: 8px; }
.gc-cards :deep(.gc-card__list li) { font-size: 9.6px; line-height: 1.4; }
.gc-cards :deep(code) { font-size: 9px; background: rgba(0,0,0,.05); padding: 1px 4px; border-radius: 4px; }
</style>

<!--
Say the skill part out loud on stage: the DevOps team codified their investigation
know-how, and that's not cheating — it's the whole point. First time we diagnose a
class of failure we write it down; every agent run after that starts already knowing.
-->

---
layout: gc-statement
headline: '<span style="color:var(--gc-muted);font-size:26px;letter-spacing:0;display:block;margin-bottom:10px">Example 01</span>The slow SQL query'
sub: 'The bound parameter behind the tail latency — and the skill that stops the agent guessing.'
cta: 'Live demo'
---

<!--
Placeholder. Switch to groundcover here: add $1 as a column, Show Distribution,
filter the user out, watch the tail collapse. Then open Agent Mode.
-->

---
layout: gc-statement
headline: '<span style="color:var(--gc-muted);font-size:26px;letter-spacing:0;display:block;margin-bottom:10px">Example 02</span>The table that isn''t there'
sub: 'An L7 failure instrumentation never reported — captured statement, status and params included.'
cta: 'Live demo'
---

<!--
Placeholder. 42P01 undefined_table on the span, full statement attached. Frame it
as a stale schema after a migration, run the RCA, capture it as a skill.
-->

---
layout: gc-statement
headline: '<span style="color:var(--gc-muted);font-size:26px;letter-spacing:0;display:block;margin-bottom:10px">Example 03</span>The noisy neighbor'
sub: 'Ruling out node contention before you go blame the query.'
cta: 'Live demo'
---

<!--
Placeholder. Node Resources and Noisy Neighbors panels inside the span Context tab,
with the trace moment marked on the timeline.
-->

---
layout: gc-statement
headline: 'Thank you.'
sub: '<b>David Flanagan</b> · CoreWeave &amp; Rawkode Academy · <span class="gc-hl-g">@rawkode</span><br/><b>Anais Dotis-Georgiou</b> · Developer Advocate, <span class="gc-hl-g">groundcover</span><br/><br/>Try it live, nothing to install — <a href="https://play.groundcover.com/" target="_blank" style="color:var(--gc-green);font-weight:700;text-decoration:none;border:none;border-bottom:1.5px solid rgba(0,164,123,.45)">play.groundcover.com</a><br/>Then run it in your own cluster — <a href="https://www.groundcover.com/" target="_blank" style="color:var(--gc-green);font-weight:700;text-decoration:none;border:none;border-bottom:1.5px solid rgba(0,164,123,.45)">groundcover.com</a><br/>Docs — <a href="https://docs.groundcover.com/" target="_blank" style="color:var(--gc-green);font-weight:700;text-decoration:none;border:none;border-bottom:1.5px solid rgba(0,164,123,.45)">docs.groundcover.com</a>'
cta: 'Questions — the cluster stays up.'
---

<!--
Leave the ShopIQ dashboards on screen behind the Q&A if the room is still asking.
-->
