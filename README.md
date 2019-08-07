# galaxy-integration-nds

A Nintendo DS platform integration for GOG Galaxy 2.0

## Features:
- Add your Nintendo DS games to your Library, with seperate collections for owned and local ("installed") games.
- Owned games import is based on a text list, which can accommodate the addition of physical games without requiring dumping.
- Specify a folder for "local games" and path to an emulator, and these games are considered "installed" and can be launched via Galaxy.
- Support for adding games with multiple entries from more than one region or revision.

## How to Install:

1) Download the .zip from the [Releases section](https://github.com/TBemme/galaxy-integration-nds/releases), and extract the contents into your installed plugins folder. If you are on Windows, it will be at `%localappdata%\GOG.com\Galaxy\plugins\installed`

2) Configure the plugin by editing the `config.py` file, and providing the desired paths and files.

### The games list (NDS-list.txt)

Please provide a .txt file, with one game title on each line. These names must match exactly the format of the No-Intro .dat file you are using, the included `NDS-list.txt` contains an example. By default, the plugin will look in its own directory for this file, but you can specify a different path in `config.py`

This list can be generated by:
- Comparing your dumped games against the .dat with a rom manager, and creating a 'have' list. The simplest to use in my experience is [Rom Center](https://www.romcenter.com/), but if you are not familiar with the program, please carefully note the warning to only use on files you have backed-up. Rom Center can also allow you to create a list without renaming the files if desired.
	- If using Rom Center, please uncheck the "Add description" box when creating the report. For Romulus, use the 'description' column only, without column titles. clrmamepro 'have' lists should work with default settings.
- If your dumped games are already named using the No-Intro format, you can use the included `create_NDS-list.bat` to automatically create the list for you. Put it into a directory with all your games, and open the file.
- Creating a text file, and putting the titles of the games you wish to add to your Library, one per line. Use the [DAT-o-MATIC search](https://datomatic.no-intro.org/?page=search) or open your .dat file in a browser or text editor to search for your title (for example, "Ace Attorney") and add the game name (in this case, "0395 - Phoenix Wright - Ace Attorney (Europe) (En,Fr)")

### The games index (NDS.dat)

This plugin uses a No-Intro Nintendo DS .dat file as an index to prepare a game id and title, in order to add it to your Library. Both numbered and not numbered formats are supported. By default, the `NDS.dat` in the plugin directory uses the numbered format. If desired, an alternate .dat and path can be specified in `config.py`.

The game titles on the game list MUST match the full names in the .dat being used.

Two recent .dat files (numbered and non numbered) are included, but more specific ones may be downloaded from [datomatic.no-intro.org](https://datomatic.no-intro.org)

### Local/"Installed" games and emulation

Using `config.py`, you can specify a path to a directory of games that will be considered as local games/Installed by Galaxy. You may also specify a path to the executable of the emulator of your choice.

Please note that, in order to support launching them with this plugin, the games must be .nds files (not inside archives), and they must be named according to the .dat you have used.

## Known Issues/Caveats:
- DSi exclusive games/DSiWare are not currently supported.
- Titles of more than 100 characters may cause an error.
- Titles that contain a '(' may truncate early, and in one case cause an error.
	- The above two issues affect a very small number of titles (33 and 4 respectively out of a current 7166 indexed), largely Japanese software titles. If you wish to add one of these, a possible solution is to manually edit the text list and .dat files to shorten them (as long as the list and "rom name" field minus the file extension in the .dat match.)
- The game_id is generated based on the internal serial of game, and there does not seem to be a way to add a single game_id to your Library twice. As far as I am aware, there are 3 cases of unexpected internal serials where several European Pokemon releases have internally US region serials. In this case, it would mean you cannot add both regions of one of these games to your Library. Affected titles are Platinum, SoulSilver and HeartGold.
	- If you really wanted to add both for one game, you could get around this by manually editing the .dat as mentioned above. In this case, only the rom serial field of the European release needs to be changed (example, change IPGE to IPGP).
- When first connecting, there is an X displayed next to Installing & Launching, however Launching is a feature supported by this plugin.
- Currently, the Nintendo DS games category lacks a title when using the "Group by Platform" Library display. This does not seem able to be fixed by the plugin. In a possibly related issue, if more than one plugin that lacks "Group by Platform" title is installed at one time, the All Games tab of the Library will no longer work correctly when on Group by Platform view.
	- A workaround is to use a grouping other than by Platform.
- I don't know or have a way of testing if this will work on macOS. The fallback/default paths use a Windows structure, so it will require all paths specified in `config.py`.

### Games database related issues
- Please keep in mind that the games database that all games must draw information from is still a work-in-progress by the GOG staff. It is not currently able to be edited by the plugin, or by users. So particularly for platforms that are only available via community-integration at this time, information on the game pages may be sparse, images may be missing or have cropping issues, and games may take a while to appear before they are linked to an entry.
- Sometimes games are successfully added (which can be verified in the logs file), but not visible in the Library. Or, some games were initially visible, but then disappeared. All games get information from GOG's game database. This includes being linked to a game title within the database and a `"visible_in_library":` field. This is not editable by the plugin. If this field is set to `False`, or in some cases it cannot find a game entry to link to, the game will not be visible. Wait some time (at least 24 hours, and restart Galaxy) to see if the game appears, or you can try filing a bug report with GOG.
- A game was added to the Library and is visible, but it is not linked to the correct one/it appears as 'Unknown Game'. If you know what it is, you can use the Edit game window to search for the correct title to link it to. Especially if it is a Japanese title, they will often only be searchable under an English release title. If it is one of your 'local games', you can try launching the game to identify it.
- On a game's page, an error message appears saying "Sorry, we couldn't load the data. Retry." keeps appearing. Logs indicate this is an error from failing to retrieve Activity Feed data for that particular game (this platform does not support any activity tracking), and a number of similar bugs have been reported to the Galaxy issue tracker. As before, wait some time (at least 24 hours, and restart Galaxy) to see if the problem is fixed, or you can try filing a bug report with GOG.

## Acknowledgements

- Thanks to @AHcoder, whose [PS2 Galaxy integration](https://github.com/AHCoder/galaxy-integration-ps2), was the starting point for this plugin.
- Thanks to @TheMas3212 for getting me started with BeautifulSoup, and helping to answer my many (many) questions.