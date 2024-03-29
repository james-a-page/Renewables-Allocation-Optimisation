# Data Processing

## Sources

Weather - https://www.visualcrossing.com/weather-history#

Using ElexonDataPortal library https://github.com/OSUKED/ElexonDataPortal
https://data.nationalgrideso.com/generation/daily-wind-availability/r/daily_wind_availability

Names of generators https://www.netareports.com/data/elexon/bmu.jsp?id=4115

## Data Stages

Initally starting with only wind options as data is most prevalent separate extening implementation will be required in future

1. Get list of generator units & correcsponding location
2. Get data set of actual generation on different dates & corresponding generator
3. Build training set as y = actual generation on date, X features including weather data for that date

### 1

- National grid wind availabilty predictions, extract the unique BMU_Ids &#9745;
- Build Dictionary of locations with ids as key - csv as ./locations.csv
    - Inital results in 'initialLocations.csv'
    - Then refined the BMUID list and locations to Onshore farms due to access to weather data, and removed ids where a farm name and location could not be found.
    - We also added the capacity of each farm
    - This took inital 194 locations down to 99 farms
    -https://www.thewindpower.net/windfarm_en_1357_braes-of-doune.php acted as useful source for location info


### 2

- Need to select a set of dates randomly within a range to make up sample for data set
- With dates selected, sample generation at 4 key points through out day
    - 00:00, 06:00, 12:00, 18:00
- Get weather for location of generator at the 4 points throughout day
```HTTPRequest
GET https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/!!_____LOCATION HERE_____!!/2021-11-11/2021-11-11?unitGroup=uk&key=6FF9G8N2T2NZCSR2C2WPZ42QF&include=hours
```

- Ran into unexpected issues which meant a reduced dataset taking from 99 to 55 locations, upon discovery that not all locations had 'actual generation' data that could be sourced, and the balancing data which was tried instead was not useful as a metric as it was based on predictions of what the output would be rather than the actual generation.

- There is a potential limitation in this study also brought up in the fact that some generators have capacity to limit generation based on demand. Due to wind being inconsistent anyway from what we have read it it appears that the are mostly allowed to run at what ever capacity is possible so it should not interfer with this investigation.

### 3

- Accuracy not as good as expected, however is understandable as differetn aspects have introduced some uncertainty and thus the achieved accuracy is acceptable as long as handled and disscssed appropritately.