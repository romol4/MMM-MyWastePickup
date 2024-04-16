# MMM-MyWastePickup

This a module for [MagicMirror](https://github.com/MichMich/MagicMirror).

This displays your schedule for Toronto curbside waste pickup. Forked from [jclarke0000/MMM-MyWastePickup](https://github.com/jclarke0000/MMM-MyCalendar), fixed parsing and updated to work till the end of 2024.

![Screenshot](https://private-user-images.githubusercontent.com/6601859/322709496-0752732a-d4de-4224-a8bf-664d44f21196.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTMyNDQ5MTksIm5iZiI6MTcxMzI0NDYxOSwicGF0aCI6Ii82NjAxODU5LzMyMjcwOTQ5Ni0wNzUyNzMyYS1kNGRlLTQyMjQtYThiZi02NjRkNDRmMjExOTYucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDQxNiUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA0MTZUMDUxNjU5WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OTMyMzkyYThmY2E3NmJkYTlkZWI0MmIwMTc1NTNmMDcyOWJiNWU1NzZlMmU4MWYyNGRlMTg3NmExYmEyYzllYyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.c6FiMc4jd04dEURfdtDzCbAUnrd2J-3WJTKEqDLzbX8 "Screenshot")


## Installation
1. Navigate into your MagicMirror `modules` folder and execute<br>
`git clone https://github.com/jclarke0000/MMM-MyWastePickup.git`.
2. Enter the new `MMM-MyWastePickup` directory and execute `npm install`.

## Configuration

Go to https://www.toronto.ca/services-payments/recycling-organics-garbage/houses/collection-schedule/curbside-collection-maps/
to determine your collection calendar, and configure it as below:

<table>
  <thead>
    <tr>
      <th>Option</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>collectionCalendar</code></td>
      <td><strong>REQUIRED</strong> The schedule for your curbside pickup, as dertmined above.<br><br><strong>String</strong> <code>Array</code><br />Valid values are <code>MondayNight</code>, <code>Tuesday1</code>, <code>Tuesday2</code>, <code>Wednesday1</code>, <code>Wednesday2</code>, <code>Thursday1</code>, <code>Thursday2</code>, <code>Friday1</code>, or <code>Friday2</code>.<br />It is also possible to specify <code>Custom</code> in case you want to use your own custom pickup schedule.  See below for more information about using your own schedule.<br /><br />Any other value will be ignored and the module will default to <code>Tuesday1</code>.</td>
    </tr>
    <tr>
      <td><code>weeksToDisplay</code></td>
      <td>How many weeks into the future to show collection dates.<br /><br /><strong>Number</strong><br />Default: <code>2</code>.</td>
    </tr>
    <tr>
      <td><code>limitTo</code></td>
      <td>Limit the display to the spcified number of pickups.<br /><br /><strong>Number</strong><br />Default: <code>99</code>.</td>
    </tr>
  </tbody>
</table>

### Example config

```
{
  module: 'MMM-MyWastePickup',
  position: 'top_left',
  header: 'My Waste Collection',
  config: {
    collectionCalendar: 'Tuesday1'
  }
},
```

## Note

This works off of a static CSV file obtained from the city of Toronto's website here:
https://open.toronto.ca/dataset/solid-waste-pickup-schedule/

As such, this module currently only has data until the end of the year 2021.In 2022 and subsequent years, download the new
schedule and copy it over the existing schedule.csv file in this module's directory.



## Using Your Own Custom Schedule

If you live outside of Toronto and you'd like to use this module, you can create your own schedule to use with this module.

1. First, in your config specify `collectionCalendar: 'Custom'`.
2. Create a CSV based on the following template:

```
Calendar,WeekStarting,GreenBin,Garbage,Recycling,YardWaste,ChristmasTree
Custom,03/07/18,1,0,1,0,0
Custom,03/14/18,1,1,1,0,0
Custom,03/21/18,1,0,1,0,0
Custom,03/28/18,1,1,1,0,0
```
Add lines for each pickup date as needed

The following points are very important:
* The CSV file must be delimited using commas
* The first line containing the headers must appear exactly as above.  If the module is stuck on "Loading..." after you've created your custom schedule, double-check that none of the headers are misspelled.
* You MUST use the schedule name `Custom` at the beginning of each line.
* The date format needs to be specified as `MM/DD/YY` (e.g.: 05/28/18 for 28-May-2018)
* The last five fields of each line specify whether the particular waste product is scheduled to be picked up on the given date. A value of `0` means no pick up. A value of ANYTHING ELSE means the product will be picked up.  Using the first pick up date entry in the template above, `1,0,1,0,0` means that `GreenBin` and `Recycling` will be picked up on that date, while `Garbage`, `YardWaste`, and `ChristmasTree` will not be picked up.

3. Save the file as `schedule_custom.csv` in the `MMM-MyWasteCollection` directory.
4. Restart Magic Mirror


