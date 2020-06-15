# WORKING TITLE: Value Added to School Selection

<p align="center">
  <img src="assets/bsu_logo.png" width="300" />
</p>

This repository is related to the economic research problem Kyle Brookman is pursuing for his thesis at Boise State University for the MS in Economics.

## Research Question: What is the value added to returns to labor from attending more selective academic institutions

## STATUS: MERGING `recruit247.xlsx` with `PLAYER_nfl.xlsx` to create `merged.csv`

---
**Merge strategy:**

`recruit247.xlsx`
- keep all variables in recruit247
- This is high school recruits with individual and school data
> * `playerRec` = individual level who needs data merged to
> *	`UNITID_col` = code for college on record of attending
> * `genPosRec` = a general position code. It’s less specific than
>> *	`posRec` = high school recruit position
>> *	`posCol` = college position

`PLAYER_nfl.xlsx`
- Every NFL player on a roster since 2002 (**ERROR: TIME FRAME IS EARLIER  | The variable "start" is the year the player started in the NFL. However, the data itself is taken from the seasons spanning 2002 - 2019. Anyone with a start year on or before 2004 can be dropped because there wont be anything from the recruit data to merge with. 2005 will likely see some mergable observations trickle in and start picking up in 2006.**)
- Merge **all columns** onto “recruit247” (**VALIDATE: MERGED ALL?  | This was ment to say keep all variables from the PLAYER_nfl. Merge on individual player as in the other datasets.**) 
- recruit data starts in 2002 so no one will show up in the PLAYER_NFL until at least 2004 (**ERROR: TIME FRAME STARTS 2000  | That's my bad, I should have dropped those before hand. There are various quality control and specification reasons to drop recruits in 2000-2001. I have a hard time parting with the data but lets drop observations for years 2000 and 2001 in recruit247 data. **)
- The variable start is the year they started in the NFL so you should be able to filter out anything before 2004 (**VALIDATE: REMOVED ALL BEFORE 2000  | IF we start with recruit data on year 2002, then you can safely/conservatively remove all players with a start date = 2003 or earlier because of the lag time spent in college. Keeping 2004 in PLAYER_nfl is playing it VERY safe because it is very rare someone spends just two years in college (e.g. 2002 and 2003) and then head directly into the NFL in 2004, but it is possible. It is more typical someone that a 2002 recruit shows up in the outcome data in 2006. Also, there isn't a hard time frame in which the process from recruit to outcome variables happens, but in the case of the NFL Draft and NFL Combine outcome data, it would be safe to say that if the player doesn't show up in those data within 7 years then you can conclude that the recruit is not mergable on those datasets. For example, for the 2010 recruiting class, you can safely limit the nfl_c and nflDraft_02_20 possible merges between (2012,2017). The PLAYER_nfl data isn't as easy to make that narrow time window assumption because a recruit can potentially start in the league any year after leaving college. Theoretically, someone who left college in 2008 can start in the NFL in 2018. But it's safe to say that high school recruits in the 2008 class will never be in PLAYER_nfl data in 2009 or before. I hope this makes sense and is helpful! **)
> *	`UNITID` is coded as the most recent school
>> * May match with `UNITID_col` or `UNITID_comRec` in `recruit247`
> * `pos1` and `pos2` in `PLAYER_nfl` may help with the merge

`nfl_c.xlsx`
- NFL combine data
> * `player_c` is individual level to merge with `playerRec` and `player` from the other two data sets
> * `year_c` likely coincide `start` from `PLAYER_nfl` if the player attended the NFL combine and entered the NFL in the same year
> * `dpos` is draft position in `PLAYER_nfl` which should be the same as `draftOverall_c` in `player_c`
> * `draft` in `nfl_c` is not a complete listing of the draft data
>> * There are players drafted who didn’t attend the nfl combine.
>> * The file `nflDraft_02_20` is a complete listing of all NFL draftees from 2002 -2020
> * `player` needs merged onto the recruit247 `playerRec` variable
> * `UNITID` is a school code for the last college attended/drafted from

**NOTE:** much data can be merged with `nfl_c` by the variables  `year_d` on `year_c` and `overall_d` on `draftOverall_c` as they are mutually exclusive
* same with `PLAYER_nfl` and `nflDraft_02_20` where `year_d` → `start` &  `overall_d` → `dpos`

**NOTE:**
It may be better to merge `nfl_c` → `nflDraft_02_20` → `PLAYER_nfl` before attaching to `recruit247`
* Nearly every individual who is in `nflDraft_02_20` and `nfl_c` will also be in `PLAYER_nfl` so it will be easier to exhaust all possible merging options
* Likely some will not match.
