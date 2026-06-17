# Blackjack Project: Polished Instructions and Implementation Plan

## Purpose

This project has two related goals:

1. Teach beginning Java students, especially students preparing for AP Computer Science A, by building Blackjack in small, understandable steps.
2. Provide a precise source-of-truth specification from which an AI or experienced developer can independently build a JavaFX Blackjack game.

The student path should emphasize reasoning, debugging, loops, conditionals, classes, collections, enums, interfaces, inheritance, recursion, algorithms, testing, and refactoring. Students should implement each checkpoint themselves rather than receiving a complete solution up front.

The final application should be a playable desktop Blackjack game with a JavaFX interface, wagers, a bankroll, common player actions, multiple decks, and existing visual assets.

## Source Material Assessment

The original `directions.txt` contains a strong curriculum and many useful implementation observations. It also combines several kinds of material:

- Beginner programming lessons
- Incremental implementation directions
- Temporary testing hacks
- Replit-specific setup notes
- Known defects that are intentionally fixed later
- Future ideas and unfinished TODO items

For clarity, this polished document separates:

- The final product requirements
- The recommended student learning sequence
- Dependencies and assets
- Unresolved decisions requiring approval
- Milestones and acceptance checks

The original directions remain useful as a detailed lesson journal, but this document should become the implementation contract after its open decisions are approved.

## Current Repository Constraints

### Existing Build Configuration

Use the existing `pom.xml`.

The current Maven project specifies:

- Java 17
- JavaFX Controls 18.0.2
- JavaFX FXML 18.0.2
- JUnit Jupiter 5.9.2
- Maven Surefire for tests
- `Main` as the application entry point

Expected commands:

- Verify Maven: `mvn -version`
- Run tests: `mvn test`
- Run the application: `mvn clean javafx:run`

### Expected Project Layout

- Production Java: `src/main/java`
- Resources: `src/main/resources`
- Tests: `src/test/java`

The original directions incorrectly mention `/test/java`; Maven expects `src/test/java`.

### Existing Assets

Use the existing images. Do not regenerate or replace them unless separately approved.

Felt backgrounds:

- `felt_green.png`
- `felt_blue.png`
- `felt_burgundy.png`

Card backs:

- `card_back1.png`
- `card_back2.png`
- `card_back3.png`
- `card_back4.png`
- `card_back5.png`

Chip images:

- `chip_white.png` for $1
- `chip_pink.png` for $2.50
- `chip_red.png` for $5
- `chip_green.png` for $25
- `chip_black.png` for $100
- `chip_purple.png` for $500
- `chip_orange.png` for $1,000

Other:

- `gear.png`

All resources must be loaded from the application classpath. File-system paths tied to a particular computer or IDE must not be used.

## Final Product Scope

### Required Baseline

The approved baseline should produce a coherent, playable game with:

- A JavaFX desktop interface
- One player against one dealer
- A multi-deck shoe
- Correct card uniqueness within each deck
- Cut-card reshuffling between rounds
- Correct Blackjack hand scoring, including soft Aces
- Natural Blackjack detection
- Dealer hole card behavior
- Player actions: Hit, Stand, Double, Surrender, and Split
- A maximum of four player hands after splitting
- Wagers, payouts, and bankroll tracking
- A wager-selection dialog
- Existing felt, card-back, chip, and gear assets
- Score badges and bankroll display
- Tests for non-UI business logic

### Deferred Features

The following ideas appear in the source directions but are not complete enough to be baseline requirements:

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

These should be implemented only after the baseline is complete and after their behavior is separately specified.

## Proposed Rule Model

This section records the proposed defaults needed to make the game deterministic. Items marked **Approval needed** are assumptions inferred from the original directions.

### General Round Rules

- The player and dealer each receive two cards.
- The dealer's second card is hidden until dealer play or until it must be revealed to prove dealer Blackjack.
- A natural Blackjack is exactly two cards totaling 21.
- A Blackjack created after a split is treated as an ordinary 21, not a natural Blackjack. **Approval needed**
- If both original hands have natural Blackjack, the result is a push.
- If only the player has natural Blackjack, the player receives the configured Blackjack payout.
- If only the dealer has natural Blackjack, the player loses.
- Otherwise, the player acts before the dealer.
- The dealer acts only after all active player hands are complete.
- The dealer's one final hand is compared independently against every player split hand.

### Dealer Rules

- Dealer draws until reaching at least 17.
- Proposed default: dealer stands on soft 17. **Approval needed**
- Dealer does not continue drawing after all player hands have busted or surrendered.

