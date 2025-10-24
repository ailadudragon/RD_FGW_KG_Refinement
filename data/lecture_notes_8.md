# Data Science Programming:markdown
:markdown
## Week 9: Lecture 1: Time Series Data Analysis:markdown
:markdown
---:markdown
:markdown
**Agenda:**:markdown
- Apply techiques to time series data:markdown

# Time Series:markdown
Time series data is an important form of structured data in many different fields, such:markdown
as finance, economics, ecology, neuroscience, and physics. Anything that is observed:markdown
or measured at many points in time forms a time series. Many time series are fixed:markdown
frequency, which is to say that data points occur at regular intervals according to some:markdown
rule, such as every 15 seconds, every 5 minutes, or once per month. Time series can:markdown
also be irregular without a fixed unit of time or offset between units. How you mark:markdown
and refer to time series data depends on the application, and you may have one of the:markdown
following::markdown
:markdown
- Timestamps, specific instants in time Fixed periods, such as the month January 2007 or the full year 2010:markdown
- Intervals of time, indicated by a start and end timestamp. Periods can be thought:markdown
of as special cases of intervals:markdown
- Experiment or elapsed time; each timestamp is a measure of time relative to a:markdown
particular start time (e.g., the diameter of a cookie baking each second since:markdown
being placed in the oven):markdown

# imports:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code

## Date and Time Data Types and Tools:markdown
The Python standard library includes data types for date and time data, as well as:markdown
calendar-related functionality. The datetime, time, and calendar modules are the:markdown
main places to start. The datetime.datetime type, or simply datetime, is widely:markdown
used.:markdown
```:code
from datetime import datetime:code
now = datetime.now():code
:code
now.year, now.month, now.day:code
```:code

We can apply arithmatic operations on datetime objects::markdown
```:code
datetime.now() - datetime(2024, 4, 20):code
```:code

The result have time related properties::markdown
```:code
delta = datetime(2021, 1, 7) - datetime(2008, 6, 24, 8, 15):code
:code
delta.days:code
delta.seconds:code
```:code

## Time Zone Handling:markdown
:markdown
Working with time zones is generally considered one of the most unpleasant parts of:markdown
time series manipulation. As a result, many time series users choose to work with:markdown
time series in coordinated universal time or UTC, which is the successor to Greenwich:markdown
Mean Time and is the current international standard. Time zones are expressed as:markdown
offsets from UTC; for example, New York is four hours behind UTC during daylight:markdown
saving time and five hours behind the rest of the year.:markdown
:markdown
In Python, time zone information comes from the third-party pytz library (installable:markdown
with pip or conda), which exposes the Olson database, a compilation of world:markdown
time zone information. This is especially important for historical data because the:markdown
daylight saving time (DST) transition dates (and even UTC offsets) have been:markdown
changed numerous times depending on the whims of local governments. In the United:markdown
States, the DST transition times have been changed many times since 1900!:markdown
:markdown
```:code
import pytz:code
local_tz = pytz.timezone('America/New_York'):code
:code
now = datetime.now():code
:code
now = now.astimezone(local_tz):code
:code
now.year:code
:code
now.hour:code
:code
now.tzinfo:code
```:code

```:code
for tz in pytz.common_timezones::code
if 'America' in tz::code
print(tz):code
```:code


### Converting Between String and Datetime:markdown
Format datetime objects and pandas Timestamp objects as strings using str or the strftime method, passing a format specification.:markdown
```:code
stamp = datetime(2021, 1, 3):code
str(stamp):code
stamp.strftime('%Y-%m-%d'):code
:code
s = stamp.strftime('%m/%d/%Y'):code
```:code

Convert from string to datetime::markdown
```:code
d = datetime.strptime(s, '%m/%d/%Y'):code
```:code

## Exercise::markdown
```:code
value = '2021-01-03':code
datetime.strptime(value, '%Y-%m-%d'):code
datestrs = ['7/6/2021', '8/6/2021']:code
[datetime.strptime(x, '%m/%d/%Y') for x in datestrs]:code
```:code

pandas is generally oriented toward working with arrays of dates, whether used as an:markdown
axis index or a column in a DataFrame. The to_datetime method parses many different:markdown
kinds of date representations. Standard date formats like ISO 8601 can be:markdown
parsed very quickly::markdown
```:code
datestrs = ['2021-07-06 12:00:00', '2021-08-06 00:00:00']:code
pd.to_datetime(datestrs):code
```:code

