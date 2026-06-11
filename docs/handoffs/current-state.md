# NEXW 自動化系統 — Current State

**Last updated:** 2026-06-11(PR-23.1 DB1 nexw-discord-bot container 骨架 shipping;PR-D12-hotfix ✅ merged `0fff2c0` #260;Sprint 23 進行中)

## 1. 階段

**v2.0 實作階段 — Sprint 17 規劃中**(2026-06-08 Sprint 16 closed)

- v1.0.md FROZEN,不再 maintain([v1.0.md](../automation/v1.0.md))
- v2.0.md Sprint 4-21 規格書定稿([v2.0.md](../automation/v2.0.md))
- Sprint 3 closed at PR-3.12,Sprint 編號從 Sprint 4 承接(server Sprint = v2.0 §15 Sprint,強制同步)
- **Sprint 4 ✅ Closed at 2026-05-03**(I1 + J2 + J1 半自動 ship,J1 全自動 timer 移 v2.1 candidate;close report:[sprint-4-close-report.md](../sprints/sprint-4-close-report.md))
- **Sprint 5 ✅ Closed at 2026-05-05**(LiteLLM Proxy + H1 Claude API + E1/E2 Secret + 方案 X/Y 跨 chat 接手包,6/6 模組真打 PASS;9 個 feature/hotfix PR + 1 個 verify noop ship;close report:[sprint-5-close-report.md](../sprints/sprint-5-close-report.md))
- **Sprint 6 ✅ Closed at 2026-05-27**(close report:[sprint-6-close-report.md](../sprints/sprint-6-close-report.md))
- **Sprint 7 ✅ Closed at 2026-05-29**(close report:[sprint-7-close-report.md](../sprints/sprint-7-close-report.md))— F1/D2/H2/D4 主軸全 ship + Sprint 6 移交 I2/B4 完成;真打全綠(F1 NEXW 站雙發 / koalo-api/docs Swagger 200 / H2 L18+TG)
- **Sprint 8 ✅ Closed at 2026-06-01**(close report:[sprint-8-close-report.md](../sprints/sprint-8-close-report.md))— H7a 7 步 onboard 自動化 + H4 全路徑倒數 rollback(execute_deploy 六入口 + /update + Stage 2);真打全綠(拋棄式 VM;H4 Stage 2 Phase 3 自動 rollback / Phase 4 cancel,零停機,taoyuan 未碰);Gap 7 + clone-if-absent → Sprint 9
- **Sprint 9 ✅ Closed at 2026-06-02**(close report:[sprint-9-close-report.md](../sprints/sprint-9-close-report.md))— H7b 首次部署鏈三塊串接打通(clone-if-absent + known_hosts + onboard repos 認領)+ G2 Memory Layer 設計 part 1(分層查找的持久記憶);真打全綠(拋棄式 VM:onboard → 首次 /deploy → 自動 clone → production live,連環暴露 host key + deploys gap 各補,taoyuan 未碰);Gap 7 + H7b 收尾 + split-repo clone + /remove_server → backlog
- **Sprint 11 ✅ Closed at 2026-06-07**(close report:[sprint-11-close-report.md](../sprints/sprint-11-close-report.md))— A2 Idea-to-Spec(`/expand` `/spec` 多輪→Sonnet 組 spec)+ H5 三家 Coding Agent CLI headless(claude/codex/gemini、file-queue、`/agent [engine]`、L0 scratch 隔離、admin-gated)+ G3 跨模型 context 注入(`context_export` L1+L2 → agent & A2;L2 backfill 4 產品);15 PR + 1 chore 全 merged。OBS O1-O8 見下方 Known Issues / Follow-up。
- **Sprint 12 ✅ Closed at 2026-06-07**(close report:[sprint-12-close-report.md](../sprints/sprint-12-close-report.md))— F2(条 10)`/new_from_url <url> <name>` → F1 scrape → A2 spec → 建 repo + bootstrap 檔(atomic commit + wait-init gate)→ webhook → 註冊 product → 可 /deploy;B1(条 2 規則式 v1)部署失敗 → TG 自動「🔍 可能原因 + 建議」(6-rule 引擎,hook /deploy + Stage 2,fail-safe);9 PR 全 merged #209-#217。
- **Sprint 13 ✅ Closed at 2026-06-07**(close report:[sprint-13-close-report.md](../sprints/sprint-13-close-report.md))— H8a RN App pipeline(方案 A:build 觸發 + 監看觀測,免費 Android 真打):eas 執行容器地基 + EAS webhook→TG observe(HMAC-SHA1)+ app-mode registry/`/register_app` + `/app_build`(clone→eas build→webhook→TG)。real-fire 端到端 ✅ 驗 2 次。5 PR 全 merged #219-#223。
- **Sprint 14 ✅ Closed at 2026-06-07**(close report:[sprint-14-close-report.md](../sprints/sprint-14-close-report.md))— Multi-AI part 1:LiteLLM 加 OpenAI provider(gpt-flagship/gpt-nano)+ Gemini provider(gemini-pro/gemini-flash-lite,`gemini/` AI Studio 路)+ Multi-AI 路由初步(`model_router` 任務→model;classify→免費 gemini-flash-lite,其餘 claude-sonnet)。3 PR 全 merged #225-#227。⚠️ 真 key(OPENAI/GEMINI)已上線 server .env。
- **Sprint 15 ✅ Closed at 2026-06-07**(close report:[sprint-15-close-report.md](../sprints/sprint-15-close-report.md))— Multi-AI part 2:budget UI 多家化 + cap↑$2000 + 80%/100% warning 主動推 TG + 跨 provider fallback(router_settings)+ Eval `/eval` 同題並發三家 + per-task effort 旋鈕骨架(只 complex=high)。6 PR 全 merged #229-#234。effort 值延後(OBS-KK)、完整路由內容感知不做(O4 過早優化)。
- **Sprint 16 ✅ Closed at 2026-06-08**(close report:[sprint-16-close-report.md](../sprints/sprint-16-close-report.md))— C1 Daily Briefing(每日各專案近況 Haiku 合成 → TG,失敗退純格式)+ H6 Auto Merge 4 道防護(merge 前 gate:no-conflict + tests-pass + human approval + DoD)+ H8b Flutter prompt readiness(pipeline 自動化延後至接第一個 Flutter 客戶)。3 PR 全 merged #236-#238。
- **當前 Sprint:** **v3.0 階段 — Sprint 23 待開工**(Discord bot 地基 + central adapter)。v2.0 已封版(2026-06-09,18 Sprint 4-21 / 16-16 active 100%);**R-001 於 2026-06-11 廢止 30 天設計凍結提前解凍**(原至 2026-07-09)→ v3.0 即日動工,建造期新需求進 v4.0 候選池。live 進度見 [v3.0-progress.md](../sprints/v3.0-progress.md)。
- **設計凍結:** 2026-06-09 啟動,30 天(至 2026-07-09)。期間新需求一律進 v3.0 候選池(`v2.0.md §13`,現 5 條:圖影片 model / 四種 PPT C3-C6 / D1 footer / B3 Proactive / A3 W-only 整合),**不擾動 v2.0**。凍結期滿評估候選池排 v3.0。
- **當前 PR:** PR-23.1 DB1 nexw-discord-bot container 骨架(shipping;runtime-wired)— bot 第一次上線:login + agent-測試區報到 + ping-pong;不接引擎(adapter = PR-23.2)
- **Next action:** PR-23.1 merge 後 TEEMO:server checkout main → rebuild `discord-bot`(--no-cache,可帶 DBOT_GIT_SHA)→ up → **L18 smoke**(container Up + login 成功行無 traceback)→ **Discord 真打**(bot 變綠 / agent-測試區上線訊息 / `ping`→`pong`);失敗回滾 `docker compose stop discord-bot`(獨立 service)。接 PR-23.2 DB2 adapter 層
- **(shipping)PR-23.1** DB1 nexw-discord-bot container 骨架 — 新 `dbot/`(⚠️ 不叫 discord/,防套件撞名):config.py(token `/data/secrets/bot-tokens/discord.txt` 釘死 + `/data/discord-config.json` guild_id/channels 驗證,缺檔 ConfigError)+ main.py(discord.py gateway;intents message_content+members;on_ready mapping sanity check〔包含比對,WARNING 不 crash〕→ agent-測試區上線訊息〔版本/sha/時間〕;ping/@bot → pong+時間戳)+ Dockerfile(python:3.12-slim,COPY *.py 慣例,ARG GIT_SHA,無 HEALTHCHECK)+ requirements(discord.py>=2.4,<3)。compose 新 `discord-bot` service(central-bot 同型:/srv/bot-state:/data rw;無 Traefik/healthcheck/port;既有 5 services 0 改)。12 測綠(config 層;gateway 屬真打)。TG 0 擾動。⚠️ **runtime-wired → merge 後 rebuild + L18 smoke + Discord 真打**。
- **(已 merged)PR-D12-hotfix** actions/checkout@v4→v5 ✅ merged `0fff2c0`(#260)— state-mirror workflow checkout 升版(Node 24);V1-V5 驗收照 PR-D12 流程。
- **(已 merged)PR-D12** state 鏡射 public repo + 全 repo 轉 private 前置 ✅ merged `69bf877`(#259)(**過渡件,Sprint 23 DB5 後退役**)— 新 `.github/workflows/state-mirror.yml`(本 repo 首個 workflow):main push 動 state 三檔(current-state / v2.0-progress / v3.0-progress)→ 鏡射到 public `xxnexw/nexw-state-mirror`(保持相對路徑),讓「全 repo 轉 private 後」跨-chat 接手 SOP 仍能讀 state。機制 = B-1 GitHub Actions;`MIRROR_PUSH_TOKEN` 只經 secrets 引用、絕不 echo/log;有 diff 才 push(idempotent)。**順序鐵律:鏡射先活 → SOP 驗證 → 才轉 private。** forbidden 0 改:除 workflow + §6 docs 外 0 changes。⚠️ **merge 後 Actions 自動跑(V1)** → TEEMO 驗收 V1-V5(轉 private 在 V4,可逆回滾)。
- **(已 merged)PR-22.1** v3.0 kickoff ✅ merged `925a491`(#258)— v3.0.md 定稿進 repo(13 Sprint 22-34)+ v3.0-progress.md 建立 + Sprint 22 ✅ Closed + v2.0-progress 封存(self-stale 全翻 PR-21.2 → 610ba15#257)+ changelog R-001 kickoff 筆。
- **(已 merged)PR-21.2** Sprint 21 Close + v2.0 封版 ✅ merged `610ba15`(#257)— v2.0 整體驗收通過(matrix + smoke 全綠 2026-06-09);Sprint 21 ✅ Closed + 進度條 16/16(100%)+ master 訴求表 hygiene 刷 + Sprint 7 header 補 ✅ Closed + self-stale 回填 PR-21.1 `2387e96`#256 + 新 `sprint-21-close-report.md`。
- **(已 merged)PR-21.1** v2.0 整體驗收 ✅ merged `2387e96`(#256)— `v2.0-acceptance-report.md`(矩陣 + 15 條訴求 + KI + v3.0 移交 + smoke runbook)+ `v2.0.md §13` A3→v3.0。
- **(已 merged)PR-20.6** Sprint 20 Close 儀式 ✅ merged `3f05ca1`(#255)— Sprint 20 ✅ Closed;進度條 15/16 active(94%);B3 → v3.0;self-stale 回填 PR-20.5/20.0。
- **(已 merged)PR-20.5** bot teardown 逐 repo footprint ✅ merged `035e558`(#254)— `teardown_product_staging` 原只看單一 `/srv/projects/<product>` → rdot 無此目錄、backend 在 sibling `/srv/projects/rdot-backend` → `/delete_product rdot` 孤兒化 backend 容器+目錄。修:step 2/3 額外**逐 repo 用 `resolve_project_path` 解實體目錄併進** compose_down + rmtree 集合(deepest-first rmtree + is_dir 守衛);app repo(無 compose)→ resolve None → 跳過。step 1(客戶偵測)+ step 4(紀錄清理)**0 改**;結構上仍只碰本機 staging,客戶 server 0 mutation。**rebuild target = `bot` 容器**(teardown_poller 跑在 bot,非 central-bot)。forbidden 0 改:`projects.py`(只讀用)/ `docker_ops.py` / `teardown_poller.py` / `central/` / Dockerfile。3 層綠(py_compile + ruff + 20 測〔含 sibling/split/single/app-only〕 / bot 379 pass,0 regression)。⚠️ **runtime-wired → merge 後 rebuild `bot` + L18 smoke + 真打**(拋棄式 sibling 容器仿 rdot 佈局 → `/delete_product` 驗 sibling 容器停 + 目錄清;真實產品全程不碰)。
- **(已 merged)PR-20.4** D3 執行(1)`/rename_product` 引導式改名 ✅ merged `11c07f3`(#253)— central record rename(`storage.rename_product` 可逆、confirm-gated)+ 手動 checklist + 文字 confirm;preview 0 mutation。
- **(已 merged)PR-20.3** B2-c:rename 影響面偵測 + 折進 `/rename_plan` ✅ merged `9421040`(#252)— 把 rename 計畫的舊識別字(舊產品名 + 各舊 repo 名)餵進既有 `watch_mode.analyze_impact`,偵測「改名會牽動哪些**別的**產品/部署/設計/對話」,結果折進 `/rename_plan` 輸出末端(計畫 + 🔍 影響面 一次出)。spec §15.20 B2-c:改動前自動觸發、不靠手動 `/watch_impact`。`watch_mode.py` **additive**(新增 `analyze_rename_impact` + `render_rename_impact`,不動既有 `analyze_impact`/`_hit_*`/規則表):餵舊名+舊 repo 名、**排除自身**、**dedupe 按 product 留最高 severity**(substring 比對會重疊)、排序 high→low 再 product。§1 校正:`analyze_impact` 回的 `impacts` 是 **plain dict**(非 Impact 物件)→ 用 `imp["..."]` 取值。handler 折進 reply(`render_plan` + `render_rename_impact`,既有 `_chunks`);admin gate/product_exists/InvalidProductNameError 不動。鐵律:`analyze_rename_impact` 只呼叫 `analyze_impact`(只讀記憶層),handler 仍 storage 零 mutation。forbidden 0 改:`analyze_impact`/規則表 / `rename_plan.py` 引擎 / storage / domain_resolver / memory_layer 內部 / bot/ / Dockerfile。3 層綠(py_compile + ruff + 31 pass / central 1074 pass,0 regression)。⚠️ **runtime-wired → merge 後 rebuild central-bot + L18 smoke + TG 唯讀 smoke**(`/rename_plan rdot foo` 末端多 🔍 影響面段;rdot 多半無別產品引用 → 「✅ 相對安全」)。L2 卡片住 server `/data`,真實影響面 merge 後 server 看。
- **(已 merged)PR-20.2-hotfix** rename-plan 引擎 type-aware(app repos)✅ merged `66bb32a`(#251)— `type:app` repo(rdot-rn)跳過 server_dir/registry/repo_internal、改 emit `app` surface(EAS rename 手動);render product_record 標 target label;回歸保護:無 type = server 路徑。
- **(已 merged)PR-20.2** D3-a 唯讀 rename-plan 引擎 + `/rename_plan` ✅ merged `cc85072`(#250)— 給 `<old> <new>` 唯讀算 rename 動到哪些面(product 記錄 / repo / nexw domain / 慣例 server dir / registry)各自 current→proposed;A1 deferred repo 內部檔+GitHub rename;B1 路徑慣例自算;只讀 + 絕不寫(唯讀鐵律測試)。新 `central/rename_plan.py` + `handlers/rename_plan.py` + 註冊行。
- **(已 merged)PR-20.1** rdot-A:rdot-backend 連進記錄 ✅ merged `6a9dd0c`(#249)— 一次性 backfill script `central/backfill_rdot_backend.py` 把 `rdot-backend`(`{type:backend}`)append 進 rdot `repos`;`KI-rdot-backend-unlinked`(repos 層)關。
- **(已 merged)PR-20.0** Roadmap re-scope:跳過 Sprint 18/19 ✅ merged `d02f602`(#248)— S18(C3/C4 PPT)+ S19(C5/C6 PPT + D1 footer)全砍 → v3.0 候選池(§13);條 4/5 移出 v2.0;下一站 Sprint 20(D3 + B3 + S17 延後的 B2-c hook 落地)。改 4 docs(v2.0.md / progress / current-state / changelog)。
- **(已 merged)PR-17.7** Sprint 17 Close 儀式(含 J3 demo2 收尾)✅ merged `ed84dec`(#247)— §7 Sprint Close:backfill PR-17.6 → merged `c697cc6`#246;Sprint 17 模組 checklist 標 ✅ Closed〔B2 Watch Mode + J4 `/delete_product` 完整化 + **J3 demo2 pre-completed**〕;進度條 13→14/18(78%);當前狀態 → Sprint 18;新 `sprint-17-close-report.md` + changelog 檔尾「Sprint 17 closed」筆。**J3 0 code**:demo2 殘留 2026-05-01 已清、staging 已 `*.nexw.com` 正式慣例、2026-06-09 server recon 七項零殘留。carry:B2-c→S20 D3、`KI-rdot-backend-unlinked`、Sprint 16 carries。⚠️ **純 docs → merge 後無 rebuild、無真打,merge 完 Sprint 17 正式 closed。**
- **(已 merged)PR-17.6** J4-c products-reality 重 audit + KI close + Retired 慣例 ✅ merged `c697cc6`(#246)— **J4 收尾末塊**。① products-reality:新增 `rdot` entry(app + split backend,⚠️ 標 backend 未連入記錄的 latent orphan gap)+ koalo container 3→4(加 `koalo-typesense` + healthcheck false-positive 註記)+ 速查表 Total 改 grounded 基準(9 product containers + 5 deploy-bot ecosystem;traefik/registry 屬 _infra 不計) + alaska-monitor 標「非追蹤單元(勿誤判為孤兒)」+ Last audit 日期 + Retired 慣例釘樁(手動 PR 步驟、非 `/delete_product` 自動)。② current-state §8.1:關閉「`/delete_product` cleanup 不完整」KI(✅ RESOLVED by J4-a/b;legacy ①②③ 標 OBSOLETE〔現代產品不建 per-product bot〕、④ container-reap 由 `compose_down` 處理)+ 新增 `KI-rdot-backend-unlinked`(LOW/follow-up)。**forbidden 0 改:任何 `.py`/`bot/`/`central/`/Dockerfile/compose/test。只動 3 docs。** ⚠️ **docs-only → merge 後無 rebuild、無真打。**
- **(已 merged)PR-17.5** J4-b-2 central Teardown client + wire `/delete_product` ✅ merged `67d57b1`(#245)— 新 `central/teardown_client.py`(mirror onboard_client):`write_teardown_intent`(atomic_io 寫 `/data/teardown-intents`)+ `poll_teardown_result`(`TeardownResultTimeout`,120s)。改 `handlers/delete_product.py` `cb_delete_product`:confirm 後 → 寫 intent → `_teardown_and_finalize`:poll →(逾時/crash→不刪)→ `customer_deployments` 非空 → **decision b:不呼叫 `storage.delete_product`** + 印手動 checklist(只 whitelist,無 host)+「記錄保留」;空 → 既有刪除邏輯原樣搬。⚠️ **runtime-wired(central):TEEMO 仍待 rebuild central-bot + L18 smoke + 破壞性真打**(等 spec owner 腳本)。
- **(已 merged)PR-17.4** J4-b-1 bot 側 Teardown Poller ✅ merged `ea02ae5`(#244)— 新 `bot/src/services/teardown_poller.py`(mirror onboard_poller):撿 `/data/teardown-intents/<id>.json` pending → `await teardown_product_staging` → 寫 `/data/teardown-results/<id>.json` → flip intent done(先寫 result 再 flip,crash-safe idempotent)。**並發鐵律**:`registry`/`deployments` 用 main shared instance 傳進(不自建)。唯一改既有 = `main.py`(create_task + cleanup)。
- **(已 merged)PR-17.3** J4-a Product Teardown primitive ✅ merged `05c3c0d`(#243)— `teardown_product_staging`(結構上只碰 staging:layout-agnostic compose_down + rmtree + deployments.remove)+ `detect_customer_deployments`(productions_for_repo + dedupe,只讀 whitelist)。`docker_ops.compose_down` + `server_deployments.remove`(additive)。客戶 server 零觸碰。
- **(已 merged)PR-17.2-hotfix** B2 引擎聚合修正 ✅ merged `e43e23e`(#242)— `_scan_product` collect-all(同卡多維度多筆,card-generic 只墊底)+ `_hit_repos`/`_hit_deploy` 點名〔whitelist〕。
- **(已 merged)PR-17.2** B2-b Watch Mode TG 指令 `/watch_impact` ✅ merged `a988685`(#241)— 新 `central/handlers/watch_impact.py`(admin,asyncio.to_thread 引擎,🔴/🟡/⚪ render,_chunks 分段)+ `handlers/__init__.py` wiring。mirror agent.py 但簡化(無 job_queue / 無 daily limit)。
- **(已 merged)PR-17.1** B2-a Watch Mode 衝突偵測規則引擎 ✅ merged `9168ea9`(#240)— 新 `central/watch_mode.py` `analyze_impact(change={kind,target})`(純函式 L0):讀 G2 Memory(memory_layer public API 限定)→ 回 `{hit, impacts[], summary, sources}`;`WATCH_PROBES`(repos/deploy→high、design→medium、conversation/card-generic→low)。secret-safe + fail-safe + 禁 import bot/ triage。
- **(已 merged)PR-16.2** H6 Auto Merge 4 道防護 ✅ merged `cd4755c`(#237)— merge 前 4 道 gate(no-conflict + tests-pass〔今 neutral,待 PAT Checks:read〕+ human approval + DoD,全秀);block 不 merge。`github_api` +`head_sha`/`CheckStatus`/`get_checks_status`(非 200 degrade neutral)。
- **(已 merged)PR-16.1** C1 Daily Briefing ✅ merged `cd43e8b`(#236)— 新 `central/daily_briefing.py`(L1+L2 whitelist → Haiku 合成,失敗退 `_format_plain`)+ `central/handlers/briefing.py`(鏡像 heartbeat,24h)+ `model_router` briefing→claude-haiku + startup_hooks。per-product progress/findings pending(carry-forward)。
- **(已 merged)PR-15.3** Eval 模式 /eval 同題並發三家 ✅ merged `89357c7`(#233)— `central/handlers/llm_eval.py`(admin-only,asyncio.gather return_exceptions,例外只印型別名,reasoning model max_tokens 1024〔OBS-JJ〕)+ obs-tracker OBS-JJ。
- **(已 merged)PR-15.2-hotfix** 修 gemini-pro slug 加 -preview ✅ merged `fcaf92e`(#232)— `gemini/gemini-3.1-pro` → `gemini/gemini-3.1-pro-preview`(恰一行;舊 slug 被 fallback 接住反證機制 work)。alias/budget key/router 全不變。
- **(已 merged)PR-15.2** LiteLLM Fallback 機制 ✅ merged `becdfac`(#231)— `router_settings`(num_retries/allowed_fails + 7 筆跨 provider fallbacks)。real-fire:fallbacks 載入正常(main-stable 吃),gemini-pro slug 問題由 hotfix 收。
- **(已 merged)PR-15.1b** Budget warning 80%/100% 主動推 TG ✅ merged `565c1d7`(#230)— 新 `central/budget_notifier.py`(薄接線層,複用 notifier.py 兩 helper)+ `litellm_client` 換掉 `TODO(Sprint 15)` log → `await _warning_notifier`(try/except,失敗不影響 LLM 回傳)+ `main.py` register。
- **(已 merged)PR-15.1a** /budget UI 多家化 + cap→$2000 ✅ merged `3917711`(#229)— 標題去 Anthropic→「Multi-AI API 用量」+ 「🧠 各供應商 / model 用量」by_model 分組區塊 + 去 stale 文案 + `MAX_CAP_USD` 1000→2000(README 兩處同步)。
- **(已 merged)PR-14.3** Multi-AI 路由初步 ✅ merged `fd74d34`(#227)— `model_router`(`TASK_MODEL` + `model_for`)+ 4 caller 換接(classify→gemini-flash-lite,spec/snapshot/memory→claude-sonnet);prompt 不動。
- **(已 merged)PR-14.2** LiteLLM 加 Gemini provider ✅ merged `7569f46`(#226)— config.yaml +`gemini-pro→gemini/gemini-3.1-pro` / `gemini-flash-lite→gemini/gemini-2.5-flash-lite`(`gemini/` 前綴 = AI Studio 路)+ budget PRICING 2 筆 + `_ALIAS_TO_PRICING_KEY` 2 筆 + `.env.example` `GEMINI_API_KEY=`。只註冊 provider;compose 0 改。
- **(已 merged)PR-14.1** LiteLLM 加 OpenAI provider ✅ merged `a22a6cc`(#225)— config.yaml +`gpt-flagship→openai/gpt-5.5` / `gpt-nano→openai/gpt-5.4-nano` + budget PRICING 2 筆 + `_ALIAS_TO_PRICING_KEY` 2 筆 + `.env.example` `OPENAI_API_KEY=`。只註冊 provider;compose 0 改。
- **(已 merged)PR-13.3b-2** H8a /app_build TG 指令 ✅ merged `9999960`(#223)— `/app_build`(is_admin,白名單,背景 task)→ clone + eas build → build URL 回 TG。
- **(已 merged)PR-13.3b-1** /app_build plumbing ✅ merged `f534287`(#222)— `eas_workspace.materialize_app_repo`(fresh clone,PAT 不進 argv)+ `run_eas_build`(docker exec eas --no-wait)+ central-bot eas-workspace mount。
- **(已 merged)PR-13.3a** H8a app-mode product registry ✅ merged `5fa2398`(#221)— `storage.create_product(kind=)` + `product_kind` helper + `/register_app`(登記既有 app repo,is_admin)+ list_products `[app]/[web]`。
- **(已 merged)PR-13.2** H8a EAS webhook → TG observe ✅ merged `a13b844`(#220)— `POST /webhook/eas` + `verify_expo_signature`(sha1/expo-signature)+ 推 notify_target_id(observe-only)。
- **(已 merged)PR-13.1** H8a EAS 執行容器地基 ✅ merged `86c150d`(#219)— `bot/eas/Dockerfile`(node:20 + eas-cli,非 root)+ compose `eas` service(`EXPO_TOKEN` 單注入,default network)+ runbook。
- **(已 merged)PR-12.4b** B1b triage hook ✅ merged `1ab0323`(#217)— triage 接進 `_report_failure`(單台)+ `_format_final_result`(Stage 2),命中 append「可能原因/建議」,fail-safe。
- **(已 merged)PR-12.4a** B1a triage 規則引擎 ✅ merged `3448b78`(#216)— `bot/src/services/triage.py` 6 條 `TRIAGE_RULES` + `triage()` 純函式。
- **(已 merged)PR-12.3b-hotfix-2** F2 等 repo 初始化 ✅ merged `37884c1`(#215)— `wait_for_repo_initialized`(poll get_ref,catch 404/409)gate,避 commit_files 撞 template 複製未完成 409。
- **(已 merged)PR-12.3b** F2-c 後半 /new_from_url TG 指令 ✅ merged `ddd9e25`(#213)— `/new_from_url <url> <name>`(admin,背景 task)→ create_project_from_url 真打建 repo。**F2 端到端打通**。
- **(已 merged)PR-12.3a** F2-c 前半 create_project_from_url orchestration ✅ merged `24eacb4`(#212)— 驗名→scrape→spec→bootstrap→webhook→**最後**註冊 product(中途失敗不留幽靈);return-based 結構回報。
- **(已 merged)PR-12.2b** F2-b 後半 建真 repo + bootstrap + rollback ✅ merged `311a804`(#211)— `repo_bootstrap.create_product_repo` + `bootstrap_repo`(建 repo → 逐檔 commit → 失敗 delete_repo rollback)+ `RepoBootstrapError`。薄建,client 注入。
- **(已 merged)PR-12.2a** F2-b 前半 [init] 寫檔 + bootstrap ✅ merged `d1eb1b8`(#210)— `commit_initial_file`([init] guard)+ 抽共用 `_put_contents`(snapshot 迴歸護住);`repo_bootstrap.pick_template` + `build_bootstrap_files`。
- **(已 merged)PR-12.1** F2-a site_to_spec 橋接 ✅ merged `7d662c4`(#209)— F1 report → A2 `assemble_spec` 產 NEXW spec(`report_to_idea_text` / `report_to_qa_pairs`〔D1 用 llm_summary〕/ `build_spec_from_report`)。純橋接。
- **(已 merged)PR-11.5a-hotfix** L2 product backfill + L1 render 收斂 ✅ merged `591d46d`(#206)— `backfill_l2_products.py` 補登 4 產品進 L2(idempotent);`context_export` render 收斂(L1 一行式、L2 排 sprint/pr 後 recent 前)。**§5 AC 由 TEEMO 在 server 跑**(rebuild `--no-cache` 後:python backfill → ls /data/products 列 4 產品 → render_context 含 koalo)。
- **(已 merged)PR-11.5a** G3-a 跨模型 context 注入 ✅ merged `09948e2`(#205)— `context_export.render_context()`(L1+L2 → block,secret 排除/fail-safe/~2000 護欄)+ `_inject_context`(`[NEXW CONTEXT]…[TASK]…`,三家共用,render 空退化純 prompt)。`write_job` signature 0 changes。
- **(已 merged)PR-11.4b** central ALLOWED_ENGINES 加 gemini ✅ merged `2489497`(#204)— `/agent gemini` 可用;`ALLOWED_ENGINES={claude,codex,gemini}`。**H5 三家 headless agent 端 + central 接線全通**。
- **(已 merged)PR-11.4a + hotfix** Gemini CLI 進 agent + runner gemini 分支 ✅ merged `faabef7`(#202)+ `--skip-trust` hotfix `748661d`(#203)— agent 多 `gemini` 引擎(`gemini -p --yolo --skip-trust`);Dockerfile 加 `@google/gemini-cli`,compose 加 `GEMINI_API_KEY`(不映射)。**不接 central、不碰真 repo(L0)**。
- **(已 merged)PR-11.3b** central /agent 選引擎 ✅ merged `451ba15`(#201)— `/agent [engine] <prompt>`(claude 預設 / codex 可選),`_parse_engine` + `ALLOWED_ENGINES`,job 寫 engine 欄。
- **(已 merged)PR-11.3a + hotfix** Codex CLI 進 agent + runner engine 分流 ✅ merged `0fb1142`(#199)+ runner 關 codex stdin hotfix `91567f3`(#200)— agent 依 `job.engine` 跑 claude / codex;Dockerfile 加 `@openai/codex`,compose 加 `OPENAI_API_KEY`,子行程映 `CODEX_API_KEY`;`runCli` 關 child.stdin。**不接 central、不碰真 repo(L0)**。
- **(已 merged)PR-11.2b-2 + hotfix** central 派工 /agent ✅ merged `b673b82`(#197)+ job 檔 0o644 hotfix `f5fb632`(#198)— `/agent <prompt>` → 寫 job → job_queue 背景 poll → 推回 TG;job 檔 world-readable 讓非 root agent 讀得到。
- **(已 merged)PR-11.2b-1** Agent Runner + File-Queue 協定 ✅ merged `aea6599`(#196)— agent 從 idle 變 runner(`runner.js`,node):poll `/jobs` two-file 佇列 → `claude -p` in scratch → 寫 `<id>.result.json`;timeout 300s / poll 3s;L0 只掛 `/jobs`+`/workspace`。
- **(已 merged)PR-11.2a** Agent Container 地基 ✅ merged `d4092d3`(#195)— `node:20` + claude CLI headless 地基(非 root,idle);補強:不吃整包 .env,只注入 `ANTHROPIC_API_KEY`。
- **(已 merged)PR-11.1-hotfix** A2 assemble 逾時修復 ✅ merged `fe8a744`(#193)— `_SPEC_TIMEOUT_S=120s` + log status_code。
- **(已 merged)chore** budget test-rot 修復 ✅ merged `707fd02`(#194)— 10 test-rot 加 `@freeze_time`,runtime 0 changes,central suite 10 fail → 0。
- **(已 merged)PR-11.1** A2 Idea-to-Spec Engine ✅ merged `09535ec`(#192)— `/expand` / `/spec` → Sonnet 多輪 → NEXW PR-spec → 存 L2 `_specs` + 貼回 TG。
- **(已 merged)PR-10.6** Sprint 10 close 儀式 ✅ merged `02e3442`(#191;純 docs)。
- **Sprint 10 ✅ Closed at 2026-06-04**(close report:[sprint-10-close-report.md](../sprints/sprint-10-close-report.md))— G2 Memory 核心(L1-L4 + 查找)+ A1 Idea Inbox + C2 Heartbeat 全 ship。
- **(已 merged)PR-10.5** C2 Heartbeat ✅ merged `407d7f8`(#190)— 定時(6h)讀 L1 → TG 主動推 `ADMIN_CHAT_IDS` + `/heartbeat_now`;加 `[job-queue]` extra;失敗隔離。
- **(已 merged)PR-10.4** A1 Idea Inbox ✅ merged `47be990`(#189)— `/idea` → Haiku 分 5 類 → conversation inbox(分類失敗→unclassified 照存;daily limit 10)。
- **(已 merged)PR-10.3** L1 焦點 + 查找引擎 ✅ merged `0005bf2`(#188)— memory 核心(L1-L4 + 查找)完成。
- **(已 merged)PR-10.2b** L2 設計欄 + 對話欄 + 四欄整合 ✅ merged `3960c52`(#187)— L2 per-product 四欄完成。
- **(已 merged)PR-10.2a** L2 impl/deploy 兩欄即時組合 + 查詢介面 ✅ merged `0db98b2`(#186)— impl(prs + 部署 commit proxy head)+ deploy(servers,whitelist secret-safe)。
- **(已 merged)PR-10.1-hotfix** central Dockerfile COPY memory_layer ✅ merged `88f7554`(#185)。
- **(已 merged)PR-10.1** Memory Layer L4/L3 地基 + 索引 ✅ merged `5cdf66b`(#184)— 新 `central/memory_layer/`(L4 真相來源定義 + 唯讀存取〔git/snapshot/runtime JSON,secret redaction〕;L3 docs 知識庫索引生成器)。唯讀、不動現有 bot 邏輯;L1/查找引擎留後續。
- **(已 merged)PR-D3-B5** D3 operational 收尾入帳 ✅ merged `1c52256`(#183;純 docs)— taoyuan 首個 image-mode 客戶部署 live + probe VM 下線。
- **(已 merged)PR-D3-B4** D3 收尾釘樁 + SSL 修復 docs + L28 ✅ merged `68339db`(#182;純 docs)。
- **(已 merged)PR-D3-B3** Stage 2 image-mode ✅ merged `358275f`(#181)— 每台 server `deploy_mode: git|image`(預設 git → 0 改變);`deploy_repo_image_mode`(client docker login→投遞 pull-compose→docker pull→up 無 build→health)+ image rollback coro + `run_command` 加 `input_text`(password stdin)+ `_STAGE2_META` stash。
- **D3 registry 自架部署進度:** registry.nexw.com 已上線。**push 側(B1)✅ merged `43d38db`**;**生成+投遞地基(B2)✅ merged `c860d9d`**;**Stage 2 image-mode(B3)✅ merged `358275f` + 真打 PASS**:拋棄式 client VM(alias probe-test / probe.nexw.com)經 `/onboard_server` 自動 onboard(bot 自裝 docker + web network + origin cert + Traefik)→ 設 `deploy_mode=image` + 暫時隔離 taoyuan → `/deploy taoyuan-union` Stage 2 只打 probe → **image-mode 全綠**(容器自 `registry.nexw.com/taoyuan-union:8152aa5` 起、nginx 正常,live taoyuan 未碰)。rollback live-fire 未測(壞 build 先死在 Stage 1 container-up 檢查、到不了 Stage 2 = 安全網;image rollback 信 B3 unit test)。測後全清理還原。**multi-image(koalo)= 待做**。
  - **首個 image-mode 客戶部署 ✅ 2026-06-03**:taoyuan-prod 翻 `deploy_mode=image` → `/deploy taoyuan-union`:Stage 1 主機 build `8152aa5`(無新 commit、+0 -0 零 diff = 純換機制不換內容)+ push registry → Stage 2 確認 → image-mode 部到 taoyuan-prod(首次部署 `prev_image_ref=None`,2s)→ `https://taoyuanunions.com` 獨立驗證正常服務(HTTPS + 內容)。**= NEXW 首個成功客戶 image-mode 部署,D3 payoff 兌現。** servers.json 現狀:`main`(git)+ `taoyuan-prod`(image)。
  - **probe 真打 VM 下線 ✅ 2026-06-03**:拋棄式 image-mode 真打 VM(probe-test / probe.nexw.com)下線三步完成 —— servers.json del `probe-test` entry(stop→edit→start)+ Cloudflare 清 `probe.nexw.com` DNS record + 刪 Linode VM。**剩 housekeeping(待清,不影響功能)**:① Cloudflare probe origin cert 撤銷;② registry `hello-world:test`(Block 1 standup 殘留)清理(registry:2 走 digest + GC)。
- **(已 merged)PR-B** GitHub org 搬遷文件 sweep ✅ merged `532a1b2`(#178;純 docs)
- **(已 merged)PR-A** GitHub org 搬遷 bot 雙 org / 雙 PAT ✅ merged `02d9f1c`(#177;server 真打綠)
- **(已 merged)PR-9.4** G2 Memory Layer 設計文件 part 1 ✅ merged `daafbe3`(#175;純 docs)— 新建 `memory-layer-design.md`:分層查找的持久記憶(L1 焦點→L4 真相逐層 fallback;設計/實作/部署/對話降為 L2 tag)+ schema + 遷移計畫;對齊 v2.0.md §15.9
- **(已 merged)PR-9.3** /onboard_server repos 參數 ✅ merged `5351c67`(#174;真打 PASS)
- **(已 merged)PR-9.2** deploy-key known_hosts step ✅ merged `fd1b24c`(#173;真打 PASS)
- **(已 merged)PR-9.1** deploy_repo clone-if-absent ✅ merged `b81473c`(#172;真打 PASS)
- **PR-8.2b-2 Stage 2 rollback 倒數接線 ✅ merged at 2026-06-01**(merge commit `827997b`,#166;flag 閘門預設 off)
- **PR-8.2b-1 Stage 2 rollback 地基 ✅ merged at 2026-06-01**(merge commit `2136594`,#165;`defer_rollback` 旗標 + SSH coro helper no-wire + PROD 20s 常數)
- **PR-8.4 chore server 訊息文案修正 ✅ merged at 2026-06-01**(merge commit `31c4e6c`,#164)
- **PR-8.3 deploy-UX 硬化 ✅ merged at 2026-06-01**(merge commit `416d0f2`,#163;`/deploy` guard 修空殼 pending_merge 卡死)
- **PR-8.2a H4 Stage 1 rollback + 共用倒數機制 ✅ merged at 2026-06-01**(merge commit `014dbe9`,#162;rework:倒數 context-free + 主路徑 `execute_deploy` 接倒數 + `deploy:redeploy`;`/update` 留 8.2a-2、Stage 2 留 8.2b)
- **PR-8.1b-3 H7a deploy key ✅ merged at 2026-05-29**(merge commit `033934e`,#161;`/add_deploy_key` cross-bot,私鑰留客戶 server + GitHub read-only。**H7a 全 5 PR 完成**;⚠️ 真打前 PAT 需加 Administration: R+W)
- **PR-8.1b-2 H7a wire CF + DNS + cert + Traefik ✅ merged at 2026-05-29**(merge commit `2c696fa`,#159;executor 一條龍長出 HTTPS,含兩個 heredoc 投遞 hotfix)
- **PR-8.1b-1 H7a Cloudflare 地基 + docs/19 re-charter ✅ merged at 2026-05-29**(merge commit `28dbe3f`;cloudflare_client unwired 地基)
- **PR-8.1a-2 H7a 真執行 executor ✅ merged at 2026-05-29**(merge commit `74cf783`;Docker+network+registry write role=production,不熱重載)
- **PR-8.1a-1 H7a 安全跨 bot pipe ✅ merged at 2026-05-29**(merge commit `060b72e`;零 mutation probe pipe)
- **PR-7.7 D4 多套環境變數機制 ✅ merged at 2026-05-29**(merge commit `7ed06bc`,PR #153;convention 地基層)
- **PR-7.6 H2 `/update` ✅ merged at 2026-05-29**(merge commit `6281468`,PR #152)
- **PR-7.5 D2 變數化 domain ✅ merged at 2026-05-28**(merge commit `a6a7fe1`)
- **H3 `/design_patch` 延 Sprint 11**(Q4 reality:product_bot dormant,需 Code Headless,scope 巨大)
- **D4 runtime「一鍵切 env / 部到客戶」延 Sprint 8 H7a**(reality:無客戶 production server、無 UAT infra;PR-7.7 只做 convention 地基層)
- **下一 PR candidates:** Sprint 12 F2(URL → F1 scraper → A2 spec → 自動建 repo + 初始 code:12.1 橋接 ✅shipping / 12.2 建 repo / 12.3 開 TG 指令)+ B1 Auto-Triage(部署/測試失敗自動 triage,Haiku 省費)
- v2.0 18 Sprint 範圍 14-16 個月(Sprint 4 ~ Sprint 21),累積 ship:8/18 Sprint(44%)+ Sprint 12 進行中

## 2. v1.0 既有功能(freeze 不動)

- central bot 12 commands(`/list` / `/deploy` / `/status` / `/logs` / `/pr` / `/watch` / `/unwatch` / `/watched` / `/servers` / `/test_server` / `/health` 等)
- product bot 七步引導 + template_replacer + CLAUDE.md generator + INITIAL_VERSIONS
- 兩段式部署(Stage 1 main → Stage 2 client production)
- Server Registry / Server-side SSH abstraction
- 真打驗證:demo2 七步引導 B-path 通過(2026-05-01,demo2 cleanup 已收尾)

## 2.5 v2.0 配套文件已 ship(規格書 §15 寫死的,不是 Sprint PR)

v2.0 規格書 §15 規劃的配套文件,在 Sprint 4 開工前已 ship 進 git,作為 Sprint 13 / 16 的前置:

- **PR #86** `prompts/app-project-rn-prompt.md`(2026-05-01,merge commit `8da97a3`)
  - 對應規格書 §15.13 Sprint 13 H8a App pipeline(RN + Expo + EAS)
  - Sprint 13 開工時 RN session 直接用此 prompt
- **PR #87** `prompts/app-project-flutter-prompt.md`(2026-05-01,merge commit `5dafa9f`)
  - 對應規格書 §15.16 Sprint 16 H8b App pipeline(Flutter + Shorebird)
  - Sprint 16 開工時 Flutter session 直接用此 prompt

⚠️ 這兩個 PR 是規格書 §15 規劃的配套文件 ship,不算 Sprint 4 PR(Sprint 4 主軸 = I1/J2/J1)。

## 3. v1.0 客戶端(freeze 期間繼續用)

- Koala House(frontend / backend),用 v1.0 bot 部署
- jy-official,用 v1.0 bot 部署

## 4. v2.0 規格書進度(章節 ship 狀態)

> ⚠️ **過時段(2026-05-01 規格書空殼階段內容)** — Sprint 4 開工後實作優先,
> §2-§14 placeholder 由各 Sprint PR 視需要回填,不是獨立 ship 路徑。本段
> 留作歷史追蹤,真實狀態以 [docs/sprints/v2.0-progress.md](../sprints/v2.0-progress.md) 為準。

- [x] §1 概覽 + 設計目標
- [ ] §2 15 條需求展開(原 14 條 + 條 15 Swagger)
- [ ] §3 八大能力 × 條對齊
- [ ] §4 整體架構
- [ ] §5 三級防護
- [ ] §6 Multi-AI 路由(主章節)
- [x] §6.5 成本控制設計(已 ship,Sprint 5 PR-5.1 起步註)
- [ ] §7 Secret 處理(Sprint 5 E1 + E2 涵蓋)
- [ ] §8 NEXW Footer URL
- [ ] §9 產品名變數化 domain
- [ ] §10 模仿網站架構
- [ ] §11 檔案結構標準化
- [ ] §12 跟 v1.0 整合
- [ ] §13 v3.0 候選池
- [ ] §14 風險清單
- [x] §15 Sprint 切分(§15.0 + §15.4-§15.21,18 Sprint 全 ship)

## 5. v2.0 18 Sprint 總覽

詳細進度看 [docs/sprints/v2.0-progress.md](../sprints/v2.0-progress.md)。

階段速查:
- 階段 1(Sprint 4-6)基礎建設 — M1-M3
- 階段 2(Sprint 7-9)業務 enabler 起步 — M4-M6
- 階段 3(Sprint 10-12)主動 agent 起步 — M7-M10
- 階段 4(Sprint 13-16)App pipeline + Multi-AI — M11-M13
- 階段 5(Sprint 17-21)加值收尾 — M14-M16

## 6. Sprint 6 階段紀錄(歷史 — 結構統一 + 防護升級)

> ⚠️ **歷史段(Sprint 6 規劃內容,已過時)** — Sprint 6 已 ✅ Closed at 2026-05-27。
> 本段留作歷史脈絡追蹤,**真實當前 Sprint / PR 狀態以本檔 footer + [v2.0-progress.md](../sprints/v2.0-progress.md) 為準**。


**主軸:** G1 檔案結構統一(含 `.env.example` 規範 + Swagger 規範)+ I2 branch lock(Multi-AI 前必做)+ B4 危險動作主動提醒
**估時:** 對齊規格書 §15.6,以 PR pace 跑
**對應月份:** M3
**狀態:** 開工中(2026-05-09 Governance Track Stage 1 closed,Sprint 6 恢復)
**Sprint 統合追蹤:** [docs/sprints/sprint-6.md](../sprints/sprint-6.md)(待建)

> Sprint 4 ✅ Closed at 2026-05-03(close report: [sprint-4-close-report.md](../sprints/sprint-4-close-report.md))
> Sprint 5 ✅ Closed at 2026-05-05(close report: [sprint-5-close-report.md](../sprints/sprint-5-close-report.md))
>
> 下方 Sprint 4 PR 清單留作歷史紀錄。

### Sprint 4 PR 清單(計畫中)

- [ ] PR-4.1 docs cleanup + snapshot.sh fix(本 PR)
  - 內容:current-state.md 重寫 + v2.0-progress.md 新建 + snapshot.sh 段 4 glob 防爆
- [x] PR-4.2 I1 verify_state.py(防 phantom state 檢查腳本)
- [x] PR-4.3 J2 docs/nexw-infra-conventions.md(infra 規範文件)
  - 含 central-bot Dockerfile healthcheck 評估(對齊 J2 規範後加)
- [x] PR-4.4a J1 self-deploy helper(純 helper,不啟用)
- [x] PR-4.4b J1 self-deploy TG 半自動
- [x] PR-4.4b.1 Hotfix:_git_pull docker.sock workaround(真打驗收 /repo:ro mount 衝突)
- [x] PR-4.4b.2 Hotfix:central-bot Dockerfile 加 docker CLI(真打驗收 image 缺 docker binary)
- [x] PR-4.4b.3 Hotfix:`_git_pull_via_docker` 加 `--entrypoint sh`(真打驗收 alpine/git ENTRYPOINT=git)
- [x] PR-4.4b.4 Hotfix:`_git_pull_via_docker` caller 傳 host path(真打驗收 container path 進 docker -v 自動建空 volume)
- [x] PR-4.4b.5 Hotfix:mount host `/root/.ssh:ro` 進 alpine/git container(本 PR,真打驗收 GitHub SSH auth 缺 known_hosts + 私鑰)
- [ ] PR-4.4c J1 self-deploy systemd 全自動(待後續)

### 下一步

Sprint 6 — 結構統一 + 防護升級(對齊規格書 §15.6)

主軸:
- G1 檔案結構統一(含 `.env.example` 規範 + Swagger 規範,順手解 KI-PR-5.4-B Layer 2)
- I2 branch lock(單一執行路徑,Multi-AI 前必做)
- B4 危險動作主動提醒(secret 出現 / 撤回 chat 自動 STOP)

**Sprint 6 ✅ Closed at 2026-05-27**(close report:[sprint-6-close-report.md](../sprints/sprint-6-close-report.md))— G1 8/10 主軸 + Swagger 規範 ship + close 階段 PR-A/B/C/D/E 100% 完成(2 個新 cross-cutting reference 檔 + 業務里程碑段升級 + close report)。整體 ~42 PR(26 主 repo + 10 Governance + 6 跨 repo)。Sprint 7 啟動加 Sprint 6 移交項:I2 / B4 / Gap 7 / PR-6.3-koalo-backend / Group C wording chore PR / cross-chat-workflow.md update / KI-PR-5.4-B Layer 2。

---

## 🤖 自動區參考

即時狀態請執行 `./server/snapshot.sh`,輸出含:
- Git branch / 最近 10 commits / working tree
- Open PRs(需 gh CLI,獨立 server 操作 SOP 安裝)
- docs/sprints / docs/handoffs / docs/automation 列表
- 當前 sprint preview head 30 行(若有)
- system_version.json + budget.json
- Docker container 狀態
- 本檔人工區內容(便於一條指令拿全狀態)

---

## Governance Track(獨立追蹤,跨 Sprint)

**起源**:2026-05-08 治理結構決策 handoff([檔](./2026-05-08-governance-decision-handoff.md))
**路徑**:Y(認真治理 + 認真改造 4 個既有 product)
**Stage**:1(治理基礎建設)— 進行中

### Stage 1 PR 清單 ✅ Closed at 2026-05-09(main HEAD `a51cada`)

完整 close report:[sprint-G-close-report.md](../sprints/sprint-G-close-report.md)

- [x] **PR-G.1** product-chat-bootstrap.md
  - 通用接手包模板
  - URL:https://github.com/Source-Solution/nexw-deploy-bot/pull/116
  - Branch:docs/pr-g.1-product-chat-bootstrap
- [x] **PR-G.2** cross-chat-workflow.md
  - 跨 chat 工作流規則 + 配對紀律 + 衝突解決 + fallback + 接手包用法
  - URL:https://github.com/Source-Solution/nexw-deploy-bot/pull/117
  - Branch:docs/pr-g.2-cross-chat-workflow
- [x] **PR-G.3** product-chat-instructions/koalo.md
  - URL:https://github.com/Source-Solution/nexw-deploy-bot/pull/119
  - Branch:docs/pr-g.3-product-chat-instructions-koalo
- [x] **PR-G.4** product-chat-instructions/jy-official.md
  - URL:https://github.com/Source-Solution/nexw-deploy-bot/pull/121
  - Branch:docs/pr-g.4-product-chat-instructions-jy-official
- [x] **PR-G.5** product-chat-instructions/{trinova,taoyuan}.md
  - URL:https://github.com/Source-Solution/nexw-deploy-bot/pull/123
  - Branch:docs/pr-g.5-product-chat-instructions-trinova-taoyuan

### Stage 1-4 全景

- Stage 1:治理基礎建設(本 chat,3-5 PR)— 進行中
- Stage 2:TEEMO 手動 setup product chat(各 chat 1 次)
- Stage 3:各 product chat 跑 sub-PR(4 sub-PR)
- Stage 4:本 chat 跑 audit PR(4 audit PR,nexw-deploy-bot)



---

## 7. 接手第一步(給新 session 的 Claude)

新 chat 開頭,Claude 會先:

1. web_fetch 讀 5 個 git URL:
   - docs/automation/v2.0.md
   - docs/automation/v2.0-changelog.md
   - docs/handoffs/current-state.md(本檔)
   - docs/sprints/v2.0-progress.md
   - docs/sprints/sprint-3.md(歷史)
2. 報告當前 Sprint + 當前 PR 階段給 TEEMO
3. 問 TEEMO 要不要 ssh server 跑 `./server/snapshot.sh` 確認 reality 對齊
4. 等 TEEMO 回應再進今天的議題

TEEMO 接手提示:
- 本 chat Project Instructions 已含 v2.0 全景速查(15 條訴求 + 18 Sprint 速查表)
- 不用對 Claude 重新解釋 v2.0 規劃
- 直接問當前 Sprint 任務即可

⚠️ 不憑 TEEMO 口頭描述進度,以 git reality 為準(NEXW cross-session verify rule)。

### 不要重新規劃已決策事項

- v2.0 規格書 §1 / §6.5 / §15 已 ship,不要懷疑
- 18 Sprint 範圍 + 順序已定稿(規格書 §15.0)
- Sprint 編號規則寫死(server Sprint = v2.0 §15 Sprint,強制同步)
- 30 天設計凍結原則已定(Sprint 21 後啟動)

---

## 8. Known Issues + Follow-up(動態)

### Sprint 12 OBS

- **F2 commit 競態兩修(PR-12.3b-hotfix + hotfix-2)**:① 逐檔 Contents API commit 撞連續寫同 branch 的 409 最終一致性 → 改 Git Data API `commit_files` 一次 atomic commit(讀 HEAD→tree→commit→update_ref,409 retry 一次)。② `generate_from_template` 後 template 內容**非同步複製**,`wait_for_repo_ready` 只確認 repo 存在沒等 ref → `commit_files` 太早撞 409(`Git Repository is empty`)→ 加 `wait_for_repo_initialized`(poll `get_ref`,catch 404/409 retry)在 `create_product_repo` 當 gate。`commit_initial_file` 保留為單檔 [init] 原語(暫無 caller)。

### Sprint 11 OBS(帶往後;close report 同步)

- **O1 — agent 網路隔離(L1 前必做)**:agent 在 default network,可達 litellm / central。**升 L1(agent 動真 repo)前**要先做網路隔離,避免自主 task 觸達內部服務。
- **O2 — sandbox-off 的 codex/gemini 可見整個 /jobs 佇列**:`--dangerously-bypass-sandbox` / `--yolo` 下,task 可讀 `/jobs` 全部 job/result。L1 前處理(per-job 隔離目錄 / 收斂掛載)。
- **O3 — /jobs 孤兒檔無自動清掃**:`.job.running`(runner crash 殘留)+ 逾時後遲到的 `.result.json` 無 sweeper。目前靠 central 360s timeout + 手動清。未來加掃描器。
- **O4 — relevance-query 延後**:G3-b 縮編只做 A2 接線;context block 現在小(4 產品 + L1 收斂),relevance-query 屬過早優化。記憶變大 / L3 doc body 進來再做(Sprint 14-15 一帶)。
- **O5 — A2 對語意模糊 idea 仍可能走 generic**:注入的 context 不覆蓋使用者 steering;模糊 idea 仍可能產 generic spec。A2 品質 refine 候選,**非缺陷**。
- **O6 — litellm_client.py 30s 全域 timeout 偏緊**:A2 assemble 已用 120s 繞過,但全域預設仍 30s。Sprint 15 一併收。
- **O7 — context_export 一行式 recent 截斷會切到字中**(cosmetic,低優先)。
- **O8 — /expand <idea_id> 需要人看得懂的 idea 清單入口**:uuid 手打不可用,需列 inbox idea 的指令/UI。

### ✅ Resolved (PR-A, merge `02d9f1c` @ 2026-06-02):GitHub org 搬遷 — bot 雙 org / 雙 PAT 接線

6 個 repo 從個人帳號 `Source-Solution`(待全驗證後刪除)搬到兩個 org:5 個產品 →
**nexwproject**(primary),bot 自己 `nexw-deploy-bot` → **xxnexw**(self-repo)。
主 server git 身分換成 GitHub 帳號 **nexwtech**(兩 org owner)。

- **PR-A code ✅ merged(`02d9f1c`)+ server 真打綠:** owner resolver 雙 org 化
  (`config.github_full_repo` 接受 nexwproject + xxnexw);central
  `GitHubClient.for_owner()` 依 owner 選 PAT(api-token.txt = nexwproject 主、
  api-token-xxnexw.txt = xxnexw self,無 fallback);snapshot_generator + verify_state
  走 xxnexw token;所有功能性 `Source-Solution` 字串改對;bot `GITHUB_PAT`=nexwproject。
  server rebuild + 雙 token 已塞、`GITHUB_OWNER=nexwproject` 已設;
  smoke 驗過:`/test_github`(authenticated as nexwtech / nexwproject)+ `/snapshot`
  (走 `for_owner(xxnexw)`,main HEAD `f2ad52c` 即該 self-snapshot)。
- **PR-B(本 PR)✅:** 操作 / 前瞻文件 sweep(`Source-Solution` → nexwproject 產品 /
  xxnexw 自身)+ 接手 SOP raw URL 改對 + v2.0-changelog 記一條;歷史文件 / PR permalink
  維持原樣(GitHub 對搬遷 repo 自動轉址)。
- **`Source-Solution` 刪除:** 待 PR-B merge + 全 docs 驗證後才刪(TEEMO 端另需手動改
  Claude.ai Project 的 live Project Instructions,非 repo 內)。

### ✅ Resolved (PR-8.5):直接 `/deploy <repo>` 靜默只部 staging(L22 類缺口)

`/deploy <repo>` 在 deploy queue 空時走 `execute_deploy`(single-confirm 路徑),
該路徑**完全沒接 Stage 2** → Stage 1(staging)成功後不跳 `[推全部 production]`
prompt,直接部署的 repo 永遠上不了 production(8.2b 真打發現 rollbacktest 不跳
Stage 2)。`send_stage2_prompt` 原本只掛在 queue 路徑(`execute_queue_deploy`)+
PR-merge 路徑(`pmg:deploy_now`),漏了 single-confirm 主路徑(同 8.2a 踩過的 L22
「主路徑要 runtime 驗證」)。修:execute_deploy 加 `context=None` opt-in,成功尾端
接 send_stage2_prompt;只 single `/deploy` callsite 傳 context,其餘不傳 = 0 行為改動。

### ✅ Resolved (PR-8.3):stale/空殼 pending_merge group 擋死 `/deploy`

`/deploy` guard 原本只看 `has_active_notification()`,連 `pending_prs` 空的 stale
group(PR 被 direct-push / 外部 merge 掉、通知沒清乾淨)都擋 → 卡死(本 session
卡 4 次,每次手動清 JSON 才能動)。修:guard 改為「只在真有待部署 PR(`pending_prs`
非空)時擋」+ 對空殼 group `set_notification(None)` 清掉 stale notification 後放行。
direct push 本身不建 pending_merge(recon 校正);未加 direct-push 部署 button(保留
直推需刻意 `/deploy` 的防呆)。

### ✅ Resolved (PR-8.4):server 訊息文案修正

- **`/delete_server` 幽靈指令**:`server_registry_writer.py` alias 重複錯誤訊息原引用不存在的 `/delete_server` → 改指向真實移除路徑「手動編輯 `/srv/bot-state/servers.json` 刪 block + 重啟 bot(見 docs/14 解約/下線流程)」。
- **onboard 完成報告 + 前置說明 stale Traefik 文案**:8.1b 已併入 onboard 一條龍(自動裝 Traefik + 簽 Cloudflare Origin 憑證),但完成報告(:262)仍寫「Traefik/憑證未裝」、usage(:58)/ confirm(:119)仍寫「不碰/留 8.1b」→ 全改成「自動裝好 + HTTPS 就緒」。

### Open follow-up(本 session 累積,另開 PR)

- **2026-06-09 scope 決定(PR-20.0)**:Sprint 18/19(PPT C3-C6 + footer D1)**skip → v3.0 候選池**(v2.0.md §13);條 4(四種 PPT)+ 條 5(footer)移出 v2.0。Roadmap 下一站 = **Sprint 20**(D3 換產品名背景換 + B3 Proactive Suggestions + S17 延後的 B2-c Watch Mode 半主動 hook 落地)。v2.0 active sprints 18 → 16。
- **L22(重申)**:deploy 路徑要 runtime 驗證,不能只讀 code 認(8.2a recon 曾誤判主路徑)。

### Sprint 9 close 移交 follow-up(2026-06-02,→ Sprint 10 / backlog)

- **H7b 收尾**:首次部署鏈三塊已通(clone-if-absent + known_hosts + onboard repos),但 docs/14 完整 13 步逐項對應 + `/test_server` 整合測試指令尚未做 → Sprint 10 / backlog。
- **split-repo(koalo backend+frontend)首次 clone**:塊 1 clone-if-absent 只處理單 repo,split-repo 首次 clone 不在 scope → backlog。
- **Gap 7 docker-compose 頂層 `name:` 跨 product 補齊**(Sprint 7→8→9 三度 re-route):H7b 真打聚焦首次部署鏈、未碰任何 product compose → backlog(併 Sprint 20 訴求 7「換產品名背景全變數一起換」,屬 G1 尾巴)。**koalo-fe / jy-official 頂層 compose `name:` 命名一致性**併此項一起收。
- **`/remove_server` 解約指令缺**:onboard 有了(`/onboard_server`),但移除 server 仍得手動編 `/srv/bot-state/servers.json` 刪 block + 重啟(PR-8.4 已把錯誤訊息改指此手動路徑)→ backlog(對齊 L27「別讓手動編 registry JSON 當必經步驟」)。

### 2026-06-01:PR-8.2a-2 `/update` 接倒數 ✅ — H4 全路徑覆蓋完成

- **PR-8.2a-2 done:** `/update` 的 inline build 路徑接共用倒數機制(checkout 前抓 `before_sha` → build/up 失敗 60s 倒數 → `git reset --hard` + rebuild);fetch/checkout 失敗不倒數;cancel 走共用 `rbk:cancel`(不另造)。forbidden scope(rollback_countdown / execute_deploy / two_stage / deploy_repo / ssh_client)0 changes。tests +7 + 全 bot suite 211。
- **H4 完整:** staging 倒數覆蓋 execute_deploy 六入口 + `/update` + Stage 2;H4 觸發面再無缺口。剩 F1/F2 收尾訊息 polish(見下)。

### 2026-06-01:H7a + H4 真打 ✅ 後續(PR-8.6 reality-sync 入帳)

- **KI(F1/F2) ✅ Resolved at PR-8.2b-3:** Stage 2 deferred(flag on)收尾訊息瑕疵。**F1**(aggregate summary 發兩次):recon 確認現行 main **已是單發** —— `_execute_stage2` 每按鈕只呼叫一次、`_format_final_result` 唯一 callsite、倒數解決只 edit per-server 載體訊息(無回呼 stage2),非真重複(真打疑為把「🏁 指標 + 📊 明細」兩段式設計看成兩份);**不改 code,加回歸鎖 test**。**F2**(rollback/cancel 後 summary 寫「倒數 rollback 處理中」stale):deferred 分支文案改 timeless 指標「🔙 倒數 rollback:最終結果見上方該 server 專屬訊息」,defer-time / 解決後 re-read 都不 stale。**flag off 路徑逐字不動。**
- **KI(clone-if-absent / finding #3)✅ Resolved at PR-9.1**(merged `b81473c` #172,真打 PASS):`deploy_repo` Step 1 加 clone-if-absent(`test -d .git` ABSENT → `git clone git@github-<repo>:<owner>/<repo>.git` deploy-key alias → 重抓 HEAD → 續部署;SSH-fail / PRESENT-broken 不 clone)。單 repo only;split-repo(koalo)首次 clone 留 follow-up。
- **KI(首次自動 clone host key gap)✅ Resolved at PR-9.2**(merged `fd1b24c` #173,真打 PASS):`/add_deploy_key` 沒填 github.com 進客戶 server known_hosts → bot 非互動自動 clone `Host key verification failed`(exit 128)。deploy_key_executor 加 `knownhosts` step(`ssh-keyscan github.com`,idempotent skip-if-exists,fail-fast)預填 host key。
- **KI(onboard deploys gap)✅ Resolved at PR-9.3**(merged `5351c67` #174,真打 PASS):`/onboard_server` block `deploys=[]` → `/deploy` 找不到目標只跑 Stage 1。PR-9.3 加選填第 5 參數 repos(逗號分隔)→ onboard 當下寫進 deploys → 一鍵接完整(接機器 + 認領 repo 一次到位)。
- **G2 Memory Layer 設計 ✅ Resolved at PR-9.4**(merged `daafbe3` #175,part 1 = 只設計):新建 [`memory-layer-design.md`](../automation/memory-layer-design.md) —— 分層查找的持久記憶(L1 焦點 → L4 真相逐層 fallback;設計/實作/部署/對話降為 L2 tag)+ 各層 schema + 遷移計畫(L3/L4 不搬家,真正建 L2 聚合 + L1 升級 + 查找引擎,全在 Sprint 10 實作)。
- **flag 決策(B)記錄:** `STAGE2_ROLLBACK_COUNTDOWN_ENABLED` 真打驗證 ✅,現歸 0;taoyuan 維持 immediate rollback,待 F1/F2 polish 後評估開 flag。
- **KI(current-state.md 內部 stale sprint 指標)✅ Resolved at PR-8.7:** §6 標題改「Sprint 6 階段紀錄(歷史)」+ 過時 banner;§8 內嵌「當前 Sprint: Sprint 6 / 當前 PR: PR-5.6」改標「(當時)」+ 歷史快照 banner;§4 過時段本就有 banner(指向 v2.0-progress.md)。所有 live 指標(§1 頂部 + footer)統一 Sprint 9。consolidation 於 Sprint 8 close 一次收齊。

### 2026-06-01:主 server VM 誤刪 → backup 還原(IP 變更)

主 server VM 被誤刪,從 backup / snapshot 還原成新 instance,disk 資料完整(零損失),
對外 IP 變更(舊 172.104.98.216 → 新 172.237.7.201)。

還原後處理:
- Cloudflare DNS A record 改指新 IP(灰雲不變)→ 站台恢復上線
- 所有 container 自動重啟正常(restart policy)
- Let's Encrypt 憑證隨 DNS 自動續期(acme.json 在還原的 disk 內)
- SSH host key 不變(同一份 disk),Termius 連線無痛

殘留 follow-up(另開 koalo PR 修):
- koalo-typesense healthcheck 假性 unhealthy —— image 內無 curl,healthcheck 指令
  `curl ...` 一直 not found(FailingStreak 累積);typesense 本體正常(Raft committed、
  Peer refresh succeeded)。修法待定:拿掉 healthcheck 或改用 image 內有的工具。

### H7a onboarding(Sprint 8)follow-up

- **8.1a fresh-install 已實證(freshvm)**:乾淨 VM 上 Docker 28.5.2 install path 真打過。
- **testvm / freshvm 兩 test block 待清(重啟前)**:8.1a 真打在 servers.json 留了測試用 block;bot 重啟前先清掉(`role=production` 測試殘留,避免進記憶體 registry)。

### Reality Audit Reference

任何動 product / infra 的 PR 開工前必讀:
[`docs/automation/products-reality.md`](../automation/products-reality.md)

5 個 server-running unit 完整 reality(koalo / jy-official / trinova-center / taoyuan-union / nexw-deploy-bot)。

Last audit: 2026-05-07(PR-6.0)

---

### 8.1 Cleanup 殘留 ✅ RESOLVED by Sprint 17 J4-a/b(2026-06-08~09)

**`/delete_product` cleanup 不完整**(2026-04-30 PR-3.10 smoke 階段發現)— ✅ **RESOLVED**

> **✅ 關閉(Sprint 17 J4):** staging 足跡(compose / 目錄 / server_deployments)現由 `teardown_product_staging`(J4-a `05c3c0d`)+ bot teardown poller(J4-b-1 `ea02ae5`)+ `/delete_product` 接線(J4-b-2 `67d57b1`)**自動清除**。客戶端(role=production live)→ detect-only + 手動 checklist(decision b),不自動刪。下方原 4 處逐項收斂:

`/delete_product <name>` 原本只砍 `/srv/bot-state/products/<name>/` state 跟 GitHub repos,曾**沒砍以下 4 處**(現況逐項標註):

1. ~~`bot/docker-compose.yml` 的 `<name>-bot` service block~~ → **OBSOLETE**:現代產品(`/new_project` / `/new_from_url` / `/register_app`)**不再建 per-product bot service**,故無此殘留;歷史 demo2 已清(見下方 patch 紀錄)。
2. ~~`bot/.env` 的 `<NAME>_BOT_TOKEN=` 那行~~ → **OBSOLETE**:同上,現代產品無 per-product bot token。
3. ~~`/srv/bot-state/spawn_quarantine/` 殘留的 queue file~~ → **OBSOLETE**:spawn_quarantine 屬 per-product bot spawn 機制,現代產品不觸發。
4. ~~cron 不會 reap 對應 container~~ → **RESOLVED**:`teardown_product_staging` 的 `compose_down` 步驟直接停 + 移除 staging container,不依賴 cron reap。

**手動 patch unblock 流程**(以 demo 為例):
- `docker stop nexw-demo-bot && docker rm nexw-demo-bot 2>/dev/null`
- `git checkout bot/docker-compose.yml`(若 service block 是 spawn-worker auto-generated 留下的)
  - 或 `sed -i` 砍 docker-compose.yml `demo-bot` block(若已 commit)
- `sed -i.bak '/^DEMO_BOT_TOKEN=/d' bot/.env && shred -u bot/.env.bak`
- `rm /srv/bot-state/spawn_quarantine/demo.*.json 2>/dev/null`

**何時修:** ✅ **已修(Sprint 17 J4-a/b,2026-06-08~09)** — v2.0 §12「跟 v1.0 既有 code 整合」評估後,J4「`/delete_product` cleanup 完整化」拆 a(primitive)/ b-1(poller)/ b-2(接線)/ c(docs 收尾)完成。

**已執行 patch 紀錄:**
- 2026-05-01:demo2 cleanup(docker-compose.yml service block + bot/.env DEMO2_BOT_TOKEN + .env.bak shred)— Sprint 3 真打測試殘留處理完畢

**KI-rdot-backend-unlinked ✅ RESOLVED by PR-20.1 `6a9dd0c`(#249)(repos 記錄層;2026-06-09 發現於 PR-17.6 J4-c audit)**

`rdot` product 記錄的 `repos` 原只含 app repo `rdot-rn`,backend(`rdot-backend` + `rdot-postgres` @ `rdot-api.nexw.com`,server path `/srv/projects/rdot-backend`)未連入記錄。

- **影響(已解):** `/delete_product rdot` 原會用 `repos=[rdot-rn]` → teardown 清不到 backend 足跡。
- **解法:** PR-20.1 `6a9dd0c`(#249)一次性 backfill script `central/backfill_rdot_backend.py`(idempotent)把 `rdot-backend`(`{type:backend,name,url}`)append 進 rdot `repos`。merge 後 server 跑 `docker compose exec central-bot python backfill_rdot_backend.py` 落地。`/delete_product` 讀 `repos[].name`,append 後自動涵蓋 backend。
- **systemic 治本:** `/register_app` 只收 app repo、不收 backend → 凡 app+backend 產品都會出現此 repos gap(見 obs-tracker OBS-LL)。暫不排程。
- **詳見:** [products-reality.md rdot entry](../automation/products-reality.md)。

**KI-rdot-backend-teardown-orphan ✅ RESOLVED by PR-20.5(2026-06-09 PR-20.1 merge 後 server recon 發現)**

server recon 結論:rdot-backend 實際在 **`/srv/projects/rdot-backend`**(平行目錄、repo-named)、**手動部署**、**`server_deployments.json` 無紀錄**。`teardown_product_staging` 原以單一 **`/srv/projects/<product>`** 目錄為模型(rdot 無此目錄)→ `/delete_product rdot` 會留下 **rdot-backend + rdot-postgres container + 目錄孤兒**。

- **根因:** teardown 未用 `resolve_project_path` 逐 repo 解 footprint(只認 `/srv/projects/<product>` 單目錄模型)。
- **解法:** PR-20.5 改 `teardown_product_staging` step 2/3 — 額外**逐 repo `resolve_project_path` 解實體目錄併進** compose_down + rmtree 集合(deepest-first + is_dir 守衛);sibling repo-named dir(rdot-backend)現會被停 + 清。repos 記錄層(PR-20.1)+ 執行層(PR-20.5)雙補齊 → `/delete_product rdot` 不再孤兒化 backend。app repo(無 compose)resolve None 跳過,不誤刪。結構上仍只碰本機 staging。
- **驗證:** merge 後 server 真打(拋棄式 sibling 容器仿 rdot 佈局)— 見 PR-20.5 runbook。
- ~~短期手動 patch~~ → 不再需要(teardown 自動 reap sibling)。

### 8.2 Tooling 改善

**KI-update-detached-head(NEW,MED,2026-05-29 Sprint 7 close 教訓 7)**

`/update <commit>`(PR-7.6 H2)checkout 到 commit SHA 後,repo 進 **detached HEAD** 狀態。之後對同 repo 跑 `/deploy`(內部 `git pull --ff-only`)會失敗(detached HEAD 無 upstream)。

- **Workaround:** `/update <branch>`(用 branch 名非 commit)切回 attached;或 server 手動 `git checkout <branch>`
- **Follow-up:** 評估 `/deploy` 前自動偵測 detached HEAD + 提示切 branch,或 `/update` 後提示「現在 detached,要 `/deploy` 請先 `/update <branch>`」。留 Sprint 8+ 與部署紀律一起處理

**KI-server-checkout-stale-branch(NEW,MED,2026-05-29 Sprint 7 close 教訓 12)**

deploy-bot 的 `/srv/projects/nexw-deploy-bot` checkout 曾停在 PR-7.6 feature branch(`§0 Sandbox Sync` 只同步 Claude Code sandbox,**不同步 server 上的 git checkout**)。L18 rebuild 前若 server checkout 不在 main,會 build 到舊/錯 code。

- **教訓:** rebuild / L18 smoke 前先確認 server checkout 在 main(`cd /srv/projects/nexw-deploy-bot && git branch --show-current`)
- **Follow-up:** 評估 snapshot.sh / verify_state.py 加「server checkout branch ≠ main」告警。留 Sprint 8+ 一起處理

**KI-venv-tmp-reboot(NEW,LOW,2026-05-28 PR-7.1 真打發現)**

`/tmp/test-venv` 在 server reboot 後遺失(2026-05-13 reboot 遇一次,PR-7.1 pytest 真打又遇一次,需手動重建)。`docs/09-troubleshooting.md:711` venv pattern 用 `/tmp/`,reboot 不持久。

**觸發 case:** PR-7.1 pytest 真打 — TEEMO 跑 `/tmp/test-venv/bin/python -m pytest` 發現 venv 遺失,手動 `python3 -m venv /tmp/test-venv && pip install -r central/requirements-dev.txt` 重建後 32 tests pass。

**Follow-up 評估:**
- 換持久位置(`/srv/test-venv`)— 不受 tmpfs / reboot 影響
- 加開機自動重建 script(systemd unit 啟動時驗證 venv 存在)
- 留 Sprint 8+ 與其他 infra 紀律一起處理

---

**snapshot.sh 段 4 glob 防爆 bug**(2026-05-01 接手測試發現,本 PR 修)

`server/snapshot.sh` 段 4「Current Sprint Preview」glob 失敗導致 set -euo pipefail 提前 exit,本 PR 改用 find 修復。

---

**verify_state.py reality verifier**(2026-05-01 PR-4.2 ship)

`server/verify_state.py` 5 條檢查(git_main_stale / working_tree_clean / open_prs / docker_containers / budget_month_stale),可手動 / cron 跑驗證 reality 一致性,防 cross-session phantom state。CLI: `./server/verify_state.py [--format json] [--strict]`,exit 0/1/2 分級。

---

**verify_state.py open_prs check 邏輯瑕疵**(2026-05-01 真打發現,本 PR 接受 bug 留 follow-up)

- 對 squash/rebase merge PR 誤判 open
- 影響 open_prs 列表內容不可信,其他 4 check 正常
- **何時修:** Sprint 5 H1 LiteLLM + Claude API 接入後改用 GitHub API
- 詳見 v2.0-progress.md Sprint 4 校正紀錄

---

**gh CLI 沒裝**(從 Sprint 3 candidate 推延)

`server/snapshot.sh` 段 2「Open PRs」依賴 `gh` 命令,主 server 沒裝。

**何時修:** 獨立 server 操作 SOP(不進 PR,TEEMO ssh 自己裝)。

---

**central-bot 沒 healthy 標記**(2026-05-01 觀察)

`docker ps` 顯示 nexw-central-bot 只有 `Up X hours`,沒 `(healthy)`。對照 deploy-bot 有 `(healthy)`。

**判讀:** central-bot Dockerfile 可能沒設 healthcheck。對照 docs/20 規則「後端可選」,沒設合規但建議設。

**何時修:** Sprint 4 PR-4.3(J2 nexw-infra-conventions.md)時對齊規範一起處理。

✅ **PR-4.3 已釐清(2026-05-02)**:reality 為「故意不設」,對齊 docs/20「polling-only bot 不設 healthcheck」規則。J2 §2.3 摘要納入此規則,follow-up 收掉。

---

**docs/automation/nexw-infra-conventions.md**(2026-05-02 PR-4.3 ship,framework-agnostic NEXW infra reference)

- 給客戶 C2 場景(60-80% 接非 NEXW 寫的 repo)改造對齊 NEXW infra 慣例用
- Single source of truth:衝突時以 docs/19 / docs/20 / docs/13 為準(本檔只摘要 + checklist + 例子)
- Sprint 6 G1 / Sprint 7 D2 ship 時擴展(.env 命名規範 + Swagger 規範 + API_DOMAIN 變數)

---

**bot/src/services/self_deploy.py**(2026-05-02 PR-4.4a ship,J1 self-deploy helper)

- 純 helper 邏輯,**no-wire**(沒人 call,0 production impact)
- 提供 `build_plan()` + `execute_self_deploy()` 兩個 entry point
- Q1=C 切 3 sub-PR 第一個:helper / 4.4b TG 半自動 / 4.4c systemd 全自動
- 詳見 docs/21-self-deploy-mechanism.md

---

**central /self_deploy TG 半自動 trigger**(2026-05-02 PR-4.4b ship,J1 sub-PR 2/3)

- TG 指令 `/self_deploy [service]` → ConversationHandler + 4 顆按鈕(Plan / Dry-run / Execute / Cancel)
- 透過 bridge 模式 reuse `bot/src/services/self_deploy.py`(走 `/repo:ro` mount + sys.path)
- Paradox 防護:`/data/central/last_self_deploy.json` + `application.post_init` 補推完成通知
- ⚠️ SECURITY: central-bot 加 `docker.sock` mount(root-equiv access),用 `ADMIN_TG_USER_IDS` 白名單(`auth.py`)hard-gate
- 7 commits / 16 檔 scope / 24 unit tests passed
- 設計題 Q0=A / Q1=A / Q2=B / Q3=B / Q4=B / Q5=B / Q6=A / Q7=A
- 7 個 drift 校正(D1-D7,詳見 v2.0-progress.md PR-4.4b entry)

---

**PR-4.4b.1 hotfix:_git_pull via docker.sock**(2026-05-02 PR-4.4b.1,真打驗收 bug 修)

- **bug**:`/self_deploy bot` 真打跑炸 `git pull failed: cannot open '.git/FETCH_HEAD': Read-only file system`(`/repo:ro` mount 拒寫)
- **修法**:`_git_pull` → `_git_pull_via_docker`,走 `docker run --rm -v <path>:/repo:rw alpine/git:latest sh -c "rev-parse → pull → rev-parse"` 1-shot chain
- **設計校正**:Q3 B → B'(維持 auto-pull UX,避免 SSH 退步)
- **Production prereq**:TEEMO 主 server 跑 `docker pull alpine/git:latest`(~25MB)
- 12 handler tests 全綠 / 387 non-budget regression passed

---

**PR-4.4b.2 hotfix:central-bot Dockerfile 加 docker CLI**(2026-05-02 PR-4.4b.2,真打驗收 bug 修)

- **bug**:PR-4.4b.1 ship 後,`/self_deploy bot` 改報 `docker command not found`(central-bot image 沒 docker CLI binary,只有 docker.sock mount 沒用)
- **影響**:不只 PR-4.4b.1,**PR-4.4a `execute_self_deploy()`** 用的 `docker tag` / `docker compose up` / `docker ps` 全會炸 — 整個 J1 機制本來都不能用
- **修法**:`central/Dockerfile` line 25 `apt-get install git` → `git docker.io`
- **設計選擇**:debian default repo `docker.io`(~150MB)勝於 docker-ce-cli(~60MB,加 apt source)/ static binary(維護成本)
- **Image size**:218MB → ~370MB(NEXW 1 主 server 規模可接受)
- **Production prereq**:TEEMO 主 server `docker compose build --no-cache central-bot` 後驗 `docker run --rm nexw-central-bot:latest which docker`
- 12 handler tests 全綠 / 387 non-budget regression passed(test mock subprocess,不依賴 image 內 docker CLI)

---

**PR-4.4b.3 hotfix:`_git_pull_via_docker` 加 `--entrypoint sh`**(2026-05-02 PR-4.4b.3,真打驗收 bug 修)

- **bug**:PR-4.4b.2 ship 後,`/self_deploy bot` 改報 `git: 'sh' is not a git command`
- **root cause**:`alpine/git:latest` ENTRYPOINT 寫死為 `["git"]`,`docker run alpine/git sh -c "..."` 實際變成 `git sh -c "..."`(git 把 `sh` 當 subcommand)
- **修法**:`central/handlers/self_deploy.py` 內 `_git_pull_via_docker` 的 `docker run` args 加 `"--entrypoint", "sh"` 覆蓋 entrypoint,並從 args 移除冗餘的 `"sh"`(只留 `"-c", sh_command`)
- **為什麼之前沒抓到**:test 用 `patch.object(_git_pull_via_docker, ...)` mock 整個函數,沒驗 alpine/git 真實 entrypoint。Sprint 5 spec 模板新增 Recon 必驗:用了 `--entrypoint` 鎖死的 image 必跑 `docker inspect <img> --format='{{.Config.Entrypoint}}'`
- **Production prereq**:TEEMO 主 server `docker compose build --no-cache central-bot` 後真打 `/self_deploy bot`(這次包含真打 Execute)
- 12 handler tests 全綠(0 regression,Recon 確認 test 不 assert call_args list)

---

**PR-4.4b.4 hotfix:`_git_pull_via_docker` caller 傳 host path**(2026-05-02 PR-4.4b.4,真打驗收 bug 修)

- **bug**:PR-4.4b.3 ship 後,`/self_deploy bot` 改報 `fatal: not a git repository (or any parent up to mount point /)`
- **root cause**:caller 傳 `_PROJECT_PATH = Path("/repo")`(container path)進 `_git_pull_via_docker`,函數內 `docker run -v /repo:/repo:rw` 對 host docker daemon 來說,host 上沒 `/repo` → docker 自動建空 named volume 掛進 ephemeral container → no .git → fatal
- **修法**:加常數 `_HOST_PROJECT_PATH = Path("/srv/projects/nexw-deploy-bot")`,caller 改傳此常數;其他 3 個 `_PROJECT_PATH` caller(`build_plan` / `git diff` cwd / `execute_self_deploy`)不動(在 central-bot container 內跑,/repo 是對的)
- **為什麼之前沒抓到**:PR-4.4b.1 spec §5.2 範本本來寫 host path 預設,ship 時對齊既有 `_PROJECT_PATH = Path("/repo")` silent 退成 container path;test mock 整個 `_git_pull_via_docker`,不驗 caller 傳什麼。Sprint 5 spec 模板新增 Recon 必驗:任何 path 用作 `docker -v` 必驗 host path,常數命名須帶 `HOST_` prefix
- **Production prereq**:TEEMO 主 server `docker compose build --no-cache central-bot` 後真打 `/self_deploy bot` Step 4 Execute
- 12 handler tests 全綠(0 regression,Case A 確認)
- ⭐ 累積 4 個 hotfix 後預期 Sprint 4 J1 end-to-end work

---

**PR-4.4b.5 hotfix:mount host `/root/.ssh:ro` 進 alpine/git container**(2026-05-03 PR-4.4b.5,真打驗收 bug 修,reality miss #9)

- **bug**:PR-4.4b.4 ship 後,`/self_deploy bot` 真打 Step 4 改報 `Host key verification failed` / `fatal: Could not read from remote repository`
- **root cause**:ephemeral alpine/git container 內 `/root/.ssh/` 是空的 — 沒 known_hosts(host key 驗證失敗)+ 沒 SSH 私鑰(即使過了 host key check 也沒 auth credentials)。git remote 是 SSH protocol(`git@github.com:...`)所以一定要 GitHub auth
- **設計拍板**:Option A — mount host `/root/.ssh:ro` 進 container,reuse 既有 GitHub SSH 設定(github_personal 私鑰 + known_hosts)。對比 Option B HTTPS PAT(要管 token + 改 git remote)/ Option C ssh-keyscan(只解 known_hosts 不解 auth)/ Option D 重設計,Option A scope 最小(1 行 docker run args),read-only 不能寫壞 host,single-admin scope + 可信 alpine/git 官方 image,risk 可控
- **修法**:加常數 `_HOST_SSH_PATH = Path("/root/.ssh")`,docker run args 加 `-v {_HOST_SSH_PATH}:/root/.ssh:ro`(在 host repo mount 之後)
- **為什麼之前沒抓到**:test mock 整個 `_git_pull_via_docker` 不驗 args list;PR-4.4b.1~4 真打都還沒到 SSH auth 階段就先炸(Read-only mount / docker CLI 缺 / entrypoint / container path)。Sprint 5 spec 模板新增 Recon 必驗:用 ephemeral container 跑 `git pull` over SSH 必驗 SSH auth(known_hosts + 私鑰)從哪來
- **Production prereq**:TEEMO 主 server `docker compose -f bot/docker-compose.yml build --no-cache central-bot` 後真打 `/self_deploy bot` Step 4 Execute
- 14 handler tests 全綠(12 既有 + 2 新加,新 case 用 `monkeypatch.setattr` 抓 args list 直接驗 mounts)
- ⭐ 累積 9 reality miss 收尾,Sprint 4 J1 end-to-end work 待真打驗收

---

### 2026-05-03:Sprint 4 完整 close

PR-4.5 ship 後 Sprint 4 close。詳細 close report:
[docs/sprints/sprint-4-close-report.md](../sprints/sprint-4-close-report.md)

11 reality miss 累積經驗寫入:
[docs/handoffs/sprint-4-lessons-learned.md](./sprint-4-lessons-learned.md)

**Sprint 4 follow-up 移交:**
- F1 ✅ PR-4.5 解(handler logging — `_git_pull_via_docker` 加 4 個 INFO/ERROR 點)
- F2 `/self_deploy nexw-deploy-bot` alias UX(評估,Sprint 5 candidate)
- F3 I1 `check_open_prs` squash/rebase 誤判(Sprint 5 H1 GitHub API 改)
- F4 PR-4.4c systemd timer(移 v2.1 candidate)
- F5 ✅ PR-4.5 解(Sprint 5 spec 模板「Runtime Reality 驗證」7 點 checklist 寫入 lessons-learned)

> ⚠️ **歷史快照(Sprint 5→6 era,已過時)** — 真實當前狀態見本檔 footer + [v2.0-progress.md](../sprints/v2.0-progress.md)。
**(當時)Sprint:** Sprint 6 — 結構統一 + 防護升級(Sprint 5 ✅ Closed at 2026-05-05)
**(當時)PR:** PR-5.6(Sprint 5 close 儀式),後續 PR-6.1 開工

### 8.3 Governance Track Stage 1 close 發現(2026-05-12 PR-G.6;PR-6.2.1 reality update)

**KI-G-1:raw.githubusercontent.com cache lag**

- **症狀:** 2026-05-12 新 chat 接手 SOP web_fetch 5 個 raw URL,全 stale at 2026-05-01(lag 9-11 天),snapshot.sh 拍板才看到真實 reality
- **影響:** Project Instructions §1 SOP「web_fetch 5 個 raw URL」不可單獨信任,需 fallback snapshot.sh
- **何時修:** Sprint 6 G1 後續評估 patch Project Instructions §1 SOP,加 fallback 規則
- **Workaround:** 新 chat 接手 web_fetch 後若 reality 看似大幅 drift(例:current-state.md Last updated 落後 1 週以上),自動 ssh server 跑 snapshot.sh 拍板
- **Lesson 對應(2026-05-28 PR-6.B 升正式):**
  - L9 / L10 / L11 / L12 候選 → ✅ 升正式(本 PR-6.B)
  - 完整 wording + 真打驗證紀錄見 [`docs/automation/lessons-catalog.md`](../automation/lessons-catalog.md) §2
  - L11 alias「footer 完整性」/ L12 alias「不假設 default」/ L13 正式化(footer self-stale write-side)同 PR ship
  - 原候選 wording(PR-6.2.1 累積)歷史紀錄保留於 `docs/automation/v2.0-changelog.md` 2026-05-12 PR-6.2-koalo-frontend-audit entry

**KI-G-2:snapshot.sh 段 7 container listing 不完整 ⚠️ Reality update(2026-05-12 F-A1 audit)**

- **原症狀(已過時):** 2026-05-12 snapshot.sh 段 7 只看到 3 個 container(nexw-deploy-bot / nexw-central-bot / nexw-litellm),memory + handoff #2026-05-07 寫 5 個 server-running unit deploy 完成
- **F-A1 audit reality(2026-05-12):** 5 unit + 支援 container 全部 healthy running
  - `koalo-frontend` (Up 2 days) / `koalo-backend` (Up healthy) / `koalo-postgres` (Up 10 days healthy)
  - `jy-official-web` (Up 5 days) / `trinova-center-web` (Up 10 days) / `taoyuan-union-web` (Up 2 weeks)
  - `nexw-deploy-bot` / `nexw-central-bot` / `nexw-litellm` / `traefik` (Up 2 weeks)
  - 共 10 個 container running,對齊 products-reality.md 速查表
- **Root cause:** snapshot.sh 段 7 listing 不完整(可能有 grep filter / 漏列 web container / 漏列產品 container)
- **真實影響:** snapshot.sh tooling bug,**Sprint 6 G1「所有產品 repo 結構一致」對齊範圍不受影響**(5 unit reality 都在,規範可正常推進)
- **何時修:** Sprint 6 PR-6.7(snapshot.sh 段 7 fix)
- **F-A1 audit 指令(供後續 audit reuse):**
```bash
  docker ps -a --format 'table {{.Names}}\t{{.Status}}\t{{.Image}}' | grep -E 'koalo|jy-official|trinova|taoyuan'
  ls -la /srv/projects/
  find /srv/projects -maxdepth 2 -name docker-compose.yml
```

**KI-PR-6.2:koalo.md §5.2 merge mode doc drift(2026-05-12 PR-6.2-koalo-frontend-audit ship 階段 surface)**

- **症狀:** Sprint F PR-6.2 ship 階段,koalo chat AC surface koalo.md §5.2 寫法「`koalo-frontend` 預設 merge commit(若未來改 squash 再更新本檔)」跟 reality 不一致(PR #67 真實 merge commit `22d4a52` format 為 merge commit,但 Sprint 4 4 PR 全 squash)
- **Reality verified(2026-05-12,via GitHub API + UI Settings):** `koalo-frontend` 跟 `koalo-backend` 三個 merge mode 全 enabled(`allow_merge_commit` / `allow_squash_merge` / `allow_rebase_merge` 三 true)。**無 repo-level default merge button setting**(GitHub UI 無此選項,API 無對應 field)
- **Root cause:** koalo.md §5.2 假設「repo-level default」概念存在(GitHub 無此 setting),屬 stale assumption / doc drift
- **Resolution:** 本 PR(PR-6.2-koalo-frontend-audit)§2.2.3 完全改寫 koalo.md §5.2 → pattern-based wording(描述 historical pattern,非 default)
- **Historical pattern reference:**
  - `koalo-backend`:Squash merge sustained N=6+(PR-6.1b-koalo `0ddfdad` + Sprint 4 4 PR + chore PR + S5-B1 PR + Gap 4 PR #24 `0f7001e`)
  - `koalo-frontend`:混合 pattern(Sprint 4 4 PR 全 squash,PR-6.2 #67 走 merge commit)
  - `trinova-center`:N=1 first sample(PR-6.2-trinova PR #6 `3e25e83` merge commit mode,2026-05-13 ship;對齊 [L12 紀律](../automation/lessons-catalog.md#l12--ac-verbatim-source-verify不假設-default) reality observation 累積開始)
  - `taoyuan-union`:N=1 first sample(PR-6.2-taoyuan PR #9 `8152aa5` **squash mode**,2026-05-13 ship;跟 trinova-center first sample merge commit 不同 → OBS-EE([`docs/automation/obs-tracker.md`](../automation/obs-tracker.md) §2))
- **Lesson:** 對齊 [L12 正式 lesson](../automation/lessons-catalog.md#l12--ac-verbatim-source-verify不假設-default)(2026-05-28 PR-6.B 升正式)+ koalo 軸教訓 #49 跨 chat redux
- **何時關掉:** 本 PR ship 後 ✅ closed(doc drift fixed at PR-6.2-koalo-frontend-audit)

### 8.4 觀察項(進觀察池,暫不處理)

**budget.json 沒 month rollover**(2026-05-01 發現)

`/srv/bot-state/central/budget.json` 的 `current_month` 停在 `"2026-04"`,但今天是 2026-05-01。`history` 也沒輪轉紀錄。

**判讀:** 可能 bot 還沒接 Anthropic API(對,Sprint 5 H1 才接),所以 0 cost 是正確的;但月底 rollover 邏輯沒跑。

**何時看:** 觀察 5 月底會不會自動切到 6 月。若沒,進 Sprint 5 H1(Claude API 接入)scope 一起處理。

### 8.5 Doc 過時更新(本 PR 已收尾)

**「接手第一步」段過時**(2026-05-01 發現,本 PR 已重寫)

舊版段「接手第一步」還在指 Sprint 3 PR-3.10 Hash One template repos / Recon 流程,Sprint 3 已 close 但段沒更新。本 PR(PR-4.1)已重寫成 v2.0 設計階段現況。

### 8.6 Follow-up(TEEMO 自己處理,不影響開發)

- BotFather demo2 token revoke(2026-05-01 cleanup 後補底)

### 8.7 Sprint 5 期間累積

- **PR-5.2 prompt caching cache_read drift**(校正 #13):
  - test:`test_live_prompt_caching_hit` 標 `@pytest.mark.xfail(strict=False)`
  - 影響:v2.0 §6.5.2 手段 2「prompt caching 省 40-60%」延後驗證
  - 何時修:Sprint 10 A1 Idea Inbox 上線前(production 累積真實 user-facing data 便於診斷)
  - 候選方向:升 LiteLLM 版本 / 開 upstream issue / 改 raw Anthropic API call 繞過 LiteLLM
  - 詳見:`docs/sprints/v2.0-progress.md` Sprint 5 校正紀錄 #13

- **`nexw-central-bot:dev` image 缺**(校正 #12):
  - PR-5.2 live test 真打要進 production container `pip install pytest`
  - 一次性 hack 可接受,長遠該另起 dev image + docker-compose dev profile
  - 何時修:Sprint 5 後續 PR 或 Sprint 17 J3 demo cleanup 一併處理

- **PR-5.3 secret 管理 surface 不全**(follow-up):
  - dev surface 只有 `/generate_secret`;**沒有** `/list_secrets <product>` / `/delete_secret <product> <name>` / `/rotate_secret <product> <name>`
  - 影響:rotate / cleanup 階段只能 SSH host 手動 `sed`,沒 TG-side 控制面
  - 何時修:Sprint 5 close 前 PR-5.5 順手做(若 scope 容許),否則進 v2.1 candidate
  - 候選方向:reuse `secret_generator.fingerprint()` 做 `/list_secrets` 列表(只回 fingerprint 不回 value)+ `set_key` 對等的 `unset_key` 做 `/delete_secret`

- **PR-5.3 secret encryption-at-rest 留 v3.0**:
  - .env mode 0o600 file permission 視為 baseline
  - 需要 HashiCorp Vault / AWS Secrets Manager 等 secret manager 整合在 v3.0 candidate

- **PR-5.3.1 Dockerfile COPY 紀律**(校正 #15):
  - 未來 PR 涉及 `central/*.py`(or 其他 Dockerfile-tracked 目錄)新增 module → spec §1 Recon 必加「Dockerfile COPY 模式」驗證
  - PR ship 真打前必跑 image-level smoke test(`docker run --rm <image> python -c "import <new_module>"`),不只跑 sandbox pytest(sandbox 是 source layout,image 是 baked layout,兩者可分歧)
  - PR-5.3 之所以 sandbox 全綠仍 production crash:sandbox `import secret_generator` 走 `central/` source path → OK;production `import secret_generator` 走 `/app/` baked image path → `/app/secret_generator.py` 不存在
  - Follow-up:Sprint 6 G1 結構統一時評估 `product_bot/Dockerfile` / `bot/Dockerfile` 是否也用顯式 COPY 列表 → 若有則一併切 wildcard

---

**KI-PR-5.4-A:/snapshot TG reply markdown parse error**(2026-05-05 PR-5.4 真打發現,PR-5.4.1 修復中)

- **症狀**:`/snapshot nexw-deploy-bot` reply 卡住,server logs 出現 `telegram.error.BadRequest: Can't parse entities: can't find end of the entity starting at byte offset N`
- **Root cause**:`result.short_summary` 含 LLM 動態產出段(雙星號 bold + 反引號 inline code 等 markdown 語法),`[:500]` 截斷可能切到 markdown entity 中間 → telegram parser 抓不到結尾
- **狀態**:[修復中 / PR-5.4.1] — `central/handlers/snapshot.py` line 120 `parse_mode="Markdown"` → `parse_mode=None`,disable markdown rendering 後不再 render bold / inline code
- **Trade-off**:bold / inline code 在 TG 上顯示為 raw chars,.md 附件 + GitHub URL 仍正常 render(主要用途不受影響)
- **Follow-up KI-PR-5.4-A-followup**:line 88 / 110 hard-coded error reply 也帶 `parse_mode="Markdown"`,雖目前 backtick 配對 OK,但未來改文案時容易再爆同樣問題;Sprint 6+ G1 結構統一時評估 escape markdown 或統一 parse_mode 政策
- **Follow-up KI-PR-5.4-A-followup-2** [修復中 / PR-5.5.1]:`/user_memory` 真打階段 LLM 回覆含 markdown fenced JSON(```json\n{...}\n```)或前綴文字,`json.loads(raw_text)` 直接爆 `JSONDecodeError`。PR-5.5.1 Part A 加 `_extract_json` helper(處理 fenced / 前後綴文字 / 純 JSON passthrough)+ `_call_llm` 包 try/except wrap。Sprint 6+ G1 follow-up:helper 抽 `central/shared/llm_helpers.py` 共用

---

### KI-PR-5.4-B:`@nexw_serverbot` [Merge] 按鈕無反應

**狀態:** Layer 1 ✅ Closed(2026-05-05 PR-5.5.1 + server SOP + #108 verify noop)/ Layer 2 ⏳ 留 Sprint 6 G1
**Layer 2 症狀:** deploy-bot polling 仍偶爾 raise `Conflict: terminated by other getUpdates request`,`[Merge]` 按鈕**偶爾** work(race condition),GitHub UI manual merge 100% 通(workaround)
**Layer 2 何時修:** Sprint 6 G1 子題「`.env.example` 結構規範 / token loading 機制統一」順手解(本來就要做,不是 Sprint 6 額外加項)。詳見 [sprint-5-close-report.md](../sprints/sprint-5-close-report.md) 第 5 章 KI 跨 Sprint 追蹤
**Discovered:** 2026-05-04 PR-5.4 真打階段
**Recon V1:** 2026-05-05 PR-5.5 開工前(假設:deploy-bot env-var token rotation 機制差異)
**Recon V7-V16:** 2026-05-05 PR-5.5 ship 後(90+ 分鐘 reality 排查確認真實 root cause)

**Root cause(V7-V16 確認):**
central-bot 跟 deploy-bot 共用同一把 TG token,搶 Telegram long-polling。
Symptom:`[Merge]` callback 間歇 silent drop,看哪個 container 贏 polling race。

**Reality:**
- deploy-bot:`@nexw_serverbot`,token 從 `.env` `TELEGRAM_BOT_TOKEN` 讀(env-var-based)
- central-bot:**file-based**,token 從 `/data/secrets/bot-tokens/central.txt` 讀
  → docker-compose `env_file: .env` 對 central-bot 無效(它不讀 env var)
  → V1 Recon 推論「deploy-bot config 機制」方向錯誤,真實衝突點在 polling 共用 token

**Fix Path I**(本 PR PR-5.5.1 採 — Q5=A「最小 fix」紀律對齊):
- Part A (code):`_extract_json` robust JSON parsing(對齊 KI-PR-5.4-A-followup-2)
- Part B (docs):`bot/.env.example` + 本檔註解標清楚 file-based reality
- **Server SOP**(TEEMO PR-5.5.1 merge 後手動執行):

```bash
# 從 BotFather 拿 @NEXW_Central_Bot token
# 寫進 host file(對應 container 內 /data/secrets/bot-tokens/central.txt)
echo -n "<paste @NEXW_Central_Bot token>" > /srv/bot-state/secrets/bot-tokens/central.txt
chmod 600 /srv/bot-state/secrets/bot-tokens/central.txt

# rebuild + restart central-bot
cd /srv/projects/nexw-deploy-bot/bot
docker compose up -d central-bot

# 驗證 polling 用新 token
docker logs nexw-central-bot --since 60s 2>&1 | grep -iE 'application started|polling'
```

**真打驗證(merge + server SOP 後):**
1. TG `@NEXW_Central_Bot` `/user_memory` → 應 ✅(Part A robust JSON 修)
2. TG `@nexw_serverbot` 任一 PR `[Merge]` → 應 ✅(token 不衝突,server SOP 修)

**TEEMO 已加但無效的 .env entry:**
TEEMO 在 server `.env` 加了 `CENTRAL_BOT_TOKEN=...`(2026-05-05 11:00 UTC)。
依 file-based 機制,此 entry 不會被 central-bot 讀,留著無害,
留作 G1 統一機制時 pre-existing data。

**Sprint 6+ G1 follow-up:**
- 統一 token loading 機制(file-based vs env var,二選一,別兩種共存)
- 對齊 `.env.example` + secret-management 文件結構
- SERVER_BOT_TOKEN deploy-bot 重命名(對齊 BotFather 名)

**舊 short-term fix 紀錄(2026-05-05 09:52 UTC,已 superseded):**
當時 `docker restart nexw-deploy-bot` 對齊 GITHUB_PAT,但那不是 [Merge] 按鈕真正 root cause
(GitHub PAT 影響的是 webhook / pending_merge get_pr,不是 callback dispatch)。
V7-V16 Recon 才確認真實 cause 是 TG token polling 衝突。

---

## 9. 規則摘要

- 繁中 + 英文術語(repo / PR / merge / deploy / container / endpoint)
- 不偷跑(每步驟先說「我要做 X」等確認)
- 給選項(重大決策 2-3 方案 + 優缺 + 推薦 + 理由)
- 誠實不討好
- Secret 不貼 chat(看到 ssh-/key/fingerprint 立刻擋)
- 文件透過 Claude Code 寫進 repo,使用者不下載
- 設計藍圖(v1.0.md FROZEN,新設計 v2.0.md)
- 客戶 server IP / token / SSH key 絕不出現在 chat
- Sprint 編號規則:server Sprint = v2.0 §15 Sprint(強制同步)
- 後端規範:必含 Swagger spec + UI in `/docs`(條 15)

---

**Last updated**: 2026-06-11(PR-23.1 DB1 container 骨架 shipping;PR-D12-hotfix ✅ merged `0fff2c0` #260;Sprint 23 進行中)
**當前 Sprint**: v3.0 階段 — Sprint 23 進行中(Discord bot 地基 + adapter);v2.0 已封版(2026-06-09),live 進度見 v3.0-progress.md
**當前 PR**: PR-23.1 DB1 nexw-discord-bot container 骨架(runtime-wired)(shipping)
**Next action**: PR-23.1 merge 後 rebuild `discord-bot` + L18 smoke + Discord 真打(bot 綠/上線訊息/ping-pong);接 PR-23.2 DB2 adapter。PR-D12 V1-V5 驗收(轉 private)依既排程推進。**R-001 已廢止 30 天設計凍結(2026-06-11 提前解凍,原至 2026-07-09)→ v3.0 即日動工**,建造期新需求進 v4.0 候選池。**v2.0 封版後續追(非 blocker,見 `v2.0-acceptance-report.md §3`):** KI-PR-5.4-B([Merge] 按鈕,v3.0 Sprint 26 DM4 順帶解)/ H6 Gate2 待 PAT `Checks:read` / H8b Flutter pipeline 待首客戶 / C1 briefing richer 待 L2 / koalo typesense healthcheck / registry housekeeping 2 項 / KI-update-detached-head / effort 值 / snapshot regex(CF4)。**v4.0 候選池:** 見 v3.0.md §5(自動商業 pipeline / AI 即時語音 / Client 頻道 …)。

< KI-PR-5.4-B verification after PR-5.5.1: 2026-05-05T12:17:53Z -->
