# Election Night Big Board

[An interactive county-level election map](https://texas-election-board.onrender.com/) for the 2026 Democratic and Republican primaries in Texas. Heavily inspired by the "big boards" used on TV by Steve Kornacki and John King. 

![A screenshot of the map](docs/screenshot.png)

## Local Dev

To run locally:
1. `npm install -g serve`
2. `serve . -p 8080` 
3. visit http://localhost:8080/ on your browser.

## Candidates Config

The provided CSV template (`candidates_template.csv`) file shows what data is needed for each candidate in the race:

- `candidate_id`: A unique ID provided for this candidate, will be referenced in the results file.
- `party`: D or R for Democratic or Republican candidate
- `name_first`: Candidate's first name
- `name_last`: Candidate's last name
- `incumbent`: `true` or `false` if this candidate should be flagged as an incumbent
- `photo_url`: optional URL to an image to show as a candidate's profile picture on the right side summary
- `hex_color`: color this candidate will be shaded as on the big board

## Results File

Check out the template (`results_template.csv`). Each row of the results file has:

- `candidate_id`: A unique ID for a candidate
- `county`: County name in all caps, used for joining to the map make sure to include a `STATEWIDE` county which is the results for the whole state combined.
- `votes`: Integer number of votes this candidate has received
- `pct_vote`: 0-100 float that shows the percent of the vote the candidate has received.
- `pct_reporting`: 0-100 float representing the percent of the precincts that has been reported, this should be the same for all candidates in a particular county.
- `called`: `true` or `false` if this county or statewide result has been called for a particular candidate. If multiple candidates receive a call in the `STATEWIDE` result, then instead of `WINNER ✓` it will display `ADVANCED TO RUNOFF ✓` on the right side summary.

## Data Source and Map Config

In `index.html` edit these variables to configure your data source and map settings

Edit the following variables to point to the CSV file you have hosted for the backend, or supply `null` and the map will fallback to some hard coded test data. I would recommend serving these CSVs from an AWS S3 bucket or a Google Cloud Storage bucket. 
- `const CANDIDATES_URL` 
- `const RESULTS_URL` 

`const LIVE_UPDATES` being set to `true` will make the app refresh with new data every `const REFRESH_SEC` seconds. When `LIVE_UPDATES` is `true` precent reported will max out at >99.9% and never show 100% even if 100 is supplied in the results CSV.

`LIVE_UPDATES` set to `false` will no longer refresh automatically, will display "FINAL RESULTS" on the top right, and will allow 100% reporting to be shown.
