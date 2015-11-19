### Kat-api

KAT (Kickass Torrent) api wrapper with mirror fallback and advanced search parameters.


#### Simple search

```
var kat = require('kat-api');

kat.search('test').then(function (data) {
        console.log(data);
    }).catch(function (error) {
        console.log(error);
    });
```

#### Advanced search

```
kat.search({
        query: 'Anger Management',
        category: 'tv',
        min_seeds: '10',
        uploader: 'ettv',
        sort_by: 'seeders',
        order: 'desc',
        verified: 1,
        language: 'en'
    }).then(function (data) {
        console.log(data);
    }).catch(function (error) {
        console.log(error);
    });
```

*Output example:*

##### response:
```
page: 2
response_time: 743
results: Array[25]
total_pages: 824
total_results: 20578
```


##### results:
```
{ 0
    {
        category: "TV"
        comments: 16
        files: 3
        guid: "http://kat.cr/anger-management-s02e34-hdtv-xvid-afg-t7861470.html"
        hash: "8427C27F3081F7B05E1F036B851A6847124D73F5"
        leechs: 2
        link: "http://kat.cr/anger-management-s02e34-hdtv-xvid-afg-t7861470.html"
        magnet: "magnet:?xt=urn:btih:8427C27F3081F7B05E1F036B851A6847124D73F5&dn=anger+management+s02e34+hdtv+xvid+afg&tr=udp%3A%2F%2Ftracker.publicbt.com%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337"
        peers: 18
        pubDate: "Friday 13 Sep 2013 02:32:42 +0000"
        seeds: 16
        size: 178012276
        title: "Anger Management S02E34 HDTV XviD-AFG"
        torrentLink: "http://torcache.net/torrent/8427C27F3081F7B05E1F036B851A6847124D73F5.torrent?title=[kat.cr]anger.management.s02e34.hdtv.xvid.afg"
        verified: 1
        votes: 81
    }
}
```

*Use example:*

We'll display the magnet link for the most seeded 'Anger Management' episode.

```
var kat = require('kat-api');
kat.search({
    query: 'Anger Management',
    sort_by: 'seeders'
}).then(function (response) {
    console.log(response.results[0].magnet);
}).catch(function (error) {
    console.log('an error occured');
});
```

#### Parameters

KAT search requires *at least 1* of the following queries: `query, category, min_seeds, uploader, safety_filter, verified, language, age`

Every other parameter is optionnal.

--

- `query`: search terms.
 * as shown on the above examples, it can be passed simply as a string: `search('string')` if you don't use any other parameter.

- `category`: possible values: tv|movies|anime|applications|books|games|music|xxx|other   * Subcategories can also be used in the `category` parameter, but I won't list them.

- `min_seeds`: a number, integer. Can be contained in a string.
 * The torrent must be seeded by at least `min_seeds` people.

- `uploader`: a KickassTorrent's username.
 * The torrent was uploaded by `uploader`.


- `imdb`: IMDB id - *movies only*
 * You can pass the entire id or just the figures: 'tt2561572' or '2561572'.

- `tvrage`: TVRage id - *tv only*

- `sort_by`: possible values: seeders|leechers|time_add|files_count

- `order`: possible values: asc|desc
 
- `safety_filter`: 0 or 1. (1=true, 0=false)
 * This option turns off all the pornographic content.

- `verified`: 0 or 1 (1=true, 0=false)
 * The torrent has been identified as 'not fake' by the community

- `language`: langcode or language ID (see below) - *only for Movie or TV*
 * The spoken language. Example: `en` or `2`.

- `age`: possible values: hour|24h|week|month|year
 * The torrent has been uploaded in the last hour,day,week,month,year.

- `page`: integer.
 * KAT uses 25 results max per page, navigate between pages (1 to X)


#### Errors
- Regular HTTP request errors.

- Regular Javascript errors.

- `Missing a mandatory parameter`: you made an error somewhere.

- `No data`: this happens when the server sent back a statusCode > 400 or if the something went wrong with the request, but the server answered.

- `No results`: 0 results found. Catch this error to warn your user there were no results for its request, or to know when you reached last page.


#### Languages

About the language parameter:

- English: 'en' (id:2)
- Albanian: 'sq' (id:42)
- Arabic: 'ar' (id:7)
- Basque: 'eu' (id:44)
- Bengali: 'bn' (id:46)
- Brazilian: 'pt-br' (id:39)
- Bulgarian: 'bg' (id:37)
- Cantonese: 'yue' (id:45)
- Catalan: 'ca' (id:47)
- Chinese: 'zh' (id:10)
- Croatian: 'hr' (id:34)
- Czech: 'cs' (id:32)
- Danish: 'da' (id:26)
- Dutch: 'nl' (id:8)
- Filipino: 'tl' (id:11)
- Finnish: 'fi' (id:31)
- French: 'fr' (id:5)
- German: 'de' (id:4)
- Greek: 'el' (id:30)
- Hebrew: 'he' (id:25)
- Hindi: 'hi' (id:6)
- Hungarian: 'hu' (id:27)
- Italian: 'it' (id:3)
- Japanese: 'ja' (id:15)
- Kannada: 'kn' (id:49)
- Korean: 'ko' (id:16)
- Lithuanian: 'lt' (id:43)
- Malayalam: 'ml' (id:21)
- Mandarin: 'cmn' (id:23)
- Nepali: 'ne' (id:48)
- Norwegian: 'no' (id:19)
- Persian: 'fa' (id:33)
- Polish: 'pl' (id:9)
- Portuguese: 'pt' (id:17)
- Punjabi: 'pa' (id:35)
- Romanian: 'ro' (id:18)
- Russian: 'ru' (id:12)
- Serbian: 'sr' (id:28)
- Slovenian: 'sl' (id:36)
- Spanish (latin america): '' (id:41) //no langcode, use ID.
- Spanish: 'es' (id:14)
- Swedish: 'sv' (id:20)
- Tamil: 'ta' (id:13)
- Telugu: 'te' (id:22)
- Thai: 'th' (id:24)
- Turkish: 'tr' (id:29)
- Ukrainian: 'uk' (id:40)
- Vietnamese: 'vi' (id:38)