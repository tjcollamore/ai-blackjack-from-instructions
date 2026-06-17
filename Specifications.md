# Blackjack Project Specification

## Purpose

Build a JavaFX Blackjack game that is also usable as a teaching project for students preparing for AP Computer Science A.

The final application must be a playable one-player Blackjack game against a dealer. The codebase should emphasize clear Java fundamentals, testable business logic, and clean separation between game rules and JavaFX UI so the game can later be ported to mobile technology without rewriting the core model.

## Repository and Build

Use the existing `pom.xml`.

The project uses:

- Java 17
- Maven
- JavaFX Controls 18.0.2
- JavaFX FXML 18.0.2
- JUnit Jupiter 5.9.2
- `Main` as the JavaFX application entry point

Expected project layout:

- Production Java: `src/main/java`
- Resources: `src/main/resources`
- Tests: `src/test/java`

Expected commands:

- Verify Maven: `mvn -version`
- Run tests: `mvn test`
- Run the application: `mvn clean javafx:run`

## Existing Files and Assets

Use existing repository assets. Do not regenerate or replace them unless specifically requested.

### Java Scaffolding

`src/main/java/CardFaceCreator.java` already exists and should be treated as supplied scaffolding.

Use it to render card fronts and backs:

- `createFrontFace(Suit suit, Rank rank)`
- `createBackFace()`

`CardFaceCreator` is not a student assignment. Students may inspect it, but they are not expected to write its card-layout algorithm from scratch.

The file includes attribution to its original source. Preserve that attribution and record any substantial changes in the file.

### Felt Backgrounds

- `felt_green.png`
- `felt_blue.png`
- `felt_burgundy.png`

Default felt: `felt_green.png`.

### Card Backs

- `card_back1.png`
- `card_back2.png`
- `card_back3.png`
- `card_back4.png`
- `card_back5.png`

Default card back: `card_back4.png`.

### Chip Images

- `chip_white.png` for $1
- `chip_pink.png` for $2.50
- `chip_red.png` for $5
- `chip_green.png` for $25
- `chip_black.png` for $100
- `chip_purple.png` for $500
- `chip_orange.png` for $1,000

### Other Assets

- `gear.png`

All resources must be loaded from the application classpath. Do not use absolute local file paths or paths beginning with `/src/main/resources/` in application code.

## Required Baseline

The baseline game must include:

- JavaFX desktop interface
- One player against one dealer
- Eight-deck shoe
- Correct card uniqueness within each deck
- Cut-card reshuffling between rounds
- Correct Blackjack hand scoring, including soft Aces
- Natural Blackjack detection
- Dealer hole card behavior
- Dealer peek for Blackjack
- Player actions: Hit, Stand, Double, Surrender, and Split
- Maximum of four player hands after splitting
- Wagers, payouts, and bankroll tracking
- Wager-selection dialog
- Score badges and bankroll display
- Split-hand badge and review UI
- Existing felt, card-back, chip, and gear assets
- Unit tests for non-UI business logic
- Student-facing instructions for each implementation milestone

The baseline should display the gear icon, but a working settings dialog is not required. Game options should still live in a centralized configuration class so a settings dialog can be added later.

## Game Rules

### Initial Deal

- The player and dealer each receive two cards.
- Deal alternates between player and dealer.
- The dealer's second card is hidden.
- The hidden card remains hidden until all player hands are complete, except when it must be revealed to prove dealer Blackjack.

### Natural Blackjack

- A natural Blackjack is exactly two cards totaling 21 on an original, non-split hand.
- A two-card 21 created after a split is an ordinary 21, not a natural Blackjack.
- If both the dealer's original hand and the player's original hand have natural Blackjack, the result is a push.
- If only the player has natural Blackjack, the player receives the Blackjack payout.
- If only the dealer has natural Blackjack, the player loses.

### Dealer Peek

- The dealer peeks for Blackjack when showing an Ace or ten-value card.
- If the dealer has Blackjack, resolve the round immediately before offering late surrender or other player actions.

### Dealer Play

- Dealer hits on soft 17.
- Dealer draws until reaching a hard 17 or higher.
- Dealer does not draw if all player hands have busted or surrendered.
- Dealer plays once after all active player hands are complete.
- The dealer's final hand is compared independently against every player hand.

### Player Actions

Hit:

- Allowed until the hand busts, reaches 21, or another rule ends the hand.

Stand:

- Ends action for that hand.

Double:

- Allowed only as the first action on a two-card hand.
- Allowed on any two-card total.
- Allowed after a split except on split Aces.
- Deducts an additional wager equal to the hand's current wager.
- Draws exactly one additional card and then automatically stands.

Surrender:

- Late surrender only.
- Allowed only as the first action on a two-card hand.
- Allowed after splitting.
- Returns half the wager.
- Not offered if the dealer has Blackjack after peeking.