### Player Actions

- Hit is allowed until the hand busts, reaches 21, or another rule ends the hand.
- Stand ends action for that hand.
- Double is allowed only as the first action on a two-card hand. **Approval needed**
- Double deducts an additional wager equal to the original wager.
- After doubling, exactly one card is drawn and the hand automatically stands.
- Surrender is allowed only as the first action on the original two-card hand. **Approval needed**
- Surrender returns half the wager.
- Proposed default: surrender is not allowed after splitting. **Approval needed**

### Splitting

- A hand may split when its first two cards have equal Blackjack values. This permits mixed ten-value cards, such as Jack and King.
- Splitting deducts an additional wager equal to that hand's current wager.
- At most three splits are allowed, producing at most four player hands.
- A hand cannot split or double when the bankroll cannot cover the additional wager.
- Split Aces receive exactly one additional card each and may not be re-split.
- Proposed default: doubling after a split is allowed except on split Aces. **Approval needed**
- Each split hand has an independent wager, action state, result, and payout.

### Money Rules

- Starting bankroll: $400
- Table minimum: $15
- Table maximum: $1,000
- Wagers are selected in $5 increments.
- Proposed default Blackjack payout: 3:2. The source directions mention both 3:2 and 6:5. **Approval needed**
- If the bankroll is below the table minimum, the game may add another $400 after informing the user.
- Currency calculations should use integer cents or an exact decimal representation. Do not use binary floating-point as the authoritative money representation. This corrects a weakness in the original directions.

### Shoe Rules

- Default shoe size: two decks, matching the original `GameOptions` example. **Approval needed**
- Each deck contains exactly 52 unique cards.
- Every remaining card in the shoe must have equal probability of being drawn.
- The cut-card threshold is 12 remaining cards per deck.
- Crossing the cut-card threshold schedules a reshuffle after the current round, never during an active round.
- Running out of cards during a round should be prevented by the threshold. The implementation must still fail clearly if this invariant is violated.

## Core Card Model

### `Rank`

Represent ranks with an enum.

Each rank provides:

- Full display name
- Short display name
- Blackjack value

Rank values:

- Ace: 11 initially
- Two through Nine: face value
- Ten, Jack, Queen, King: 10

### `Suit`

Represent suits with an enum.

Each suit provides:

- Name
- Symbol
- Display color

Suits:

- Hearts
- Diamonds
- Clubs
- Spades

### `Card`

A card consists only of:

- One `Rank`
- One `Suit`

A card should provide:

- Rank and suit accessors
- Blackjack value
- Ace detection
- Human-readable text such as `Ace of Hearts`

Cards should not randomly create themselves in the final design. Random card generation is an early student exercise only.

## Deck, Shoe, and Shoe Management

### `Deck`

Responsibilities:

- Construct exactly one card for every rank and suit combination
- Report the number of remaining cards
- Remove and return a random remaining card
- Return no card when empty

### `Shoe`

Responsibilities:

- Own the configured number of decks
- Report the total number of remaining cards
- Draw cards with equal probability across all remaining cards
- Encapsulate decks so other gameplay classes do not access decks directly

The weighted-deck selection lesson from `directions.txt` is valuable for students. A final implementation may instead flatten and shuffle all cards if it preserves equal probability and clean ownership.

### `ShoeMgr`

Responsibilities:

- Own the active shoe
- Draw cards for game participants
- Detect the cut-card threshold
- Replace the shoe between rounds when required

No gameplay class other than `ShoeMgr` should manage reshuffling.

## Hand Logic

### Core Responsibilities

A hand owns its cards and computes facts about them:

- Current score
- Whether it is soft
- Whether it is 21
- Whether it is busted
- Whether it is a natural Blackjack
- Whether it is eligible to split
- Whether it contains two Aces

Ace scoring:

1. Count every Ace as 11.
2. While the score exceeds 21 and an Ace is still counted as 11, reduce the score by 10.
3. A hand is soft when at least one Ace remains counted as 11.

### Model and UI Boundary

The original directions gradually place JavaFX nodes and score badges inside `Hand`. That is understandable as a teaching progression, but it tightly couples game rules to the interface.

For the final implementation:

- The hand model should own card and scoring state.
- JavaFX views should render hand state.
- UI-specific card nodes and badges should not be required to test scoring.

This separation is strongly recommended for the AI implementation and can be introduced to students as a later refactoring lesson.

## Strategies and Round Flow

### `HandStrategy`

Use a strategy interface for how a participant plays a hand.

Required implementations:

