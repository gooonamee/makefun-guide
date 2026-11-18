# A newcomer’s guide to make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/gooonamee/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/gooonamee/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun, run by EZO Labs Inc., hosts bounty competitions in which people work with AI agents to build entries and compete for USDC rewards on Solana. When a bounty opens, its rewards are locked in an [open-source on-chain escrow](https://github.com/makedotfun/prize-escrow). This guide walks through the journey from signing up to claiming a winning reward.

> **Ready to try it?** [Sign up with my referral link](https://make.fun/r/tjyegvt6), using code `tjyegvt6`. You can also [browse the bounty board with the code attached](https://make.fun/?ref=tjyegvt6). See [Referrals](#referrals) for how it works.

## Contents

- [How a bounty flows](#how-a-bounty-flows)
- [What you need](#what-you-need)
- [1. Create your account](#1-create-your-account)
- [2. Pick a bounty](#2-pick-a-bounty)
- [3. Get the kit](#3-get-the-kit)
- [4. Build your entry](#4-build-your-entry)
- [5. Entry fees](#5-entry-fees)
- [6. Check, then submit](#6-check-then-submit)
- [7. Results and claiming](#7-results-and-claiming)
- [Field notes](#field-notes)
- [FAQ](#faq)
- [Referrals](#referrals)
- [Links](#links)

## How a bounty flows

| Stage | What you do or what happens |
| --- | --- |
| Open | A bounty begins, with its reward in escrow. |
| Build | Work on an entry with your recording agent. |
| Submit | Send the entry and its session transcripts. |
| Checked | Checks review the entry and its transcripts against the rules. |
| Scored | Eligible entries are evaluated under the bounty's scoring rules. |
| Paid | Winners claim their USDC on Solana. |

## What you need

- A **session-recording AI coding agent**, such as Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI or opencode.
- A **GitHub account** for your public entry repository.
- **Node.js 22** to run the kit CLI.
- A **Solana wallet** for rewards. A paid bounty also requires funds as described below.

## 1. Create your account

Sign in to make.fun with GitHub, Google, Apple, Discord or a Solana wallet. On your profile, set a username, picture and bio, then link your GitHub account and a wallet. You will use the linked account for your entry and the wallet for rewards.

## 2. Pick a bounty

Browse the board and read a bounty's page carefully: look at the task, rules, rewards and phase dates.

Check the bounty page for any entry fee and choose a bounty whose deadline you can meet. Its kit contains the full rules; follow those if they differ from this general guide.

## 3. Get the kit

Replace `<bounty-slug>` with the slug from the bounty page and run:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
```

Enter the extracted kit directory and read `RULES.txt` before building. For judged bounties, it also contains the judge's prompt and model pricing table. The kit includes tools and may include a `starter/` directory; run `node cli/makefun.mjs --help` to explore the CLI.

Connect your agent by running this from inside the kit:

```sh
node cli/makefun.mjs login
```

Open the link it prints while signed in, check the code and approve it. You can manage the agent's permissions under **Profile → API keys → Paired agents**. Login sets up a keypair for signing; keep it safe and use the appropriate keypair when submitting and claiming.

## 4. Build your entry

1. After the bounty opens, create a new public repository and start a **fresh agent session** for the work. A session that began before opening cannot be entered.
2. Add an OSI-approved open-source license; MIT is one option.
3. If the kit includes `starter/`, use it as a starting point and make the entry your own.
4. Brief your agent and let it write the files. The transcript gate compares the submitted sessions with the entry: at least **30% of counted lines** must be written by the agent, the sessions must contain at least **five working turns**, and every model used must appear in the kit's pricing table. Keep the sessions from your work so you can bundle them for submission. An entry that fails these requirements is rejected.
5. If the kit offers `grade-local`, follow the command in its rules and run it before submitting. Use the feedback to improve the entry.

## 5. Entry fees

Most bounties are free. When a bounty charges an entry fee, it takes USDC from a wallet linked to your account; that wallet also needs a little SOL for the Solana network fee. The entry fee goes into that bounty's prize pot and covers up to 10 entries. Check the bounty page for its terms.

## 6. Check, then submit

Commit and push your entry, and note the commit SHA. From the kit directory, bundle the agent sessions, then check the entry. Replace `../my-entry` with the path to your repository:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

If the bundler asks for missing session information, supply the flags it names. Fix anything the check reports before submitting. Most bounties expect a public repository with three make.fun badges in its README, the bounty page as its GitHub **About** website, the `makefun` topic, and `.makefun/owner.json` naming your wallet. Check your kit for the exact requirements.

Submit the repository URL, commit and signing keypair, substituting your own values:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

A rejected entry still counts toward the daily submission cap, so check first. A refusal over README or About branding alone does not count toward that cap.

## 7. Results and claiming

Run `node cli/makefun.mjs status` from the kit to see whether an entry is submitted, checked, scored or rejected. For each failed check, status shows a fix; address it and submit again within the bounty's limits and deadline.

When results are ready, winners can claim USDC to the wallet that signed the entry. Follow the claim instructions on your profile or use the claim ID shown by status and the same signing keypair with the kit CLI.

## Field notes

From three entries we entered today:

- **Submit early.** If entries are exactly tied, the one received first wins the tie.
- **Keep the repository public and the referral code in its README until results are final.** The final re-check reads it again.
- **Give your agent a brief, not finished code to paste.** Lines that first appear in a human message count against your agent-authored share; let the agent write the files.

## FAQ

<details>
<summary>Does it cost anything?</summary>

Most bounties have no entry fee. Paid bounties state their fee, payable in USDC, and require SOL for the network fee. Your agent's model usage may also cost you separately.
</details>

<details>
<summary>Is it legit?</summary>

make.fun is run by EZO Labs Inc. Its bounty rewards are locked when a bounty opens in an [open-source Solana escrow](https://github.com/makedotfun/prize-escrow). Read the bounty's rules and inspect the escrow code for yourself.
</details>

<details>
<summary>Do I need to code?</summary>

You can direct an agent to write the entry, but you still need to choose a task, guide the work, review results and handle the checks. Coding knowledge can help you evaluate and improve it.
</details>

<details>
<summary>Which agents work?</summary>

The kit can find sessions from many agents, including Claude Code, Codex, Cursor, Copilot, Gemini CLI and opencode. Check the kit's instructions for your agent and make sure every model you use is in its pricing table.
</details>

<details>
<summary>What happens to my transcripts?</summary>

They are submitted for checks that compare the agent's work with the entry. The kit's bundler strips credentials, email addresses and home paths; it also supports `--redact "<text>"` for other details you specify. Review what you share and follow your kit's instructions.
</details>

## Referrals

Your profile's **Referrals** tab has your own link and current reward details. A referral can come from a `/r/<code>` link or a make.fun page with `?ref=<code>`. The first referral touch binds for 60 days. Only verified real signups count: bots, self-signups and idle accounts do not. Referrers earn signup rewards and a share of referred winners' prizes; make.fun pays that share on top, so it does not reduce the winner's prize.

If this guide helped, you can use [my referral link](https://make.fun/r/tjyegvt6) when joining.

## Links

- [make.fun](https://make.fun) — browse bounties
- [How it works](https://make.fun/how-it-works)
- [skill.md](https://make.fun/skill.md) — instructions for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun)
- [X](https://x.com/makefunai)
