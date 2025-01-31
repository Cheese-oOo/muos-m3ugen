# muos-m3ugen
IPTV M3U Splitter for MuOS
This Python script processes an M3U playlist file, organizes the channels into categories, downloads logos for each channel, and creates new M3U files for each channel with their respective details.

Here’s a detailed breakdown of the code:

### Libraries and Dependencies:
- **os**: Used for file and directory manipulation (e.g., creating folders, checking if files exist).
- **re**: Provides support for regular expressions to extract specific data from the M3U file.
- **requests**: Handles the downloading of logos from URLs.
- **unicodedata**: Normalizes unicode characters in filenames to remove special or non-ASCII characters.
- **urllib3**: Suppresses SSL warnings when working with insecure HTTP requests.

### Configuration:
- **m3u_file**: Specifies the input M3U playlist file (`playlist.m3u`).
- **output_folder**: The folder where channels will be categorized and saved (`categories`).
- **logo_folder**: The folder to store downloaded logos (`logos/`).
- **skip_domain**: A domain URL (`https://temp-me.xyz`) that, if present in a logo URL, will be skipped.

### Setup:
- **Ensure necessary folders exist**: Creates the output and logo folders if they don't already exist using `os.makedirs()`.

### Regular Expression Pattern:
- **pattern**: A regex pattern to extract details from the M3U file:
  - `tvg-name`: The name of the TV channel.
  - `tvg-logo`: URL of the TV channel’s logo.
  - `group-title`: The category or group the channel belongs to (e.g., "Movies", "TV Shows").
  - `url`: The actual URL of the M3U stream for the channel.

### Main Processing Logic:

1. **Read M3U File**: The script reads the content of the M3U playlist file and extracts relevant information using the regex pattern.
  
2. **Clean Filenames and Group Titles**:
   - **clean_filename**: Normalizes and cleans the `tvg-name` by removing special characters, spaces, and ensuring compatibility with file systems.
   - **clean_group_title**: Specifically cleans the `group-title`, handling cases like "UK" which is replaced with "_UK".
  
3. **Categorize Channels**: The `categorize_channel` function determines if the channel is a TV show, movie, or TV channel based on its URL. If it doesn't match any of the predefined categories, it is categorized as "Others".

4. **Create Directories and M3U Files**:
   - For each channel, a corresponding folder is created based on the category (e.g., Movies, TV Shows, etc.), and a new M3U file is created in that folder with the channel's details. If the M3U file for the channel already exists, it is skipped.

5. **Logo Download**:
   - The script attempts to download the logo for each channel (if it’s not already downloaded), saving it to the logo folder with a filename based on the cleaned `tvg-name`. The script handles potential errors with downloading the logo, including handling timeouts and failed requests.

### Key Functions:
- **clean_filename**: Cleans and normalizes the channel name to be used as a filename.
- **clean_group_title**: Custom handling for group titles, specifically for "UK".
- **categorize_channel**: Categorizes the channels into TV Shows, Movies, or TV Channels based on URL patterns.
- **extract_show_title**: For TV Shows, extracts the show title (e.g., removes episode numbers).

### Output:
1. **M3U Files**: The script creates categorized M3U files for each channel.
2. **Logos**: Downloads and saves channel logos (if not already downloaded).
3. **Folder Structure**: The folders are organized by category (TV Shows, Movies, etc.) and group-title, with subdirectories if necessary.

### Example Folder Structure:
```
categories/
    TV Shows/
        Comedy/
            Friends/
                Friends.m3u
    Movies/
        Action/
            DieHard.m3u
    TV Channels/
        News/
            BBC.m3u
logos/
    Friends.png
    DieHard.png
    BBC.png
```

### Notes:
- Ensure the **playlist.m3u** file is in the same directory or update the `m3u_file` variable with the correct path.
- The script uses SSL verification (`verify=False`) for insecure requests, so warnings about SSL certificates are suppressed. This could be unsafe in production environments, so it's something to be aware of.
