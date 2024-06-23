# PLN Wagering API Documentation

## General Information

Lines are partitioned by tournament, and each document has:
- `tournamentId`: Unique UUID (string)
- `tournamentName`: Name of the tournament (string)
- `sportId`: UUID unique to sport (string)
- `league`: Name of the league (string)
- `tournamentStartTime`: YYYY-MM-DDTHH:MM:SSZ
- `tournamentLines`: List of markets

Each market has:
- `marketId`:
  - PLN property dependent
- `marketType`: Description matching the `marketId` (string)
- `marketStatus`:
  - "Open": Market is available/should be shown
  - "Closed": Market is not available/should not be shown
  - "Suspended": Market may be open again (should be shown but not allow bets)
- `lines`: List of lines in the market

Each line has:
- `lineID`: UUID unique to line (string)
- `description`: String identifying key line information
- `price`: American odds (marginated)
- `raw_price`: Raw, unmarginated decimal price
- `startTime`: Expected starting time for matches relevant to line (YYYY-MM-DDTHH:MM:SSZ)
- `status`:
  - "Enabled": Line should be available for wagering
  - "Suspended": Wagers should not be accepted for this line
  - "Pending Result": Wagers should not be accepted, match has ended and awaiting grading
  - "Resulted": Line has been graded
  - "ReResulted": Line has been re-graded
- `result`:
  - "Won": Line description met
  - "Lost": Line description not met
  - "Void": No result achieved
  - "Push": The result was neither "Won" nor "Lost"
- `matchId`: UUID of the match the line relates to (string)
- `createdTime`: When the line was first added to the database (YYYY-MM-DDTHH:MM:SSZ)
- `lastUpdatedTime`: When the line was last updated (YYYY-MM-DDTHH:MM:SSZ)
- `lineInfo`: Unstructured object allowing for more information to be inserted
- `lineMovements`: List of line movements

Each line movement has:
- `timestamp`: When the change was made (YYYY-MM-DDTHH:MM:SSZ)
- `user`: Internal user that made the change (string)
- `reason`: User's entered reasoning for change (string)
- `changes`: List of changes that were made

Each change has:
- Name of changed field
- Object with
  - `old`: Value of field before change
  - `new`: Value field changed to

## Status Definitions

- **Enabled**: Line should be available for wagering.
- **Suspended**: Wagers should not be accepted for this line.
- **Pending Result**: Wagers should not be accepted, match has ended and awaiting grading.
- **Resulted**: Line has been graded.
- **ReResulted**: Line has been re-graded.

## Result Definitions

- **Won**: Line description met.
- **Lost**: Line description not met.
- **Void**: No result achieved.
- **Push**: The result was neither "Won" nor "Lost".

## Market Status

- **Open**: Market is available/should be shown.
- **Closed**: Market is not available/should not be shown.
- **Suspended**: Market may be open again (should be shown but not allow bets).

## Notes

The detailed schema definitions for the models referenced in this documentation can be found in the accompanying JSON file. The definitions include detailed properties for:
- LineModel
- MarketModel
- TournamentLine
- LineMovement
- LineUpdate
- InputTournamentLine

Please refer to the swagger generated JSON file [here](https://plnapi-prod.azurewebsites.net/swagger/) for complete details on each model.

# World Putting League Specifics:

## WPL Market Types

Market ID "001":
- Description: Outright Winner
  - Format: "FirstName LastName Outright Winner"

Market ID "002":
- Description: H2H Moneyline
  - Format: "Moneyline FirstName LastName (FirstName LastName v FirstName LastName)"

Market ID "003":
- Description: Over/Under (Hole in one, par, bogey, etc.)
  - Format: "{Over/Under} {line} {stroke type} round {round number} hole {hole number}: {subject}"

### Over/Under
- **Allowed Values**: `Over`, `Under`
- **Description**: Indicates whether the bet is over or under the specified line.

### Line
- **Allowed Values**: Any positive number (integer or decimal).
- **Examples**: `0.5`, `1`, `2.5`
- **Description**: The specific amount of the stroke type that you are betting over or under on.

### Stroke Type
- **Allowed Values**: Any text describing the bet type.
- **Examples**: `HIO`, `pars`, `bogey`, `minutes to play hole 2`, `fist pumps`
- **Description**: The type of bet being placed. This can be any descriptive text.

### Round Number
- **Allowed Values**:
  - Single round: Any positive integer (e.g., `1`, `2`).
  - Multiple rounds: A list of positive integers enclosed in square brackets (e.g., `[1,2,3]`).
  - All rounds: `-1`
- **Description**: The round or rounds the bet applies to.

### Hole Number
- **Allowed Values**:
  - Single hole: Any positive integer between 1 and 18 (e.g., `1`, `4`).
  - Multiple holes: A list of integers between 1 and 18 enclosed in square brackets (e.g., `[3,4,8]`).
  - All holes: `-1`
- **Description**: The hole or holes the bet applies to.

### Subject
- **Allowed Values**: Any text representing the subject of the bet.
- **Examples**: `Joey Graybeal`, `Olivia Prokopova`, `Field`
- **Description**: The name or identifier of the subject of the bet.
