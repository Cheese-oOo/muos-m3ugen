
---

MUOS IPTV M3U Playlist Organizer

This Python script processes M3U playlist files, categorizes channels based on URL patterns, and organizes them into folders. It also downloads and saves channel logos from the provided URLs.

## Features:
- **Categorization**: Channels are categorized into "TV Shows", "Movies", "TV Channels", or "Others" based on the URL.
  - "TV Shows" are identified if the URL contains the word `series`.
  - "Movies" are identified if the URL contains the word `movie`.
  - "TV Channels" are identified if the URL contains the string `INSERT_SOMETHING_OTHER_HERE_THATS_NOT_MOVIES_OR_SHOWS_ETC`.
- **Organized Output**: M3U files are organized into categories such as "TV Shows", "Movies", and "TV Channels", with subfolders for each show or movie.
- **Logo Downloads**: Logos are downloaded and saved in a designated folder.
- **Clean Filenames**: Filenames are sanitized to remove special characters, and any Unicode characters are normalized to prevent issues on various file systems.
- **Blocked Domain Filter**: Any URL containing a blocked domain (`https://temp-me.xyz`) will be skipped.

## Requirements
- Python 3.x
- Requests library
- urllib3 library

You can install the required libraries using pip:

```bash
pip install requests urllib3
```

## Configuration
Update the following variables in the script to fit your needs:

- `m3u_file`: Path to the input M3U playlist file.
- `output_folder`: Path where the categorized M3U files will be saved.
- `logo_folder`: Path where the channel logos will be saved.
- `skip_domain`: Any domain URL containing this string will be skipped when processing.

## Usage

1. **Prepare your M3U file**: Ensure your M3U file is formatted properly, with `#EXTINF` lines containing the `tvg-name`, `tvg-logo`, `group-title`, and the stream URL.
   
2. **Run the Script**: Once the configuration is set, run the script:

```bash
python m3u_organizer.py
```

3. **Result**: The script will process the M3U file, categorize the channels into folders based on their type, and download logos. If a channel's URL contains the specified placeholder (`INSERT_SOMETHING_OTHER_HERE_THATS_NOT_MOVIES_OR_SHOWS_ETC`), it will be categorized as "TV Channels".

## Example Directory Structure
```
categories/
├── Movies/
│   └── Action/
│       └── Movie1/
│           └── Movie1.m3u
├── TV Shows/
│   └── Drama/
│       └── Show1/
│           └── Show1.m3u
├── TV Channels/
│   └── Sports/
│       └── Channel1/
│           └── Channel1.m3u
└── logos/
    └── Show1.png
    └── Channel1.png
```

## Functions:
### `clean_filename(name)`
This function sanitizes the `tvg-name` to create a valid filename, removing any special characters and normalizing Unicode characters.

### `clean_group_title(group_title)`
This function specifically handles the "UK" part in group titles by replacing it with "_UK" to avoid issues in folder names.

### `categorize_channel(url)`
This function categorizes the URL based on the type of content:
- `"TV Shows"`: If the URL contains `series`.
- `"Movies"`: If the URL contains `movie`.
- `"TV Channels"`: If the URL contains the string `INSERT_SOMETHING_OTHER_HERE_THATS_NOT_MOVIES_OR_SHOWS_ETC`.
- `"Others"`: For any other URL.

### `extract_show_title(tvg_name)`
This function extracts the show title, removing season and episode identifiers (e.g., "ShowName S01E01").

### Downloading Logos:
The script also downloads logos associated with each channel and saves them in the `logos` folder. Logos are only downloaded if they don’t already exist in the target folder. Further work is to be done to resize and creaate the correct folder structure.

