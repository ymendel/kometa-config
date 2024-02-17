# Plex Meta Manager

Saving my configuration for [Plex Meta Manager (PMM)](https://metamanager.wiki/).

This is very much in-progress. I've just been tweaking it too much and not saving it in any good way.

Useful links:

- https://metamanager.wiki/, of course
- https://github.com/meisnate12/Plex-Meta-Manager
- https://github.com/meisnate12/Plex-Meta-Manager-Configs
- https://github.com/meisnate12/Plex-Meta-Manager-Configs/blob/master/nwithan8/tips_and_tricks.md
  - reproduced somewhat below; being adjusted

TODOs:
- move over all collections from config.yml to separate files
  - Movies:
    - still want these?
      - studio
      - universe
  - TV:
    - still want these?
      - network
- spooderman playlist
- remove default recommendations in libraries (all this is from the libraries manage in Plex)
  - Movies:
    - recently released
    - recently added (can stay)
    - library playlists
    - top movies in (genre)
    - top movies by (actor or director)
    - top unplayed
    - seasonal
    - recently played
  - TV:
    - recently released episodes
    - recently added (can stay)
    - library playlists
    - start watching
    - rediscover
    - more from (network)
    - more in (genre)
    - top rated
    - recently played episodes
  - note these seem to be the defaults, from an "other videos" collection:
    - continue watching
    - recently added
    - library playlists
    - unplayed
    - recently played
- more collections, once the current stuff is taken care of

## Consideration / Concepts

### Collections

#### Ordering

- Use sort_title to promote and demote certain collections
    - start with `+` to promote to top of library
    - start with `~` to demote to bottom of a library
    - can use A-Z to additionally organize collections via levels
- Recommended to start with a number corresponding to a category
    - `1` above `2` above `3` etc.
- Use `+` and `~` to promote and demote secondarily
    - `1_++_` above `1_+_` above `1__` above `1_~_` above `1_~~_`
    - `1_+_` above `1_~_` above `2_+_` above `3__`, etc.

Prefixes in use:

```
# Prefixes for Collections:
#   010_+ = New
#   020_+ = People (in memoriam)
#   030_+ = Holidays
#   040_+ = Charts
#   050_+ = Awards
#   060_+ = Collections / Franchises
#   070_+ = Genres
#   080_+ = Decades
#   090_+ = People
#   100_+ = Studios / Networks / Streaming
#   110_+ = Countries
#   120_+ = General / Unspecified
```

These collection uses a combination of all these prefixes, and are always sorted in a specific way. Take this example from the Holiday template.

```
default:
  level: ""
  sort_title_name: <<collection_name>>
sort_title: 030_+<<level>>_<<sort_title_name>>
```

Affect on sorting, in order:

1. Category:
  - e.g. "New" collections (010) will always be above "Award" collections (050)
2. Level:
  - e.g. `+++` will always be above `+`, `~~~` will always be below `~`, "A" will always be above "B"
3. Alphabetical:
  - Finally, collections are sorted alphabetically. This can be adjusted with the `sort_title_name`, which defaults to the collection name.

#### Visibility

- `visible_library`, `visible_home` and `visible_shared`
    - `true`, `false` or [schedule](https://github.com/meisnate12/Plex-Meta-Manager/wiki/Schedule-Detail) available for
      all options
    - `visible_library`: Will be visible in the Recommended tab for a specific library (and in the Library tab if you have chose to intermix collections in with library items)
    - `visible_home`: Will be visible on your Home page (what appears on your Home page is determined by what libraries you have pinned)
    - `visible_shared`: Will be visible in the Home page for other users (what appears on their Home page is determined by what libraries they have pinned)
    - Collections are *ALWAYS* visible in the Collections tab for a specific library (not controlled by the `visible_library` option)
    - Advice: Treat `visible_library` the same as `visible_home`/`visible_shared`. If you want something to appear as a recommendation on the Home page, you probably want it to appear in the specific library's Recommendations tab as well.
- Visibility does not dictate when a playlist/collection is updated:
    - When setting visibility to a certain time-frame, make sure the playlist/collection itself is scheduled to update at least one day longer than the visibility time-frame. Otherwise, the playlist/collection will not update again and the visibility will not change.
        - Example: Halloween collection, `visible_shared: (10/01-10/31)`, `schedule: (10/01-11/01)`
    - You also need to schedule the playlist to update on the first day of visibility. Otherwise, the playlist will not update and the visibility will not change.
    - If you can spare the processing, the best thing to do would be to have the playlist/collection update every day, so you don't have to worry about specifically updating it to trigger visibility changes.

#### Scheduling

- Save time by updating lesser-important collections and playlists less frequently:
    - ex. `schedule: weekly(wednesday), weekly(sunday)`
- Don't need to schedule a `smart_filter` because Plex auto-updates the list itself
- Scheduling can take multiple criteria
  - Ex. yearly(02/21), all[weekly(friday),range(02/21-03/31)], yearly(04/01)
    - Run on Feb. 21 every year (regardless of day of week), then run every Friday between Feb. 21 and Mar. 31, then run Apr. 1 (regardless of day of week)
    - Useful for awards or things that need to appear and be updated only for a certain time frame of the year

