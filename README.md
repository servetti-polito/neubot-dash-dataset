Neubot DASH dataset for MMsys 2014
==================================

This DASH streaming dataset is part of the data collected by
[Neubot](http://www.neubot.org) during the period from November 1st, 2013 to
January 31, 2014. Neubot _dashtest_ periodically tests and records DASH
streaming sessions from each Neubot client to a server in the Internet.

These measurements provide insights into the performance of DASH streaming on a
large scale and they may provide cues to improve the design of new rate
adaptation logic algorithms in terms of quality of experience.

The Neubot DASH Module
----------------------

The dashtest is implemented by _mod_dash_, a Neubot module that emulates the
DASH streaming of a video resource composed of fifteen two-second segments.
Each segment is available in one of the bitrate representations used by the
[DASH dataset](http://www-itec.uni-klu.ac.at/dash/?page_id=207) (100, 150, 200,
250, 300, 400, 500, 700, 900, 1200, 1500, 2000, 2500, 3000, 4000, 5000, 6000,
7000, 10000, 20000 kb/s).

The client connects to the test server and requests the fifteen emulated-video
segments using the adaptation logic defined as follows.

The client requests the first segment using the most-conservative bitrate
representation, i.e., 100 kb/s. Then, by using the same persistent HTTP
connection, the client requests the subsequent segments, selecting the highest
available bitrate representation that is lower than the estimated available
bandwidth (EAB). For further details please read [the
paper](http://media.polito.it/mmsys14).


Dataset structure
-----------------

The Neubot DASH module performs about 10,000 tests each day involving more
1,000 IP addresses from about 100 countries and 1,000 autonomous systems.  A
single dashtest lasts about 30 seconds because it performs the download of 15
two-second long segments at the available download bitrate.

Each test result is described with a number of properties that characterize the
Neubot client, the server, the connection and the details of each segment
download (size, duration, etc.). 
The properties recorded for each test are stored in a JSON file with the
following format 

	{
	  "srvr_schema_version": 3,
	  "srvr_timestamp": 1383602906,
	  "client": [
	    {
	      "real_address": "71.231.205.19",
	      "delta_user_time": 0,
	      "uuid": "33c1f516-0021-45bc-9b30-ac33081e1f98",
	      "elapsed_target": 2,
	      "received": 25129,
	      "timestamp": 1383602884,
	      "connect_time": 0.00015148508828133345,
	      "iteration": 0,
	      "remote_address": "213.208.152.45",
	      "elapsed": 0.9627354005060624,
	      "platform": "win32",
	      "rate": 100,
	      "version": "0.004016009",
	      "delta_sys_time": 0,
	      "internal_address": "127.0.0.1",
	      "request_ticks": 173191.53351338883
	    },
	    ...
	  ],
	
	}


The following Table describes the meaning of each recorded property.

|  NAME                 |  EXAMPLE                              |  DESCRIPTION                                         | 
|:----------------------|:--------------------------------------|:-----------------------------------------------------|
|  uuid                 |  7528d674-25f0-4ac4-aff6-46f446034d81 |  Random unique identifier of the Neubot instance.    |
|  platform             |  linux2                               |  The operating system platform, e.g. linux2, win32.  |
|  version              |  0.004016008                          |  Neubot version number.                              |
|  real_address         |  130.192.225.141                      |  Neubot's IP address, as seen by the server.         |
|  internal_address     |  130.192.225.141                      |  Neubot's IP address, as seen by Neubot.             |
|  remote_address       |  80.239.142.212                       |  The server's IP address.                            |
|  whole_test_timestamp |  1382434858                           |  Time when the test was performed (Unix epoch time). |
|  srvr_data.timestamp  |  1382434858                           |  Time when the test was performed on the server (Unix epoch time).         |
|  timestamp            |  1382434858                           |  Time when the test was started (Unix epoch time).   |
|  clnt_schema_version  |  3                                    |  Version of the client schema.                       |
|  connect_time         |  0.02469491958618164                  |  RTT estimated by measuring the time that connect() takes to complete, measured in seconds. |
|  iteration            |  14                                   |  Sequence number of the current donwload request.    |
|  request_ticks        |  1382434858.103292                    |  Time when the request was performed on the server (Unix epoch time).      |
|  elapsed_target       |  2                                    |  Expected download duration, measured in seconds.    |
|  rate                 |  20000                                |  Segment representation rate for the current request, measured in kbit/s.  |
|  elapsed              |  0.5023031234741211                   |  Time elapsed from the download request to the end of the download (i.e. download duration), measured in seconds.  |
|  received             |  5000131                              |  Amount of bytes received from the server for the current request, measured in bytes.  |
|  delta_user_time      |  0.06000000000000005                  |  Accumulated user time during a request, measured in seconds.              |
|  delta_sys_time       |  0.06000000000000005                  |  Accumulated system time during a request, measured in seconds.            |


Organization of files in archives
---------------------------------

All the Neubot raw data are organized into tarballs, which are grouped by the
tool that generated the data, the date when the data was collected, and the
server that collected the data. This means that each tarball contains all the
data collected during a single day, by a single tool running on a single M-Lab
server. For example, the tarball 20131008T000000Z-mlab1-lga01-neubot-0000.tgz
contains the first 1GB of data collected by all the Neubot tests that were
served by the M-Lab server mlab1-lga01 on October 8, 2013. 

For this dataset all _dashtest_ JSON files in the tarballs for a given month
(Nov, Dec, Jan) are collected in a single archive named with the year and month
of the original tarballs in the format (YYYYMM):

-  neubot-dash-streaming-dataset-mmsys14-201311.tar.gz   (199 MB - Nov 2013)
-  neubot-dash-streaming-dataset-mmsys14-201312.tar.gz   (214 MB - Dec 2013)
-  neubot-dash-streaming-dataset-mmsys14-201401.tar.gz   (197 MB - Jan 2014)


Geolocation of the tests
------------------------

The dataset also includes a companion database that reports geolocation
information for each _dashtest_ performed during the dataset collection period.
For each test the client IP has been georeferenced using the geolocation
information of the free Geolite service provided by
[http://www.maxmind.com](http://www.maxmind.com) extracting the data reported
in the following table.


|  Name            |  Example                                 |  Description                                          |
|:-----------------|:-----------------------------------------|:------------------------------------------------------|
|  test_name       |  dash                                    |  Name of the Neubot test                              |
|  mlabservername  |  mlab2-dfw01                             |  Name of the M-LAB server for the test                |
|  lon             |  -94.627500                              |  Longitude (from IP geolocation)                      |
|  lat             |  39.114200                               |  Latitude (from IP geolocation)                       |
|  city            |  Kansas City                             |  City (from IP geolocation)                           |
|  client_country  |  US                                      |  Country code (from IP geolocation)                   |
|  region          |  KS                                      |  Region code  (from IP geolocation)                   |
|  client_provider |  AS16586 CLEARWIRE - CLEAR WIRELESS LLC  |  Client provider autonomous system (code and name)    |
|  asnum           |  16586                                   |  Provider autonomous system number                    |
|  hour            |  0                                       |  Hour of the test (0-23)                              |
|  month           |  1                                       |  Month of the test (1-12)                             |
|  year            |  2014                                    |  Year of the test                                     |
|  weekday         |  6                                       |  Day of week of the test (Monday=1)                   |
|  day             |  12                                      |  Day of the month of the test                         |
|  filedate        |  20140112                                |  Date on the tarball filename (YYYYMMDD)              |
|  client_address  |  96.24.239.246                           |  $ Client IP address                                  |
|  connect_time    |  0.151514                                |  $ RTT estimated by measuring the time that connect() takes to complete, measured in seconds.   |
|  neubot_version  |  0.004016009                             |  $ Neubot version number                              |
|  platform        |  darwin                                  |  $ The operating system platform, e.g. linux2, win32. |
|  remote_address  |  38.107.216.31                           |  $ The server's IP address.                           |
|  timestamp       |  1389484875                              |  $ Time when the test was started (Unix epoch time).  |
|  uuid            |  0d929236-fc4e-4319-ab49-e08fbcc4a744    |  $ Random unique identifier of the Neubot instance.   |
|  download_speed  |  374175.181956                           |  Average download speed (amount of bytes received / effective elapsed seconds )  |
|  upload_speed    |  -1.000000                               |  (not defined)                                        |
|  latency         |  -1.000000                               |  (not defined)                                        | 

_Mlabservername_, _timestamp_ and _client_address_ fields are sufficient to match
(and check) the geolocation records to the _dashtest_ information in the JSON
files.

The database has been split in three files (as it is done for the JSON files):

- data_csv4.201311.all.csv.gz  (22 MB - Nov 2013)
- data_csv4.201312.all.csv.gz  (24 MB - Dec 2013)
- data_csv4.201401.all.csv.gz  (22 MB - Jan 2014)


Citation of the dataset
-----------------------

We kindly ask you to refer the following paper in any publication mentioning
our dataset:

Simone Basso, Antonio Servetti, Enrico Masala, Juan Carlos De Martin,
"Measuring DASH Streaming Performance from the End Users Perspective using
Neubot", In Proc. of the ACM Multimedia Systems Conference 2014, Singapore,
March 19-21, 2014.