## Time Series Basics:markdown
A basic kind of time series object in pandas is a Series indexed by timestamps, which:markdown
is often represented external to pandas as Python strings or datetime objects.:markdown

```:code
from datetime import datetime:code
dates = [datetime(2021, 1, 2), datetime(2021, 1, 5),:code
datetime(2021, 1, 7), datetime(2021, 1, 8),:code
datetime(2021, 1, 10), datetime(2021, 1, 12)]:code
ts = pd.Series(np.random.randn(6), index=dates):code
ts:code
```:code

```:code
ts.index:code
```:code

Like other Series, arithmetic operations between differently indexed time series automatically:markdown
align on the dates::markdown
```:code
ts + ts[::2]:code
```:code

### Indexing, Selection, Subsetting:markdown
Time series behaves like any other pandas.Series when you are indexing and selecting:markdown
data based on label::markdown
```:code
stamp = ts.index[2]:code
ts[stamp]:code
```:code

For longer time series, a year or only a year and month can be passed to easily select:markdown
slices of data::markdown
```:code
longer_ts = pd.Series(np.random.randn(1000),:code
index=pd.date_range('1/1/2023', periods=1000)):code
longer_ts:code
```:code

```:code
longer_ts['2023-5']:code
```:code

### Time Series with Duplicate Indices:markdown
In some applications, there may be multiple data observations falling on a particular:markdown
timestamp. Here is an example::markdown
```:code
dates = pd.DatetimeIndex(['1/1/2000', '1/2/2000', '1/2/2000',:code
'1/2/2000', '1/3/2000']):code
dup_ts = pd.Series(np.arange(5), index=dates):code
dup_ts:code
```:code

Suppose you wanted to aggregate the data having non-unique timestamps. One way:markdown
to do this is to use groupby and pass level=0::markdown
```:code
grouped = dup_ts.groupby(level=0):code
grouped.mean():code
grouped.count():code
```:code

## Date Ranges, Frequencies, and Shifting:markdown
Generic time series in pandas are assumed to be irregular; that is, they have no fixed:markdown
frequency. For many applications this is sufficient. However, it’s often desirable to:markdown
work relative to a fixed frequency, such as daily, monthly, or every 15 minutes, even if:markdown
that means introducing missing values into a time series. Fortunately pandas has a:markdown
full suite of standard time series frequencies and tools for resampling, inferring frequencies,:markdown
and generating fixed-frequency date ranges. For example, you can convert:markdown
the sample time series to be fixed daily frequency by calling resample::markdown

```:code
from datetime import datetime:code
dates = [datetime(2021, 1, 2), datetime(2021, 1, 5),:code
datetime(2021, 1, 7), datetime(2021, 1, 8),:code
datetime(2021, 1, 10), datetime(2021, 1, 12)]:code
ts = pd.Series(np.random.randn(6), index=dates):code
ts:code
```:code

```:code
ts.resample('D'):code
```:code

```:code
resampler = ts.resample('D'):code
resampler:code
```:code

### Generating Date Ranges:markdown
pandas.date_range is responsible for:markdown
generating a DatetimeIndex with an indicated length according to a particular:markdown
frequency::markdown
```:code
index = pd.date_range('2012-04-01', '2012-06-01'):code
index:code
```:code

```:code
pd.date_range(start='2012-04-01', periods=20):code
pd.date_range(end='2012-06-01', periods=20):code
```:code

```:code
pd.date_range('2000-01-01', '2000-12-01', freq='BM'):code
```:code

```:code
pd.date_range('2012-05-02 12:56:31', periods=5):code
```:code

```:code
pd.date_range('2012-05-02 12:56:31', periods=5, normalize=True):code
```:code

### Frequencies and Date Offsets:markdown

```:code
from pandas.tseries.offsets import Hour, Minute:code
hour = Hour():code
hour:code
```:code

```:code
four_hours = Hour(4):code
four_hours:code
```:code

#### Week of month dates:markdown
One useful frequency class is “week of month,” starting with WOM. This enables you to:markdown
get dates like the third Friday of each month::markdown
```:code
rng = pd.date_range('2012-01-01', '2012-09-01', freq='WOM-3FRI'):code
list(rng):code
```:code

### Shifting (Leading and Lagging) Data:markdown
“Shifting” refers to moving data backward and forward through time. Both Series and:markdown
DataFrame have a shift method for doing naive shifts forward or backward, leaving:markdown
the index unmodified::markdown