- Dealer strategy
- Interactive player strategy

Deferred implementation:

- Basic-strategy player

### `Round`

`Round` coordinates one dealer hand and one or more player hands.

Required flow:

1. Create the initial player hand and dealer hand.
2. Deal two cards to each, alternating participants.
3. Keep the dealer's second card hidden.
4. Resolve original natural Blackjacks.
5. Collect the wager before player action.
6. Let the player complete every active hand, including splits.
7. Reveal and play the dealer hand once, if any player hand still requires comparison.
8. Analyze each player hand against the dealer.
9. Apply each payout to the bankroll.
10. Present results.
11. Ask whether to deal another round.
12. Check the cut card before the next round.

This two-pass player-then-dealer flow fixes the split design defect identified late in `directions.txt`.

## Betting and Bankroll

### `Bankroll`

Responsibilities:

- Store the player's funds exactly
- Deduct wagers
- Deposit payouts
- Report whether an additional wager is affordable
- Format values as US currency for display
- Update the bankroll display through a clean UI-facing boundary

### `Wager`

Each active player hand needs independent wager state.

Responsibilities:

- Store the original/current wager
- Track whether the hand doubled or surrendered
- Compute or record the hand's payout
- Support win, loss, push, Blackjack, surrender, double, and split-related wager changes

A shared mutable `Wager` across all split hands is error-prone. The final design should give each player hand its own wager or use an equivalent per-hand result object.

### `WagerChip`

Represent chip denominations with an enum using integer cents.

Required denominations, from largest to smallest:

- $1,000 orange
- $500 purple
- $100 black
- $25 green
- $5 red
- $2.50 pink
- $1 white

The chip-conversion algorithm must return a valid combination for every supported payout. A simple greedy algorithm fails for amounts such as $4.00 when the $2.50 chip is considered too early. Tests must demonstrate and protect the corrected behavior.

## JavaFX Interface

### Main Table

The main scene should contain:

- Felt background using an existing felt image
- Dealer card area
- Player card area
- Dealer and player score badges
- Bankroll display
- Gear icon
- A clear indication of the active split hand when multiple hands exist

### Card Rendering

- Render card fronts programmatically from rank and suit.
- Render the dealer hole card with one existing card-back image.
- Keep card dimensions centralized.
- Credit any externally derived card-rendering algorithm in its source file.
- Do not rely on Unicode playing-card glyphs as the final card UI.

### Dialogs

Required dialogs:

- Action selection
- Bet selection
- Round result / deal again
- Informational message when funds are replenished

Player action buttons must show only legal actions for the current hand and bankroll.

### Chip Preview

The bet dialog should:

- Display the selected wager
- Show a visual stack or arrangement of existing chip images
- Update the preview whenever the wager changes

### Settings

For the baseline, display the existing gear icon.

A functional settings dialog is deferred until its supported settings, persistence, and effect on an active game are specified.

## Educational Curriculum

The curriculum should be taught as checkpoints. Each checkpoint must compile and run before continuing. Temporary simplifications are allowed when they are clearly labeled and later removed.

### Phase 1: Java Fundamentals

Learning goals:

- Classes and objects
- Instance variables
- Constructors
- Random numbers
- Console input
- `do-while`, `while`, and nested loops
- `if-else`
- `switch`
- `toString`

Student checkpoints:

1. Create a random rank-only card and print it.
2. Repeatedly draw cards based on console input.
3. Handle non-numeric input without crashing.
4. Convert numeric ranks to names.
5. Add suits.
6. Draw and print a five-card hand.

### Phase 2: Collections and Blackjack Scoring

Learning goals:

- `List` and `ArrayList`
- Encapsulation
- Methods with return values
- Incremental state
- Edge-case reasoning

Student checkpoints:

1. Create a `Hand` class.
2. Add cards to a hand.
3. Calculate a score.
4. Draw until reaching 17.
5. Detect busts.
6. Apply face-card values.
7. Add dealer play.
8. Correctly score Aces as 1 or 11.
9. Detect natural Blackjack.

### Phase 3: Decks, Shoes, and Algorithms

Learning goals:

- Nested loops
- Removing list elements
- Object composition
- Probability
- Algorithm correctness
- Optional recursion

Student checkpoints:

1. Build a 52-card deck.
2. Draw without replacement.
3. Build a multi-deck shoe.
4. Identify the biased random-deck algorithm.
5. Implement equal-probability drawing.
6. Optionally replace iterative deck selection with recursion.
7. Add a cut-card threshold and between-round reshuffling.

### Phase 4: Refactoring and Polymorphism

