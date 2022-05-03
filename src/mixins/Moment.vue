<script>

import moment from 'moment'
export default {
    data: () => ({

    }),
    methods : {
        /* time_moment: function (date) {
            return moment(date, "HH:mm:ss").format("hh:mm a");
        },
        checkSched(schedule) {
            if(schedule.day == moment().format('dddd').toUpperCase()){
                let today = moment()
                return today.valueOf() > moment(schedule.time_start, "HH:mm:ss").valueOf() && today.valueOf() < moment(schedule.time_end, "HH:mm:ss").valueOf()
            }
            return false;
        },
        dateFromDay(day = null){
            return day != "All" ? moment().day(day).format('MM/DD/YYYY') : moment().format('MM/DD/YYYY')
        }, */
        getMonthFromDate(date) {
            // console.log(date);
            if(date == null || date == ''){
                return 'Pick a';
            } else {
                return moment(date).format('MMMM');
            }
        },
        checkIfToday(date){
            let created = moment(date)
            let today = moment().format('l')
            // console.log(created,today,created.isSame(today, 'day'))
            return created.isSame(today);
        },
        getDayFromDate(date) {
            // console.log(date);
            if(date == null || date == ''){
                return 'Date';
            } else {
                return moment(date).format('DD');
            }
        },
        getHoursFromDay(){
            
        },
        checkTimeToday(date){
            var dateToday = moment().format('L');
            var staticEndTime = "20:00";
            var staticStartTime = "6:00";
            var closing = moment(dateToday + ' ' + staticEndTime).format();
            var start = moment(dateToday + ' ' + staticStartTime).format();
            // var now = moment(date).add(12, 'hours').format();
            var now = date;
            console.log(now);
            console.log(closing);
            console.log(dateToday);
            if(now && now >= start && now <= closing){
                return false;
            } else {
                return true;
            }
        }

    }
}
</script>