```:code
ts = pd.Series(np.random.randn(4),:code
index=pd.date_range('1/1/2000', periods=4, freq='M')):code
ts:code
ts.shift(2):code
ts.shift(-2):code
```:code

## Time Zone Handling in Pandas:markdown

### Time Zone Localization and Conversion:markdown
By default, time series in pandas are time zone naive. For example, consider the following:markdown
time series::markdown
```:code
rng = pd.date_range('3/9/2012 9:30', periods=6, freq='D'):code
ts = pd.Series(np.random.randn(len(rng)), index=rng):code
ts:code
```:code

### Operations with Time Zone−Aware Timestamp Objects:markdown
Similar to time series and date ranges, individual Timestamp objects similarly can be:markdown
localized from naive to time zone–aware and converted from one time zone to:markdown
another::markdown
```:code
stamp = pd.Timestamp('2011-03-12 04:00'):code
stamp_utc = stamp.tz_localize('utc'):code
stamp_utc.tz_convert('America/New_York'):code
```:code

### Operations Between Different Time Zones:markdown
If two time series with different time zones are combined, the result will be UTC.:markdown
Since the timestamps are stored under the hood in UTC, this is a straightforward:markdown
operation and requires no conversion to happen::markdown
```:code
rng = pd.date_range('3/7/2012 9:30', periods=10, freq='B'):code
ts = pd.Series(np.random.randn(len(rng)), index=rng):code
ts:code
ts1 = ts[:7].tz_localize('Europe/London'):code
ts2 = ts1[2:].tz_convert('Europe/Moscow'):code
result = ts1 + ts2:code
result.index:code
```:code

## Periods and Period Arithmetic:markdown
Periods represent timespans, like days, months, quarters, or years. The Period class:markdown
represents this data type, requiring a string or integer and a frequency.:markdown
```:code
p = pd.Period(2007, freq='A-DEC'):code
p:code
```:code

### Period Frequency Conversion:markdown
```:code
p = pd.Period('2007', freq='A-DEC'):code
p:code
p.asfreq('M', how='start'):code
p.asfreq('M', how='end'):code
```:code

### Quarterly Period Frequencies:markdown
```:code
p = pd.Period('2012Q4', freq='Q-JAN'):code
p:code
```:code

### Converting Timestamps to Periods (and Back):markdown
```:code
rng = pd.date_range('2000-01-01', periods=3, freq='M'):code
ts = pd.Series(np.random.randn(3), index=rng):code
ts:code
pts = ts.to_period():code
pts:code
```:code

## Resampling and Frequency Conversion:markdown
```:code
rng = pd.date_range('2000-01-01', periods=100, freq='D'):code
ts = pd.Series(np.random.randn(len(rng)), index=rng):code
ts:code
ts.resample('M').mean():code
ts.resample('M', kind='period').mean():code
```:code

### Downsampling:markdown
```:code
rng = pd.date_range('2000-01-01', periods=12, freq='T'):code
ts = pd.Series(np.arange(12), index=rng):code
ts:code
```:code

```:code
ts.resample('5min', closed='right').sum():code
```:code

```:code
ts.resample('5min', closed='right',:code
label='right', loffset='-1s').sum():code
```:code

#### Open-High-Low-Close (OHLC) resampling:markdown
```:code
ts.resample('5min').ohlc():code
```:code

### Upsampling and Interpolation:markdown
```:code
frame = pd.DataFrame(np.random.randn(2, 4),:code
index=pd.date_range('1/1/2000', periods=2,:code
freq='W-WED'),:code
columns=['Colorado', 'Texas', 'New York', 'Ohio']):code
frame:code
```:code

```:code
frame.resample('W-THU').ffill():code
```:code

### Resampling with Periods:markdown
```:code
frame = pd.DataFrame(np.random.randn(24, 4),:code
index=pd.period_range('1-2000', '12-2001',:code
freq='M'),:code
columns=['Colorado', 'Texas', 'New York', 'Ohio']):code
frame[:5]:code
annual_frame = frame.resample('A-DEC').mean():code
annual_frame:code
```:code

```:code
# Q-DEC: Quarterly, year ending in December:code
annual_frame.resample('Q-DEC').ffill():code
annual_frame.resample('Q-DEC', convention='end').ffill():code
```:code

```:code
annual_frame.resample('Q-MAR').ffill():code
```:code