Learning goals:

- Single responsibility
- Encapsulation boundaries
- Interfaces
- Inheritance and polymorphism
- Dependency flow
- Configuration constants

Student checkpoints:

1. Move round flow out of `Main`.
2. Centralize shoe management.
3. Extract dealer play into a strategy.
4. Add realistic two-card dealing and a dealer hole card.
5. Replace numeric rank and suit identifiers with enums.
6. Introduce `HandStrategy`.
7. Add an interactive player strategy.

### Phase 5: JavaFX

Learning goals:

- JavaFX application lifecycle
- Scenes and layout panes
- Controls and dialogs
- Events
- Images and resources
- Separating model from view

Student checkpoints:

1. Launch a JavaFX application.
2. Temporarily display console-style output in a text area.
3. Create a generic output interface as a polymorphism exercise.
4. Create dealer and player card areas.
5. Add a felt background.
6. Render card fronts and backs.
7. Connect rendered cards to game state.
8. Add score badges.
9. Remove the temporary text-area console.
10. Extract scene creation from `Main`.

### Phase 6: Betting and Player Choices

Learning goals:

- State machines
- Currency formatting
- Input validation
- UI-driven decisions
- Business-rule testing

Student checkpoints:

1. Add wager outcomes.
2. Format payouts.
3. Build a bet dialog.
4. Remember the previous bet.
5. Add bankroll tracking and display.
6. Limit bets to available funds.
7. Implement Double.
8. Implement Surrender.

### Phase 7: Splitting

Learning goals:

- Coordinating multiple related objects
- Per-hand state
- Multi-pass algorithms
- Refactoring when a design stops scaling
- Regression testing

Student checkpoints:

1. Represent multiple player hands.
2. Split an eligible hand.
3. Maintain independent wagers.
4. Switch the displayed active hand.
5. Enforce maximum splits.
6. Enforce bankroll restrictions.
7. Handle split Aces.
8. Split equal-value ten cards.
9. Complete all player hands before dealer play.
10. Compare and pay every hand independently.

### Phase 8: Testing and Polish

Learning goals:

- Unit testing
- Test-driven development
- Regression testing
- Boundary cases
- UI polish

Student checkpoints:

1. Test rank and hand scoring.
2. Test Ace adjustments and soft hands.
3. Test deck and shoe sizes.
4. Test no duplicate cards within one deck.
5. Test cut-card behavior.
6. Use TDD to expose and fix the pink-chip greedy-algorithm failure.
7. Add chip previews.
8. Add remaining visual polish.

## Dependencies and Attribution

### Required Dependencies

- JDK 17
- Maven
- JavaFX Controls
- JavaFX FXML, if retained by the existing build
- JUnit Jupiter

### External Source Dependency

The original directions reference a `CardFaceCreator` implementation from:

`https://gist.github.com/Roland09/e00f7e97852c9113877b`

Before using it:

- Verify its license or reuse permission.
- Preserve attribution.
- Record substantial modifications.

If its reuse terms are unclear, create an independent card renderer instead.

## Ambiguities and Missing Assumptions

These items must be decided before implementation begins.

### Scope and Completion

1. Does the first AI implementation target the complete baseline in this document, or stop at an earlier teaching milestone?
2. Are unfinished source steps 93-99 intentionally absent?
3. Should steps 100-108 be included in the first implementation even though the source marks step 100 as the current stopping point?
4. Should the gear icon merely display, or must the settings dialog work?

### Blackjack Rules

1. Does the dealer stand or hit on soft 17?
2. Is the Blackjack payout 3:2 or 6:5?
3. Is surrender early or late?
4. Is surrender allowed after a split?
5. Is double allowed after a split?
6. May any two-card total double, or only selected totals?
7. Can Aces be re-split?
8. Does a two-card 21 after a split count only as 21 rather than Blackjack?
9. Should the dealer peek for Blackjack when showing an Ace or ten-value card?
10. Should splitting require equal ranks or equal Blackjack values? The later source directions say equal values.

### Architecture

1. Should the AI preserve the exact incremental architecture from the lessons, including UI state inside `Hand`, or implement the cleaner model/view separation proposed here?
2. Should the final implementation use the default package because `pom.xml` names `Main`, or introduce a named package and update the POM?
3. Should money use integer cents throughout? This document recommends yes.
4. Should the shoe use weighted random deck selection, or shuffle a flat card list? Both can be correct, but the lesson specifically teaches the weighted approach.

### User Experience

