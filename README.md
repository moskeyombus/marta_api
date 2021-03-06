# marta_api ![build status](https://codeship.com/projects/a6c28c70-b064-0133-543b-565ee1f98c10/status?branch=master)


Set the MARTA_TRAIN_API_KEY environment variable and then run this app to give yourself a
proxy between you and MARTA that:
- removes all capitalized letters from the MARTA API response (got to be too much, sorry)
- caches API responses for 10s (all users see the same API response within a 10s window)
- puts `scheduled` entries into the API response when a station has one direction, but not the other

### basic instructions:
1. clone the repo, cd to it
2. run `npm install` (install node if you haven't, at this step)
3. set the `MARTA_TRAIN_API_KEY` environment variable, and then run it:

```
MARTA_TRAIN_API_KEY=<api_key> node server.js
```


# updates

This grew a little since it was first written. In addition to serving as a proxy between MARTA's API and the browser
(because you shouldn't put your marta API key into the browser), there now exists a hook that fills in data gaps with schedule data.

## schedule data

Marta publishes its schedule data in [a zip file](http://www.itsmarta.com/developers/data-sources/general-transit-feed-specification-gtfs.aspx).
The files within conform to Google's [GTFS spec](https://developers.google.com/transit/gtfs/reference).
This git repo contains a `parse_gtfs.js` task within `/lib/tasks` that parses those files into some JSON files.
Those JSON files are used to fill gaps in the realtime data.

## parsing GTFS

I've committed the currently used JSON schedule files in `/lib/schedule`, but over time they will become outdated.
The process for updating them is:

1. extract the [zip file](http://www.itsmarta.com/developers/data-sources/general-transit-feed-specification-gtfs.aspx) to
a `/google_transit` directory (within the project root).
2. run `node lib/tasks/parse_gtfs.js` to run the task that generates the schedule files
