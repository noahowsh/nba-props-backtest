# nba-props-backtest

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/noahowsh/nba-props-backtest/blob/main/props_backtest.ipynb)

Check NBA player props against the closing line, using real sportsbook history since 2023. Two modes, one script:

- **player:** every closing line for one player and market beside what he actually recorded, his over rate against the price, and what happened when the line moved.
- **idea:** test a league-wide angle ("points overs for road underdogs") in one call, with the sample size, a 95% interval, and the market's implied and margin-free probability.

```bash
pip install -r requirements.txt
export DUNKAPI_API_KEY=your_key      # free: https://dunkapi.com/signup (500 credits a month)

python props_backtest.py player "Jalen Brunson" --market player_points --since 2025-10-01
python props_backtest.py idea --market player_points --side over --home-away away --role underdog --since 2024-10-01
```

Example output (Nov 2026):

```
Jalen Brunson player_points, 107 graded games since 2025-10-01
  over rate 49.5% vs 53.2% implied by the over price
  average line 26.8, average actual 26.5, mean margin -0.2

points overs, filters {'league': 'nba', 'from_date': '2024-10-01', 'home_away': 'away', 'team_role': 'underdog'}
  12,788 results across 1,685 games and 533 players
  hit rate 48.6% (95% interval 47.8% to 49.5%)
  market implied 53.1%, fair (margin removed) 49.8%
```

## The data

[DunkAPI](https://dunkapi.com/data/player-props?utm_source=github&utm_medium=readme&utm_campaign=nba-props-backtest) holds NBA prop lines by sportsbook from May 2023: points, rebounds, assists, threes, the combos, blocks, steals, and double- and triple-doubles, with the closing line and the opening line where held. `get_prop_history` grades each closing line against the box score; `get_prop_backtest` runs the aggregate. WNBA and men's college props work the same way with `league="wnba"` or `"ncaam"`.

Credits: a player search costs 1, each page of history 10, and a backtest 25.

## Ideas to extend it

- Loop over a roster and rank players by margin against the closing line.
- Join to `get_player_games` for minutes and usage, and model the line yourself.
- Compare the opening and closing line to see which moves held up.

This is a research tool. A past hit rate describes a sample; it is not a pick.

Uses the [dunkapi](https://pypi.org/project/dunkapi/) Python client (no dependencies). DunkAPI is also a hosted MCP server for Claude, ChatGPT and Cursor: https://dunkapi.com/mcp?utm_source=github&utm_medium=readme&utm_campaign=nba-props-backtest

MIT licensed.