1. Which felt image and card-back image are defaults?
2. Should the application window be fixed-size or responsive?
3. What happens when the player closes a dialog or the application window mid-round?
4. Should bankroll replenishment be automatic, optional, or end the session?
5. Should results for split hands appear all at once or one at a time?

## Corrections to Original Directions

The implementation and student materials should correct these issues:

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
- Fix the source file's mojibake characters and typographical errors before publishing it to students.
- Remove duplicate and conflicting step numbers.
- Label temporary defects and testing hacks clearly, and remove them before the next milestone.

## Testing Checklist

### Card and Deck

- Every deck starts with 52 cards.
- Every rank/suit combination appears exactly once per deck.
- Drawing removes a card.
- Drawing all cards produces no duplicates within one deck.
- Drawing from an empty deck is handled.

### Shoe

- Shoe size equals `52 * deckCount` initially.
- Every draw reduces shoe size by one.
- Draw selection is unbiased across remaining cards.
- Cut-card crossing schedules reshuffling only after the round.
- A new shoe restores the expected size.

### Hand Scoring

- Number cards score correctly.
- Face cards score as 10.
- One Ace can score as 11 or 1.
- Multiple Aces adjust correctly.
- Soft-hand detection is correct.
- Bust detection is correct.
- Natural Blackjack requires exactly two cards.
- A multi-card 21 is not Blackjack.

### Dealer and Round

- Initial deal order and hidden card are correct.
- Dealer Blackjack is handled.
- Player Blackjack is handled.
- Both Blackjacks push.
- Dealer follows the approved soft-17 rule.
- Dealer plays only once after all split hands.
- Every split hand receives an independent result.

### Wagers and Bankroll

- Initial wager is deducted exactly once.
- Win, loss, push, Blackjack, and surrender payouts are correct.
- Double deducts exactly one additional wager.
- Split deducts exactly one additional wager per new hand.
- Illegal actions are hidden or rejected when funds are insufficient.
- Currency formatting is correct.
- Bankroll cannot silently become negative.

### Splitting

- Equal-value cards can split according to the approved rule.
- Maximum split count is enforced.
- Split Aces receive one card and stop.
- Re-splitting Aces follows the approved rule.
- A split hand's cards, wager, score, and result remain independent.
- Dealer card visibility remains correct throughout all player hands.

### Chip Conversion

- $2.00 uses two white chips.
- $4.00 does not incorrectly use a pink chip.
- $4.50 uses one pink and two white chips.
- $7.50, $8.00, $165.00, and $167.50 produce valid totals.
- Every supported payout converts to chips with no remainder.

### JavaFX

- Existing assets load from the classpath.
- Dealer hole card renders with the selected card back.
- Score badges update and reset between rounds.
- Bankroll display updates after deductions and payouts.
- Action dialogs show only legal choices.
- Bet chip preview updates with the selected wager.
- Split-hand navigation is understandable.
- UI remains usable at the target window size.

## Project Plan and Milestones

### Milestone 0: Specification Approval

Deliverables:

- Approve or revise this document.
- Resolve the open Blackjack-rule decisions.
- Confirm baseline scope and deferred features.
- Confirm default felt, card back, shoe size, and payout.

Exit criteria:

- No unresolved rule affects core game flow.
- Implementation boundary is explicit.

### Milestone 1: Project Skeleton and Core Model

Deliverables:

- Maven source/test layout
- Application entry point
- `Rank`, `Suit`, and `Card`
- `Deck`, `Shoe`, and `ShoeMgr`
- Core model unit tests

Exit criteria:

- Build passes.
- Deck and shoe tests pass.
- No JavaFX dependency is required by core model tests.

### Milestone 2: Hand Scoring and Dealer Round

Deliverables:

- `Hand`
- Ace and soft-hand logic
- Natural Blackjack logic
- Dealer strategy
- Non-interactive round analyzer

Exit criteria:

- Scoring and round-outcome tests pass.
- A complete round can run without UI.

### Milestone 3: JavaFX Playable MVP

Deliverables:

- Main table scene
- Programmatic card fronts
- Existing card-back and felt assets
- Dealer hole card
- Interactive Hit and Stand
- Score badges
- Round result dialog

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

Exit criteria:

- Each selected enhancement has its own approved behavior and tests.

## Approval Gate

Do not generate implementation code until this document is reviewed and the following are approved:

1. Baseline scope
2. Blackjack payout
3. Dealer soft-17 rule
4. Double, surrender, and split restrictions
5. Default shoe size, felt, and card back
6. Final architecture preference: preserve lesson architecture or use clean model/view separation