Split:

- A hand may split when its first two cards have equal Blackjack values.
- Mixed ten-value cards may split, such as Jack and Queen.
- Splitting deducts an additional wager equal to that hand's current wager.
- At most three splits are allowed, producing at most four player hands.
- Each split hand has independent cards, wager, action state, result, and payout.
- Split Aces receive exactly one additional card each.
- Aces may not be re-split.
- A hand cannot split or double if the bankroll cannot cover the additional wager.

### Money Rules

- Starting bankroll: $400
- Table minimum: $15
- Table maximum: $1,000
- Wagers are selected in $5 increments.
- Blackjack payout: 3:2.
- Use integer cents or another exact representation for authoritative money state.
- Format displayed money as US currency.
- If the bankroll is below the table minimum, ask whether the player wants to withdraw another $400 from the ATM.
- The bankroll must not silently become negative.

### Shoe Rules

- Use an eight-deck shoe by default.
- Each deck contains exactly 52 unique cards.
- Every remaining card in the shoe must have equal probability of being drawn.
- The cut-card threshold is 12 remaining cards per deck.
- Crossing the cut-card threshold schedules a reshuffle after the current round.
- Do not reshuffle during an active round merely because the cut card has been crossed.
- Mid-round shoe exhaustion must be handled by one documented policy instead of crashing or returning an unexplained `null`.

## Core Model

### `Rank`

Represent ranks with an enum.

Each rank provides:

- Full display name
- Short display name
- Blackjack value

Values:

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

A card consists of:

- One `Rank`
- One `Suit`

A card provides:

- Rank accessor
- Suit accessor
- Blackjack value
- Ace detection
- Human-readable text such as `Ace of Hearts`

Cards should not randomly create themselves in the final design. Random card generation is only an early teaching exercise.

### `Deck`

Responsibilities:

- Construct exactly one card for every rank and suit combination.
- Report the number of remaining cards.
- Remove and return a random remaining card.
- Handle an empty deck predictably.

### `Shoe`

Responsibilities:

- Own the configured number of decks.
- Report the total number of remaining cards.
- Draw cards with equal probability across all remaining cards.
- Encapsulate decks so gameplay classes do not access decks directly.

Weighted random deck selection is acceptable and useful for teaching. A flat shuffled shoe is also acceptable if it preserves equal probability and clean ownership.

### `ShoeMgr`

Responsibilities:

- Own the active shoe.
- Draw cards for game participants.
- Detect the cut-card threshold.
- Replace the shoe between rounds when required.
- Handle the documented mid-round shoe-exhaustion policy.

No gameplay class other than `ShoeMgr` should manage reshuffling.

### `Hand`

A hand owns card and scoring state.

It must compute:

- Current score
- Whether the hand is soft
- Whether the hand is 21
- Whether the hand is busted
- Whether the hand is a natural Blackjack
- Whether the hand is eligible to split
- Whether the hand contains two Aces

Ace scoring:

1. Count every Ace as 11.
2. While the score exceeds 21 and at least one Ace is still counted as 11, reduce the score by 10.
3. A hand is soft when at least one Ace remains counted as 11.

UI state should not live in `Hand`. JavaFX views should render hand state from the model.

## Round Flow

`Round` coordinates one dealer hand and one or more player hands.

Required flow:

1. Create the initial player hand and dealer hand.
2. Deal two cards to each, alternating participants.
3. Keep the dealer's second card hidden.
4. Peek for dealer Blackjack when appropriate.
5. Resolve original natural Blackjacks.
6. Collect or confirm the wager before player action.
7. Let the player complete every active hand, including splits.
8. Reveal and play the dealer hand once if any player hand still requires comparison.
9. Analyze each player hand against the dealer.
10. Apply each payout to the bankroll.
11. Present results.
12. Ask whether to deal another round.
13. Check the cut card before the next round.

This two-pass player-then-dealer flow is required for correct split behavior.

## Strategies

Use a `HandStrategy` interface for participant play.

Required implementations:

- Dealer strategy
- Interactive player strategy

The strategy interface should allow a later basic-strategy player without changing `Round`.

## Betting and Bankroll

### `Bankroll`

Responsibilities:

- Store funds exactly.
- Deduct wagers.
- Deposit payouts.
- Report whether an additional wager is affordable.
- Format values as US currency.
- Notify or expose data to update the UI bankroll display.

### `Wager`

Each active player hand needs independent wager state.

Responsibilities:

- Store the hand's wager.
- Track whether the hand doubled or surrendered.
- Record or compute the hand's payout.
- Support win, loss, push, Blackjack, surrender, double, and split-related wager changes.

Avoid one shared mutable `Wager` across all split hands.

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

The chip-conversion algorithm must return a valid combination for every supported payout. A naive greedy algorithm fails for amounts such as $4.00 when the $2.50 chip is considered too early. Tests must protect the corrected behavior.

