

# scrape-yt
[![NPM](https://nodei.co/npm/scrape-yt.png?downloads=true)](https://nodei.co/npm/scrape-yt)

`scarpe-yt` is a simple package to scrape information from Youtube such as search results, video information, related videos, playlist information, and up next video

#### APIs:
- [search](#searchquery-options)
- [getPlaylist](#getplaylistplaylistid-options)
- [getVideo](#getvideovideoid-options)
- [getRelated](#getrelatedvideoid-options)
- [getUpNext](#getupnextvideoid-options)

## Installation
```bash
npm i scrape-yt
```

## Usage Example
Basic usage:
```js
// ES5 
const scrapeYt = require("scrape-yt");

// ES6 / TypeScript
import scrapeYt from "scrape-yt";
// or import only necessary methods
import { search, getVideo } from "scrape-yt"

// Searches for video, playlist, or channel with keyword "Never gonna give you up" and limited to 10 search result
(async() => {
    let videos = await scrapeYt.search("Never gonna give you up");
    console.log(videos[0]);
})();

// Or use promises
scrapeYt.search("Never gonna give you up", {
    type: "video"
}).then(videos => {
    console.log(videos);
});

```

Type check (see available [types](https://github.com/VincentJonathan/scrape-yt/blob/master/src/common/types.ts)):
```js

import scrapeYt, { SearchOptions } from "scrape-yt";

let options: SearchOptions = {
    type: "channel",
    limit: 5
}
await scrapeYt.search("Rick astley", options); 

```

## API
### search(query, [options])
Searches for result with given `query`.  `options` is optional and can have the following keys
- `type` - Search type, can be `all`, `video`, `playlist` or `channel` (Default = `all`)
- `limit` - The max count of the search result (Default = `10`)
- `page` - Show result on specified page (Default = `1`)
- `useWorkerThread` - Whether to use worker thread for scraping or not (Default = `false`)

Result example (video):
```json
[
    {
        "id": "dQw4w9WgXcQ",
        "title": "Rick Astley - Never Gonna Give You Up (Video)",
        "duration": 213,
        "thumbnail": "https://i.ytimg.com/vi/dQw4w9WgXcQ/hqdefault.jpg?sqp=-oaymwEjCPYBEIoBSFryq4qpAxUIARUAAAAAGAElAADIQj0AgKJDeAE=&rs=AOn4CLBW5JbJn5nTCNKe8PvMuOqEiuttiQ",
        "channel": {
            "id": "UCuAXFkgsw1L7xaCfnd5JJOw",
            "name": "Official Rick Astley",
            "url": "https://www.youtube.com/channel/UCuAXFkgsw1L7xaCfnd5JJOw"
        },
        "uploadDate": "10 tahun yang lalu",
        "viewCount": 680314160,
        "type": "video"
    },
    ...
]
```
Result example (playlist):
```json
[
    {
        "id": "PLx65qkgCWNJIgVrndMrhsedBz1VDp0kfm",
        "title": "Very Important Videos",
        "thumbnail": "https://i.ytimg.com/vi/0woboOZ9dmY/hqdefault.jpg?sqp=-oaymwEXCPYBEIoBSFryq4qpAwkIARUAAIhCGAE=&rs=AOn4CLDcy1wd9LA-toJs7Gq-_I5-00n0Mw",
        "channel": {
            "id": "electrickeye91",
            "name": "Thomas Frank",
            "url": "https://www.youtube.com/user/electrickeye91"
        },
        "videoCount": 37,
        "type": "playlist"
    },
    ...
]
```
Result example (channel):
```json
[
    {
        "id": "beegeestv",
        "name": "beegees",
        "thumbnail": "https://lh3.googleusercontent.com/a-/AOh14Gh8qGEFRMCi1sYKrapMiXS3pcOsejBEGK9WAGQsgA=s176-c-k-c0x00ffffff-no-rj-mo",
        "videoCount": 55,
        "url": "https://www.youtube.com/user/beegeestv",
        "type": "channel"
    },
    ...
]
```


---
### getPlaylist(playlistId, [options])
Get playlist information and videos from the given `playlistId`. `options` is optional and can have the following keys
- `useWorkerThread` - Whether to use worker thread for scraping or not (Default = `false`)

Result example:
```json
{
    "id": "PLx65qkgCWNJIgVrndMrhsedBz1VDp0kfm",
    "title": "Very Important Videos",
    "videoCount": 37,
    "viewCount": 159705,
    "lastUpdatedAt": "Terakhir diperbarui pada 13 Okt 2019",
    "channel": {
        "id": "electrickeye91",
        "name": "Thomas Frank",
        "thumbnail": "https://yt3.ggpht.com/a/AATXAJyAUDbyAyY3JkoMO-7Pvvf4QtksJF9Y6C6fTg=s100-c-k-c0xffffffff-no-rj-mo",
        "url": "https://www.youtube.com/user/electrickeye91"
    },
    "videos": [
        {
            "id": "0woboOZ9dmY",
            "title": "Poopy-di Scoop",
            "duration": 17,
            "thumbnail": "https://i.ytimg.com/vi/0woboOZ9dmY/hqdefault.jpg?sqp=-oaymwEiCKgBEF5IWvKriqkDFQgBFQAAAAAYASUAAMhCPQCAokN4AQ==&rs=AOn4CLD-Z5XXZlfjshvyd3K-oYIkGo-0Rw",
            "channel": {
                "id": "UCwPDMtvphYeGbzDKa8obOnQ",
                "name": "Poorly Made",
                "url": "https://www.youtube.com/channel/UCwPDMtvphYeGbzDKa8obOnQ"
            }
        },
        ...
     ]
}
```
**Note**: `videos` only limited to 100

---
### getVideo(videoId, [options])
Get video information of given `videoId`. `options` is optional and can have the following keys
- `useWorkerThread` - Whether to use worker thread for scraping or not (Default = `false`)

Result example:
```json
{
    "id": "dQw4w9WgXcQ",
    "title": "Rick Astley - Never Gonna Give You Up (Video)",
    "duration": 212,
    "thumbnail": "https://i.ytimg.com/vi/dQw4w9WgXcQ/maxresdefault.jpg",
    "description": "Rick Astley's official music video for “Never Gonna Give You Up”...",
    "channel": {
        "id": "UCuAXFkgsw1L7xaCfnd5JJOw",
        "name": "Official Rick Astley",
        "thumbnail": "https://yt3.ggpht.com/a-/AOh14GirqtIADQGwQOF14rTXYeSzIkuySwxwlqAZyzo0mQ=s176-c-k-c0xffffffff-no-nd-rj",
        "url": "https://www.youtube.com/channel/UCuAXFkgsw1L7xaCfnd5JJOw"
    },
    "uploadDate": "Dipublikasikan tanggal 24 Okt 2009",
    "viewCount": 680735840,
    "likeCount": 5257356,
    "dislikeCount": 193590,
    "isLiveContent": false,
    "tags": [
        "#RickAstley",
        "#NeverGonnaGiveYouUp",
        "#DancePop"
    ]
}
```

---
### getRelated(videoId, [options])
Get videos related to given `videoId`. `options` is optional and can have the following keys
- `limit` - The max count of the search result (Default = `10`)
- `useWorkerThread` - Whether to use worker thread for scraping or not (Default = `false`)

Result example:
```json
[
    {
        "id": "I_izvAbhExY",
        "title": "Bee Gees - Stayin' Alive (Official Video)",
        "duration": 250,
        "thumbnail": "https://i.ytimg.com/vi/I_izvAbhExY/hqdefault.jpg?sqp=-oaymwEjCNACELwBSFryq4qpAxUIARUAAAAAGAElAADIQj0AgKJDeAE=&rs=AOn4CLCEl-X_ZnGdzoLqS-wfFcs1rED1LQ",
        "channel": {
            "id": "UCD9sCcKXnFxMeuFoNayVxeQ",
            "name": "beegees",
            "url": "https://www.youtube.com/channel/UCD9sCcKXnFxMeuFoNayVxeQ"
        },
        "uploadDate": "10 tahun lalu",
        "viewCount": 530482048
    },
    ...
]
```

---
### getUpNext(videoId, [options])
Get up next video of given `videoId`. `options` is optional and can have the following keys
- `useWorkerThread` - Whether to use worker thread for scraping or not (Default = `false`)

Result example:
```json
{
    "id": "yPYZpwSpKmA",
    "channel": {
        "id": "UCuAXFkgsw1L7xaCfnd5JJOw",
        "name": "Official Rick Astley",
        "url": "https://www.youtube.com/channel/UCuAXFkgsw1L7xaCfnd5JJOw"
    },
    "title": "Rick Astley - Together Forever (Official Music Video)",
    "duration": 205,
    "thumbnail": "https://i.ytimg.com/vi/yPYZpwSpKmA/hqdefault.jpg?sqp=-oaymwEjCNACELwBSFryq4qpAxUIARUAAAAAGAElAADIQj0AgKJDeAE=&rs=AOn4CLAIYA0llYwvjijecWwCYJgXMx6YWA",
    "uploadDate": "10 tahun lalu",
    "viewCount": 98107272
}

```
---
**Note**:
* `duration` is in second
* `uploadDate` and `lastUpdatedAt` language is based on  the default language the youtube set for you 


## License
[MIT](https://github.com/VincentJonathan/scrape-yt/blob/master/LICENSE)
