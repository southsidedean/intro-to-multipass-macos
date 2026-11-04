# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status (as of 2026-11-04)

PR #1 refreshed and licensed the guide. Nothing is in flight. `PLAN.md` lists the small jobs left, and they wait until Tom asks for them.

## What this repo is

Tom Dean's beginner guide to Multipass on macOS: a `README.md` tutorial and a CC BY 4.0 `LICENSE`, with no code. It started in 2023.

Related repos under `southsidedean`:

- `intro-to-multipass-ubuntu` is the same guide for Ubuntu. When a change applies to both, keep them consistent.
- `cka-d-cluster-builder-multipass` is the CKA/CKAD lab built on Multipass, still in progress. Both guides should link to it once it has a release.
- `southsidedean` is the profile repo. Its README lists this guide under "Guides and labs".

## Where the sample output came from

All of it was captured on 2026-11-04 from an Apple Silicon Mac on Multipass 1.16.4, the version in the Homebrew cask, using a temporary instance called `resilient-dobsonfly`.

Some steps weren't run:

- The `multipass shell` example uses `multipass exec` output with a shell prompt added, because both print the same file.
- `brew install` and the two `brew uninstall` commands.

Multipass 1.17 output differs. Going by the Ubuntu guide's samples, expect a `Zone` column, a `Launched: <name> in <zone>` message, more commands in `multipass help`, and no blueprints in `multipass find`. The guide can only move to 1.17 samples once the Homebrew cask does.

## Testing

Every sample in the README comes from a real run. Never invent output or edit it to look newer.

- Ask Tom before launching an instance, because the test machines are shared with other work.
- Run `multipass list` first. Anything you didn't create belongs to someone else, so leave it alone.
- `multipass purge` permanently removes every deleted instance on the host, including ones you didn't create. Only run it when `multipass list` shows nothing else. Otherwise clean up with `multipass delete --purge <name>`.

Scripts have tripped on two things:

- On 1.17, `Launched: my-vm in zone1` means the instance is `my-vm`. A script that takes everything after `Launched: ` gets the wrong name, and its cleanup fails with the instance still running.
- `multipass exec` reads stdin, so inside a heredoc or `bash -s` it consumes the rest of the script. Over SSH, use `ssh -n` and `</dev/null`.

Confirm cleanup with `multipass list` afterward, since a script's exit status won't show a leftover instance.

## Conventions

- Never invent facts. The public email is tom@southsidedean.com. Review prose with the `humanizer` skill, and keep Tom's voice and the guide's structure.
- Work goes through a branch and a PR, and Tom merges. Ask before committing, pushing, or opening PRs, and check `git status` and the current branch first. This repo has no CI or branch rules, so nothing checks a PR automatically.
- Private details, such as test host names, go in `CLAUDE.local.md`, never in this file or `PLAN.md`. `.git/info/exclude` keeps `CLAUDE.local.md`, `.claude/`, `.mcp.json`, `.backups/`, and `.DS_Store` out of git.
- Use they/them for Tom.