## JavaFX UI

### Layout

The UI should use a portrait, handheld-style layout even though the first implementation is JavaFX desktop.

The main scene must include:

- Green felt background by default
- Dealer card area
- Player card area
- Dealer and player score badges
- Bankroll display
- Gear icon
- Active split-hand badge when multiple player hands exist

A fixed-size window is acceptable if it preserves the mobile-like layout.

### Card Rendering

- Use `CardFaceCreator.createFrontFace(Suit suit, Rank rank)` for face-up cards.
- Use `CardFaceCreator.createBackFace()` for the dealer hole card.
- Keep card dimensions centralized through `CardFaceCreator`.
- Verify glyph rendering on Windows during UI testing.
- If glyph rendering becomes a portability problem, replace `CardFaceCreator` internals without changing the public card-rendering API.

### Dialogs

Required dialogs:

- Action selection
- Bet selection
- Round result / deal again
- ATM replenishment prompt

Action dialogs must show only legal actions for the current hand and bankroll.

### Split-Hand UX

- Show one player hand at a time to preserve screen space.
- Display a badge such as `2 of 3` for the active split hand.
- After dealer resolution, allow the player to review split hands and understand why each won, lost, pushed, or surrendered.

### Chip Preview

The bet dialog should:

- Display the selected wager.
- Show a visual stack or arrangement of existing chip images.
- Update the preview whenever the wager changes.

### Settings

- Display the existing gear icon.
- A working settings dialog is not required for the baseline.
- Keep game options centralized so a settings dialog can be added later.

## Student Instruction Requirements

This repository should support teaching the project in phases.

For each implementation milestone, create or maintain student-facing instructions that include:

- The programming concepts being practiced
- Required classes or files
- Clear checkpoints
- Suggested tests or manual checks
- Vocabulary such as constructor, method, enum, interface, list, and object composition
- Notes about temporary simplifications and when they are removed

The instructions should teach the project progressively:

1. Java fundamentals: random cards, loops, input, `switch`, `toString`
2. Collections and hand scoring
3. Decks, shoes, equal-probability drawing, and cut-card logic
4. Refactoring, interfaces, strategies, and enums
5. JavaFX application structure, layout, resources, and rendering
6. Betting, bankroll, dialogs, Double, and Surrender
7. Splitting, multiple hands, and two-pass dealer flow
8. Tests, chip conversion, and UI polish

Each checkpoint should compile and run before moving on.

## Testing Checklist

### Card and Deck

- Every deck starts with 52 cards.
- Every rank/suit combination appears exactly once per deck.
- Drawing removes a card.
- Drawing all cards produces no duplicates within one deck.
- Drawing from an empty deck follows the documented policy.

### Shoe

- Shoe size equals `52 * deckCount` initially.
- Every draw reduces shoe size by one.
- Draw selection is unbiased across remaining cards.
- Cut-card crossing schedules reshuffling only after the round.
- A new shoe restores the expected size.
- Mid-round shoe exhaustion follows the documented policy.

### Hand Scoring

- Number cards score correctly.
- Face cards score as 10.
- One Ace can score as 11 or 1.
- Multiple Aces adjust correctly.
- Soft-hand detection is correct.
- Bust detection is correct.
- Natural Blackjack requires exactly two original non-split cards.
- A multi-card 21 is not Blackjack.
- A two-card 21 after split is not Blackjack.

### Dealer and Round

- Initial deal order and hidden card are correct.
- Dealer peeks for Blackjack when showing an Ace or ten-value card.
- Dealer Blackjack is handled.
- Player Blackjack is handled.
- Both Blackjacks push.
- Dealer hits soft 17.
- Dealer stands on hard 17 or higher.
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
- When bankroll is below the table minimum, the player can choose whether to withdraw another $400.

### Splitting

- Equal-value cards can split.
- Maximum split count is enforced.
- Split Aces receive one card and stop.
- Aces cannot be re-split.
- A split hand's cards, wager, score, and result remain independent.
- Dealer card visibility remains correct throughout all player hands.
- Split-hand review after dealer resolution is understandable.

### Chip Conversion

- $2.00 uses two white chips.
- $4.00 does not incorrectly use a pink chip.
- $4.50 uses one pink and two white chips.
- $7.50, $8.00, $165.00, and $167.50 produce valid totals.
- Every supported payout converts to chips with no remainder.

### JavaFX

- Existing assets load from the classpath.
- `CardFaceCreator` renders face-up cards.
- `CardFaceCreator` renders the selected card back for the dealer hole card.
- Score badges update and reset between rounds.
- Bankroll display updates after deductions and payouts.
- Action dialogs show only legal choices.
- Bet chip preview updates with the selected wager.
- Split-hand navigation is understandable.
- Layout is usable in a portrait, handheld-style window.
