---
linktitle: googlemap_2_komoot
type: book # Do not modify.
toc: true
weight : 1

title: Exporting cycle paths from Google Maps and uploading to Komoot
subtitle:
summary: ""
date: 2023-04-08
draft: false  # Is this a draft? true/false
type: book  # Do not modify.
authors:
- admin
categories: []
tags:
  - Python
  - Google Map
  - Komoot
image:
  placement: 1
  # caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  caption: 'Midjourney: GPS measurement pseudorange carrierphase position'
commentable: true
---

As a recreational cyclist, I've used Google Maps over the years. While acceptable, it does have a habit of sending me on streets only designed for vehicles. Out of desperation, I began exploring different apps and settled on [komoot](https://www.komoot.com/user/3259828117697). After a few months of using it, I've had no complaints, but I really wanted to have my historical data from Google Maps within the Komoot ecosystem. Also, this allowed me to have some fun with Python.

## Step 1. Use Google Takeout to download Location History

Google Takeout allows you to load all data from your Google Account as a file. It includes your emails, photos, videos, etc. Location history is also available for download as a ZIP file.
To get it go to https://takeout.google.com/settings/takeout , check “Location History” and click on the “Next step” button.

## Step 2. Upload location history to Komoot

The task was relatively simple as there was a friendly Python Interface for Komoot API called [komPYoot](https://pypi.org/project/komPYoot/1.0.0/). While iterating through the data, I extracted segments where the activity type matched `CYCLING` and I merged all cycling activities that occured within a day.

## Code

GIST link : [googlemap_2_komoot.py](https://gist.github.com/garrettseepersad/d8d5cfebff7371985eb4f0e32f2de35c)
{{< highlight python>}}

import json
from zipfile import ZipFile
import pandas as pd
import json
from timezonefinder import TimezoneFinder
from gpx_converter import Converter
import re
from komPYoot import API, Sport
import time
import os
import argparse
from pathlib import Path

parser  = argparse.ArgumentParser(
    prog='googlemap_2_komoot',
    formatter_class=argparse.ArgumentDefaultsHelpFormatter)

parser.add_argument('--email_id',
                    type=str,
                    help='Input komoot email address')

parser.add_argument('--password',
                    type=str,
                    help='Input komoot args.password')

parser.add_argument('--google_map_location_history',
                    type=str,
                    help='Path to location history')

args = parser.parse_args()

directory = str(Path(args.google_map_location_history).absolute())

# Initialize komoot API
a = API()
a.login(args.email_id, args.password)

# Initialize timezone
tf = TimezoneFinder()

# store all places into this array
place_visits = []

previous_day = -1
output_waypoints = pd.DataFrame([], columns=['latE7','lngE7','time'])
with ZipFile(args.google_map_location_history) as myzip:
    for file in myzip.filelist[:]:
        filename = file.filename
        name = filename.split("/")[-1].split('.')[0]

        if "Semantic Location History" in filename:
            # process all files in "Semantic Location History" directory
            history_json = json.load(myzip.open(filename))

            waypoints = []
            for ix,timeline_object in enumerate(history_json["timelineObjects"]):

                if "activitySegment" in timeline_object:
                    if timeline_object['activitySegment']['activityType'] == "CYCLING":
                        startTimestamp = pd.to_datetime(timeline_object['activitySegment']['duration']['startTimestamp'])
                        endTimestamp = pd.to_datetime(timeline_object['activitySegment']['duration']['endTimestamp'])
                        duration = endTimestamp - startTimestamp

                        if 'waypointPath' in timeline_object['activitySegment'].keys():
                            waypoints = pd.DataFrame(timeline_object['activitySegment']['waypointPath']['waypoints'])
                            waypoints["latE7"].update(waypoints.latE7/1E7)
                            waypoints["lngE7"].update(waypoints.lngE7/1E7)
                            time_zone = tf.timezone_at(lng=waypoints["lngE7"][0], lat=waypoints["latE7"][0])
                            startTimestamp = startTimestamp.tz_convert(time_zone)
                            endTimestamp = endTimestamp.tz_convert(time_zone)

                            for way_ix, waypoint in waypoints.iterrows():
                                new_time_stamp = startTimestamp + ((way_ix)*duration/len(waypoints))
                                waypoints.at[way_ix,'time'] = new_time_stamp

                        if ((previous_day != -1) and (previous_day != startTimestamp.day)):
                            filename = re.sub("\s", "_",(f"{previous_name}_{previous_day}_ix_{previous_ix}"))

                            Converter.dataframe_to_gpx(input_df=(output_waypoints),
                                                    lats_colname='latE7',
                                                    longs_colname='lngE7',
                                                    times_colname='time',
                                                    output_file=f'{filename}.gpx')

                            status = a.upload_tour_gpx(Sport.BIKE_TOURING,
                                                       directory + "/" + f'{filename}.gpx',
                                                       name = filename,
                                                       duration=None)

                            time.sleep(2)

                            try:
                                os.remove(f'{filename}.gpx')
                            except FileNotFoundError:
                                print(f"File did not exist : {f'{filename}.gpx'}")

                            output_waypoints = pd.DataFrame([], columns=['latE7','lngE7','time'])
                            output_waypoints = output_waypoints.append(waypoints)

                        elif (previous_day == startTimestamp.day):
                            output_waypoints = output_waypoints.append(waypoints).reset_index(drop=True)

                        elif (previous_day == -1):
                            output_waypoints = waypoints

                        previous_day = startTimestamp.day
                        previous_name = name
                        previous_ix = ix
                        previous_waypoint = waypoints

{{< / highlight >}}
