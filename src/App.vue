<template>
	<div id="app">

		<!--div class="calendar-controls">
      <h3 class="project-info">Project Info</h3>
			<div v-if="message" class="project-container">{{ message }}
      
      </div>

      <div v-if="message_class" class="project-link">{{ message_class }}</div>
		</div-->
		<div class="calendar-parent">
			<calendar-view
        :items="items"
				:show-date="showDate"
				:time-format-options="{ hour: 'numeric', minute: '2-digit' }"
				:enable-drag-drop="true"
				:disable-past="disablePast"
				:disable-future="disableFuture"
				:show-times="showTimes"
				:display-period-uom="displayPeriodUom"
				:display-period-count="displayPeriodCount"
				:starting-day-of-week="startingDayOfWeek"
				:class="themeClasses"
				:period-changed-callback="periodChanged"
				:current-period-label="useTodayIcons ? 'icons' : ''"
				:displayWeekNumbers="displayWeekNumbers"
				:enable-date-selection="true"
				:selection-start="selectionStart"
				:selection-end="selectionEnd"
				@date-selection-start="setSelection"
				@date-selection="setSelection"
				@date-selection-finish="finishSelection"
				@drop-on-date="onDrop"
				@click-date="onClickDay"
				@click-item="onClickItem"
			>
				<calendar-view-header
					slot="header"
					slot-scope="{ headerProps }"
					:header-props="headerProps"
					@input="setShowDate"
				/>
			</calendar-view>
		</div>
	</div>
</template>
<script>
// Load CSS from the published version
require("vue-simple-calendar/static/css/default.css")
require("vue-simple-calendar/static/css/holidays-us.css")
// Load CSS from the local repo
//require("../../vue-simple-calendar/static/css/default.css")
//require("../../vue-simple-calendar/static/css/holidays-us.css")
import {
	CalendarView,
	CalendarViewHeader,
	CalendarMathMixin,
} from "vue-simple-calendar" // published version
//} from "../../vue-simple-calendar/src/components/bundle.js" // local repo
export default {
	name: "App",
	components: {
		CalendarView,
		CalendarViewHeader,
	},
	mixins: [CalendarMathMixin],
	data() {
		return {
			/* Show the current month, and give it some fake items to show */
			showDate: this.October(21),
			message: "",
			startingDayOfWeek: 3,
			disablePast: false,
			disableFuture: false,
			displayPeriodUom: "week",
			displayPeriodCount: 1,
			displayWeekNumbers: false,
			showTimes: true,
			selectionStart: null,
			selectionEnd: null,
			newItemTitle: "",
			newItemStartDate: "",
			newItemEndDate: "",
			useDefaultTheme: true,
			useHolidayTheme: true,
			useTodayIcons: false,
			items: [
        {
					id: "1_7",
          startDate: this.October(21),
          title: "<h5 class='project-title'>1-in-7 102120: Crayoninator 2000</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/crayoninator_4-768x1024.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/22/1-in-7-102120-crayoninator/' target='_blank'>View Project</a>" ,
        },
        {
					id: "2_7",
					startDate: this.October(22),
          title: "<h5 class='project-title'>2-in-7 102220: Automatic Plant Sprayer</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/IMG_0065-768x1024.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/23/2-in-7-102220-automatic-plant-sprayer/' target='_blank'>View Project</a>" ,
        },
        {
					id: "3_7",
					startDate: this.October(23),
					title: "<h5 class='project-title'>3-in-7 102320: Virtual Dance Performance Setup</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/Capture_0_-768x477.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/23/3-in-7-102220-virtual-dance-performance-setup/' target='_blank'>View Project</a>" ,
        },
        {
					id: "4_7",
					startDate: this.October(24),
					title: "<h5 class='project-title'>4-in-7 102420: Submitting 2020 Mail-In Presidential Electoral Ballot</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/InkedIMG_0076_LI-768x1024.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/25/4-in-7-102420-submitting-2020-mail-in-presidential-electoral-ballot/' target='_blank'>View Project</a>" ,
        },
        {
					id: "5_7",
					startDate: this.October(25),
					title: "<h5 class='project-title'>5-in-7 102520: Mask Survey</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/IMG_0077-768x1024.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/25/5-in-7-102520-mask-survey/' target='_blank'>View Project</a>" ,
        },
        {
					id: "6_7",
					startDate: this.October(26),
					title: "<h5 class='project-title'>6-in-7 102620: Painting</h5>\n" +
                  "<img class='project-img' src='https://cpb-us-w2.wpmucdn.com/portfolio.newschool.edu/dist/1/32191/files/2020/10/IMG_0080-768x576.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/26/6-in-7-102620-painting/' target='_blank'>View Project</a>" ,
        },
        {
					id: "7_7",
					startDate: this.October(27),
          title: "<h5 class='project-title'>7-in-7 102720: 7-in-7 Calendar</h5>\n" +
                  "<img class='project-img' src='https://portfolio.newschool.edu/dekog666/files/2020/10/3546789jk.jpg' />\n" +
                  "<a class='project-link' href='http://portfolio.newschool.edu/dekog666/2020/10/27/7-in-7-102720-7-in7-calendar/' target='_blank'>View Project</a>" ,
				},
			],
		}
	},
	computed: {
		userLocale() {
			return this.getDefaultBrowserLocale
		},
		dayNames() {
			return this.getFormattedWeekdayNames(this.userLocale, "long", 0)
		},
		themeClasses() {
			return {
				"theme-default": this.useDefaultTheme,
				"holiday-us-traditional": this.useHolidayTheme,
				"holiday-us-official": this.useHolidayTheme,
			}
		},
	},
	mounted() {
		this.newItemStartDate = this.isoYearMonthDay(this.today())
		this.newItemEndDate = this.isoYearMonthDay(this.today())
	},
	methods: {
		periodChanged() {
			// range, eventSource) {
			// Demo does nothing with this information, just including the method to demonstrate how
			// you can listen for changes to the displayed range and react to them (by loading items, etc.)
			//console.log(eventSource)
			//console.log(range)
		},
		thisMonth(d, h, m) {
			const t = new Date()
			return new Date(t.getFullYear(), t.getMonth(), d, h || 0, m || 0)
		},
		October(d){
			const t = new Date('October 21, 2020 00:00:00')
			return new Date(t.getFullYear(), t.getMonth(), d)
		},
		onClickDay(d) {
			this.selectionStart = null
			this.selectionEnd = null
			this.message = `You clicked: ${d.toLocaleDateString()}`
		},
		onClickItem(e) {
      this.message = `${e.title}`
		},
		setShowDate(d) {
			this.message = `Changing calendar view to ${d.toLocaleDateString()}`
			this.showDate = d
		},
		setSelection(dateRange) {
			this.selectionEnd = dateRange[1]
			this.selectionStart = dateRange[0]
		},
		finishSelection(dateRange) {
			this.setSelection(dateRange)
			this.message = `You selected: ${this.selectionStart.toLocaleDateString()} -${this.selectionEnd.toLocaleDateString()}`
		},
		onDrop(item, date) {
			this.message = `You dropped ${item.id} on ${date.toLocaleDateString()}`
			// Determine the delta between the old start date and the date chosen,
			// and apply that delta to both the start and end date to move the item.
			const eLength = this.dayDiff(item.startDate, date)
			item.originalItem.startDate = this.addDays(item.startDate, eLength)
			item.originalItem.endDate = this.addDays(item.endDate, eLength)
		},
		clickTestAddItem() {
			this.items.push({
				startDate: this.newItemStartDate,
				endDate: this.newItemEndDate,
				title: this.newItemTitle,
				id: "e" + Math.random().toString(36).substr(2, 10),
			})
			this.message = "You added a calendar item!"
		},
	},
}
</script>

