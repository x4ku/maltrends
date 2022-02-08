# maltrends

[MyAnimeList.net] manga and anime trend data.

[![pipeline status](https://gitlab.com/x4ku/maltrends/badges/main/pipeline.svg)](https://gitlab.com/x4ku/maltrends/-/commits/main)

## Overview

This project collects manga and anime data from [MyAnimeList.net] each week.
This data is used to track trends in popularity.

You can download the latest files here:
- [`manga.jsonl`]
- [`manga-ranking.jsonl`]
- [`anime.jsonl`]
- [`anime-ranking.jsonl`]

The schema of these records is expressed in the following schema files:
- [`manga.json`]
- [`anime.json`]
- [`ranking.json`]

## Usage

You can read and work with the data files from this project as-is, line-by-line.
Each line is a JSON array containing an entry's *values*. There is no need to
even load the entire file into memory if your use-case does not require it -
simply read and work with one line at a time.

By storing only values instead of highly redundant dictionaries (i.e. *keys* and
*values*), we are able to achieve a massive reduction in file size while still
retaining the ability to express the complex structure of these records in JSON.
If you would prefer to convert these records into dictionaries/objects, a
reference implementation is provided below.

### Converting Entries to Dictionaries

The reference implementation below uses the [`manga.json`] schema file to
convert each record from the [`manga.jsonl`] file into a dictionary/object.

```py
import json
from pathlib import Path

def map_schema(value, schema):
    if type(schema) is list:
        return [map_schema(v, schema[0]) for v in value]
    if type(schema) is dict:
        kvs = zip(schema.keys(), value, schema.values())
        return {k: map_schema(v, s) for k, v, s in kvs}
    return value

with open(Path('schema') / 'manga.json') as file:
    schema = json.load(file)

with open(Path('data') / 'manga.jsonl') as file:
    entries = [map_schema(json.loads(line), schema) for line in file]
```

The `entries` variable should now contain the records as dictionaries.

## License

See the [`LICENSE`] file before using any of the files provided by this project
in your own work.


<!-- links -->

[`LICENSE`]: LICENSE
[`anime.json`]: schema/anime.json
[`anime.jsonl`]: data/anime.jsonl
[`anime-ranking.jsonl`]: data/anime-ranking.jsonl
[`manga.json`]: schema/manga.json
[`manga.jsonl`]: data/manga.jsonl
[`manga-ranking.jsonl`]: data/anime-ranking.jsonl
[`ranking.json`]: schema/ranking.json

[MyAnimeList.net]: https://myanimelist.net
