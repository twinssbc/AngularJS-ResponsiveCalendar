# ui-rCalendar directive

A pure AngularJS responsive calendar directive    
If you want to only use it in Ionic framework, please check https://github.com/twinssbc/Ionic-Calendar for the Ionic specific version

# Demo
http://twinssbc.github.io/AngularJS-ResponsiveCalendar/demo/

# Usage

Bower Install: `bower install ng-responsive-calendar`

Load the necessary dependent files:

    <link rel="stylesheet" href="../lib/bootstrap/dist/css/bootstrap.css"/>
    <link rel="stylesheet" href="<bower lib installation path>/ng-responsive-calendar/dist/css/calendar.min.css"/>
    <script src="../lib/angular/angular.js"></script>
    <script src="<bower lib installation path>/ng-responsive-calendar/dist/js/calendar-tpls.min.js"></script>


Add the calendar module as a dependency to your application module:

    var myAppModule = angular.module('MyApp', ['ui.rCalendar'])

Add the directive in the html page

    <calendar calendar-mode="mode" event-source="eventSource">

# Options

* formatDay    
The format of the date displayed in the month view.    
Default value: 'dd'
* formatDayHeader    
The format of the header displayed in the month view.    
Default value: 'EEE'
* formatDayTitle    
The format of the title displayed in the month view.    
Default value: 'MMMM dd, yyyy'
* formatWeekTitle    
The format of the title displayed in the week view.    
Default value: 'MMMM yyyy, Week w'
* formatMonthTitle    
The format of the title displayed in the month view.    
Default value: 'MMMM yyyy'
* formatWeekViewHeader    
The format of the header displayed in the week view.    
Default value: 'EEE d'
* formatHourColumn    
The format of the hour column displayed in the week and day view.    
Default value: 'ha'
* calendarMode    
The initial mode of the calendar.    
Default value: 'month'
* showWeeks    
If set to true, a week number column will be displayed in the month view.       
Default value: false
* showEventDetail    
If set to true, when selecting the date in the month view, the events happened on that day will be shown below.    
Default value: true
* startingDay    
Control month view starting from which day.    
Default value: 0
* eventSource    
The data source of the calendar, when the eventSource is set, the view will be updated accordingly.    
Default value: null    
The format of the eventSource is described in the EventSource section
* queryMode    
If queryMode is set to 'local', when the range or mode is changed, the calendar will use the already bound eventSource to update the view    
If queryMode is set to 'remote', when the range or mode is changed, the calendar will trigger a callback function rangeChanged.    
Users will need to implement their custom loading data logic in this function, and fill it into the eventSource. The eventSource is watched, so the view will be updated once the eventSource is changed.    
Default value: 'local'
* rangeChanged    
The callback function triggered when the range or mode is changed if the queryMode is set to 'remote'

        $scope.rangeChanged = function (startTime, endTime) {
            Events.query({startTime: startTime, endTime: endTime}, function(events){
                $scope.eventSource=events;
            });
        };

* eventSelected    
The callback function triggered when an event is clicked

        <calendar ... event-selected="onEventSelected(event)"></calendar>


        $scope.onEventSelected = function (event) {
            console.log(event.title);
        };

* timeSelected
The callback function triggered when a date or time is selected

        <calendar ... time-selected="onTimeSelected(selectedTime)"></calendar>

        $scope.onTimeSelected = function (selectedTime) {
            console.log(event.selectedTime);
        };

* translations
An object with your translations.
Default value: `{ noEvents: 'No Events'}'`

# EventSource

EventSource is an array of event object which contains at least below fields:

* title
* startTime    
If allDay is set to true, the startTime has to be as a UTC date which time is set to 0:00 AM, because in an allDay event, only the date is considered, the exact time or timezone doesn't matter.    
For example, if an allDay event starting from 2014-05-09, then startTime is

        var startTime = new Date(Date.UTC(2014, 4, 8));

* endTime    
If allDay is set to true, the startTime has to be as a UTC date which time is set to 0:00 AM, because in an allDay event, only the date is considered, the exact time or timezone doesn't matter.    
For example, if an allDay event ending to 2014-05-10, then endTime is

        var endTime = new Date(Date.UTC(2014, 4, 9));

* allDay    
Indicates the event is allDay event or regular event

**Note**
In the current version, the calendar controller only watches for the eventSource reference as it's the least expensive.
That means only you manually reassign the eventSource value, the controller get notified, and this is usually fit to the scenario when the range is changed, you load a new data set from the backend.
In case you want to manually insert/remove/update the element in the eventSource array, you can call broadcast the 'eventSourceChanged' event to notify the controller manually.

# Events

* changeDate
When receiving this event, the calendar will move the current view to previous or next range.  
Parameter: direction  
1 - Forward  
-1 - Backward

        $scope.$broadcast('changeDate', 1);

* eventSourceChanged
This event is only needed when you manually modify the element in the eventSource array.  
Parameter: value  
The whole event source object

        $scope.$broadcast('eventSourceChanged',$scope.eventSource);
