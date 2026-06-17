# Blackjack Project Planning Notes

This file holds planning context, implementation sequencing, open judgment calls, and notes about how the polished specification was derived. The implementation contract lives in `Specifications.md`.

## Source Context

The original `directions.txt` is a detailed development journal and teaching sequence. It contains useful curriculum ideas, temporary testing hacks, Replit-specific notes, duplicate numbering, known defects that are fixed later, and unfinished TODOs.

`Specifications.md` should be readable without `directions.txt`. Use this planning file for anything that explains why the specification is shaped a certain way, or what still needs sequencing decisions.

## Current Decisions

- Build one milestone at a time toward the full baseline.
- Use 3:2 Blackjack payout.
- Dealer hits soft 17.
- Dealer peeks for Blackjack when showing an Ace or ten-value card.
- Double is allowed on any two-card total.
- Double is allowed after a split except on split Aces.
- Surrender is allowed as the first action on any two-card hand, including after a split.
- Split equal Blackjack values, including mixed ten-value cards.
- Do not re-split Aces.
- Use an eight-deck shoe.
- Default to `felt_green.png` and `card_back4.png`.
- Prefer clean model/view separation; UI state should not live in `Hand` unless a teaching milestone deliberately accepts that tradeoff.
- Produce student-facing instructions as part of each milestone.

## Remaining Implementation Judgment

- Decide during implementation whether source steps 100-108 fit naturally into Milestone 6 or should move to optional enhancements.
- Decide whether `CardFaceCreator` glyph rendering is acceptable after Windows visual testing.
- Decide exactly how to handle a mid-round shoe exhaustion event, then document and test that policy.
- Decide whether to introduce a named Java package. If introduced, update `pom.xml` and explain packages in the student instructions.
- Decide whether the weighted random shoe algorithm or a flat shuffled shoe is better for the first AI implementation. The weighted algorithm remains a useful teaching moment.

## Deferred Features

These ideas appear in the source material or review comments, but are not required for the first complete baseline:

- Insurance
- Player basic-strategy automation
- Strategy deviation hints
- General hint manager
- Sound effects
- Fully functional settings dialog
- Runtime theme and card-back switching
- Configurable dealer soft-17 behavior
- A dedicated split manager
- Advanced payout/result animations

The architecture should leave room for these features. Settings should be centralized in a common configuration class even if no settings dialog is implemented yet.

## CardFaceCreator Notes

`src/main/java/CardFaceCreator.java` has been ported into this repository.

Planning notes:

- Treat `CardFaceCreator` as supplied scaffolding, not as an assignment for first-year students to write from scratch.
- Preserve the attribution already present in the file.
- It currently renders card faces from `Suit` and `Rank` and uses `/card_back4.png` for card backs.
- It relies on text/glyph rendering for suit symbols. Verify the visual result on Windows.
- If glyph rendering becomes a portability problem, replace the internals behind the same public card-rendering API.

## Corrections to Original Directions

The implementation and student materials should correct these issues from `directions.txt`:

- Use Java naming convention `isBusted`, not `IsBusted`.
- Use `GameOptions`, not both `GameOption` and `GameOptions`.
- Use `src/test/java`, not `/test/java`.
- Correct the malformed generic return type for chip conversion to a map from chip type to count.
- Avoid unused imports such as `HashMap` when `LinkedHashMap` is used.
- Avoid binary floating-point for authoritative currency calculations.
- Do not claim that growing a list while iterating is forbidden by the compiler; unsafe mutation may instead cause runtime or logic problems.
- Java enums are class-like types with singleton enum constants, but each enum constant is not normally described as a separate class.
- The four static suit instances are not the Singleton design pattern in the usual singular-instance sense; they are closer to a fixed set of instances.
- Shuffling a list is not inherently inefficient for this project and is often simpler than repeated random removal.
- Resource paths must be classpath-correct and must not begin with `/src/main/resources/`.
- Fix mojibake characters and typographical errors before publishing material to students.
- Remove duplicate and conflicting step numbers.
- Label temporary defects and testing hacks clearly, and remove them before the next milestone.

