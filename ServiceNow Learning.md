# Performance Analystic

## Indicator sources   
  1. used by indicators to calculate scores
  2. specify a table and the frequency for collecting data
  3. include filter conditions to limit the record set
  4. may be used by multiple indicators

## Indicators
  1. known as Key Performance Indicators (KPI)
  2. Three types
     1. **Automated**: Scores are automatically collected using scheduled data collection jobs
     2. **Manual**: Scores are entered manually or imported from a third-party source
     3. **Formula**: Scores are calculated using scores of other indicators

  3. Automated indicators save scores from an indicator source at a regular frequency such as daily, weekly, or monthly.

## Indicator scores 
  1. KPI values

## Breakdowns
  1. filter or group indicator scores for more detailed analysis and groups the scores by the breakdown elements
  2. For example a country breakdown breaks down the indicator source into individual country records (breakdown elements)
  3. Three types
     1. **Automated**: A breakdown source determines selectable elements
     2. **Manual**: Developers define the breakdown elements and the indicator scores for each element manually instead of using records from a breakdown source
     3. **External**: A JDBC data source and SQL statement retrieve breakdown elements

## Breakdown Sources
  1. Breakdown sources specify which unique elements a breakdown contains
  2. A breakdown source is a set of records from a table or database view

## Data Collectors
  1. Scheduled jobs that collect data from indicator sources
  2. Create data collectors after defining indicator sources, breakdown sources, and indicators
  3. To analyze data that existed prior to setting up Performance Analytics, use a historical data collector to collect scores and snapshots for existing records


## Analytics Hub 
  1. the user interface for assessing, comparing, and predicting the performance of indicators over time
  2. displays data for a single indicator



# ServiceNow Mobile Development
## Applet Launcher
  1. also known as a landing screen
  2. a user can access. Access Applet Launchers as tabs on the Navigation Bar
  3. Applet Launchers contain a header and the applets a user is authorized to use

## Applet Launcher Section
  1. Applet Launcher sections arrange applets and record information into logical groupings


## Applet
  1. Applets are small applications that run within another application
  2. Mobile Applications are made up of applets which are logically grouped into Applet Launchers

## Screen
  1. Tapping an applet opens a screen such as a List screen or a Form screen.
  2. Screen can have other types like: Employee Directory, Map, Group List, Calendar, URL

## Action item
  1. defines a database operation allowed on a table's records
  2. Guided App Creator creates New, Edit, and Delete Action Items for each table in a mobile application.
  3. Each field value is populated by an Action Item Parameter variable
  4. Developers must update the data type for the Action Item Parameter to match the data type of the field as closely as possible

## Data item
  1. defines which records an applet has access to
  2. are filtered sets of data from a table. e.g. All earthquakes in Asia, All earthquakes last year

## Function
  1. determine how a user navigates through an app
  2. define UI elements that determine what a user can do in a ServiceNow mobile application
  3. Three types: Actions, Navigation, and Smart buttons

## Action Function / Action Function Item
  1. Action Items define a database operation allowed on a table's records
  2. Action Items can create, update, or delete a record
  3. When creating Update Action Items, the Use current record as condition option must be selected if the change is only for the current record. Update Action Items without the Use current record as condition option selected update all records on the table.
  4. Actions encapsulate Action Items into buttons.
  5. UI Parameters prompt users to enter values. Action Parameter Mappings map user-entered values to parameters in Action items.


## Parameter
  1. Parameters allow users to pass values to a Data Item to filter records before the Data Item sends records to an applet
  2. Data Item parameters prevent sending unnecessary records to an applet
  3. Data Item Parameters create variables that work with applet UI Parameters to allow users to dynamically filter the data sent to an applet


## Screen UI Policies
  1. Screen UI Policies are client-side logic that control which fields are visible or mandatory on mobile app screens.
