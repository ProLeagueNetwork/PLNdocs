# PLNdocs
Documentation for Pro League Network

## General Wagering Information

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