<style>
html,
body {
	height: 100%;
	margin: 0;
	background-color: #f7fcff;
  font-family: 'Roboto', sans-serif;
}
body, div, p, h1, h2, h3, h4, h5, a  {
  font-family: 'Roboto', sans-serif;
}
#app {
	display: flex;
	flex-direction: row;
	font-family: 'Roboto', sans-serif;
	width: 95vw;
	min-width: 30rem;
	max-width: 100rem;
	min-height: 40rem;
	margin-left: auto;
	margin-right: auto;
}
.calendar-controls {
	margin-right: 1rem;
	min-width: 14rem;
	max-width: 14rem;
}
.calendar-parent {
	display: flex;
	flex-direction: column;
	flex-grow: 1;
	overflow-x: hidden;
	overflow-y: hidden;
	max-height: 80vh;
	background-color: white;
}
/* For long calendars, ensure each week gets sufficient height. The body of the calendar will scroll if needed */
.cv-wrapper.period-month.periodCount-2 .cv-week,
.cv-wrapper.period-month.periodCount-3 .cv-week,
.cv-wrapper.period-year .cv-week {
	min-height: 6rem;
}
/* These styles are optional, to illustrate the flexbility of styling the calendar purely with CSS. */
/* Add some styling for items tagged with the "birthday" class */
.theme-default .cv-item.birthday {
	background-color: #e0f0e0;
	border-color: #d7e7d7;
}
.theme-default .cv-item.birthday::before {
	content: "\1F382"; /* Birthday cake */
	margin-right: 0.5em;
}
.title, .subtitle, .project-info, .project-title {
  text-align: center;
}
.project-title {
  margin-bottom:24px;
  width: 100%;
}
.project-img {
  width: 100%;
}
.project-link {
  margin-top: 24px; 
  text-transform: uppercase;
}
.cv-item{
  white-space: normal!important;
  text-align: center;
}
</style>