## Project Plan and Milestones

Each milestone should produce two kinds of deliverables:

- Implementation artifacts, such as model classes, UI classes, tests, and resource wiring.
- Student-facing instructions, including teaching explanations, vocabulary, checkpoints, and pointers to free learning resources where helpful.

Milestone instructions should align with the overall progression in `directions.txt`, while cleaning up duplicate numbering, temporary hacks, and wording problems.

### Milestone 0: Specification Approval - Complete

Deliverables:

- Completed: `Specifications.md` was created as the implementation contract.
- Completed: working Blackjack-rule decisions are recorded in the specification.
- Completed: baseline scope and deferred features are recorded.
- Completed: default felt, card back, shoe size, and payout are recorded.
- Completed: each milestone will include student-facing instructions.

Exit criteria:

- Met: no unresolved rule affects core game flow.
- Met: implementation boundary is explicit.

### Milestone 1: Project Skeleton and Core Model

Deliverables:

- Maven source/test layout
- Application entry point
- `Rank`, `Suit`, and `Card`
- `Deck`, `Shoe`, and `ShoeMgr`
- Core model unit tests
- Student instructions for the project skeleton, Maven layout, card model, deck, and shoe

Exit criteria:

- Build passes.
- Deck and shoe tests pass.
- Core model tests do not require opening a JavaFX window or creating JavaFX scene nodes. `Suit` may still use JavaFX `Color` because `CardFaceCreator` depends on it.

### Milestone 2: Hand Scoring and Dealer Round

Deliverables:

- `Hand`
- Ace and soft-hand logic
- Natural Blackjack logic
- Dealer strategy
- Non-interactive round analyzer
- Student instructions for hand scoring, Aces, dealer rules, and round outcomes

Exit criteria:

- Scoring and round-outcome tests pass.
- A complete round can run without UI.

### Milestone 3: JavaFX Playable MVP

Deliverables:

- Main table scene
- Programmatic card fronts using `CardFaceCreator`
- Existing card-back and felt assets
- Dealer hole card
- Interactive Hit and Stand
- Score badges
- Round result dialog
- Student instructions for JavaFX startup, layout, resource loading, and connecting model state to UI

Exit criteria:

- A user can play repeated Hit/Stand rounds.
- UI and model state remain synchronized.

### Milestone 4: Betting and Bankroll

Deliverables:

- Bankroll and wager model
- Bet dialog
- Win/loss/push/Blackjack payouts
- Double and Surrender
- Bankroll display
- Student instructions for wagers, bankroll, currency formatting, action validation, Double, and Surrender

Exit criteria:

- Money tests pass.
- Illegal unaffordable actions are unavailable.
- The user can complete repeated wagered rounds.

### Milestone 5: Splitting

Deliverables:

- Multiple player hands
- Independent per-hand wagers
- Maximum split enforcement
- Split Aces
- Equal-value ten-card splitting
- Two-pass player/dealer flow
- Split-hand UI indicator and results
- Student instructions for splitting, multi-hand state, per-hand wagers, and the two-pass dealer flow

Exit criteria:

- All split scenarios in the testing checklist pass.
- Dealer plays once per round.
- Bankroll changes correctly for every hand.

### Milestone 6: Chips, Assets, and Polish

Deliverables:

- `WagerChip`
- Tested chip conversion
- Existing chip-image preview in bet dialog
- Gear icon
- Default felt and card-back selection
- Button styling and layout fixes
- Student instructions for chip conversion, TDD, visual polish, and asset usage

Exit criteria:

- Chip-conversion tests pass.
- All existing required assets are visibly used or deliberately documented as optional variants.
- UI smoke test passes.

### Milestone 7: Optional Enhancements

Possible deliverables:

- Settings dialog
- Theme and card-back selection
- Configurable soft-17 rule
- Basic strategy
- Hint manager
- Sound effects
- Student instructions for any selected optional enhancement

Exit criteria:

- Each selected enhancement has its own approved behavior and tests.
