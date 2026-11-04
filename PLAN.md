# intro-to-multipass-macos plan

Tom Dean, updated 2026-11-04

The guide is accurate for Homebrew's Multipass 1.16.4. Nothing here is urgent, and each item is its own small PR.

## Done

PR #1 (2026-11-04) replaced the HyperKit and VirtualBox text, updated the macOS requirement, noted that the cask is community-maintained, moved the `multipass.run` links, fixed the Homebrew link, updated all sample output from a real run, and added the CC BY 4.0 license.

## To do

1. Review the prose added on 2026-11-04 with the `humanizer` skill: the notes on the cask, blueprints, and the `lts` alias, plus the License section.
2. Check the steps that weren't run. Open a real `multipass shell` session and confirm the prompt and output. Confirm `brew uninstall multipass` and `brew uninstall --zap multipass` on a machine where removing Multipass is safe.
3. Once `cka-d-cluster-builder-multipass` has a release, link to it from the References and Summary as the next step. Do the same in the Ubuntu guide.

## Waiting on Homebrew

When the cask reaches Multipass 1.17 or later (check with `brew info --cask multipass`), rerun the whole guide, replace all the sample output, and bring the text in line with the Ubuntu guide:

- Remove the blueprint note if `multipass find` no longer lists them.
- Add the availability zone to the `Launched:` text and the `multipass list` samples.
- Update the `lts` note if it points to a newer release.

## Decisions

| Question | Decision |
| --- | --- |
| Keep or archive | Keep (Tom, 2026-11-04) |
| License | CC BY 4.0, because the repo is prose only |
| Install method in the guide | Homebrew, with a note about Canonical's installer |
| Sample output | Real runs only |

## Open questions

Should the guide recommend Canonical's installer over Homebrew? Multipass's README calls Homebrew "not officially supported", and the cask is behind. Keeping Homebrew is the smaller change, and it's Tom's call.
