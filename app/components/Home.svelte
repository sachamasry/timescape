<page>
  <gridLayout backgroundColor="#000" color="#eee">
        <label class="info">
            <formattedString>
                <span text="{message}" />
                <span text=" {minute_block_string}" />
                <span text="{time_joiner_string}" />
                <span text=" {hour_string}" />
                <span text=" {hour_terminator_string}" />
                <span text=" {minutes_display}" />
            </formattedString>
        </label>

        <label>
          <formattedString>
          </formattedString>
        </label>

        <label>
          <formattedString>
          </formattedString>
        </label>
    </gridLayout>
</page>

<script lang="ts">
let hours_in_words = ["twelve", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve"];

let minute_block_in_words = ["", "five", "ten", "quarter", "twenty", "twenty-five", "half", "thirty-five", "twenty", "quarter", "ten", "five"];

let current_time = new Date();

let message: string = "it is"
let minute_block_string = get_minute_block_string(current_time);
let minutes_display = get_minutes_string(current_time);
let time_joiner_string = get_time_joiner_string(current_time);
let hour_string = get_hour_string(current_time);
let hour_terminator_string = get_on_the_hour(current_time);
let seconds_string = current_time.getSeconds().toString();

var x = setInterval(() => {
  current_time = new Date();
  minute_block_string = get_minute_block_string(current_time);
  minutes_display = get_minutes_string(current_time);
  time_joiner_string = get_time_joiner_string(current_time);
  hour_string = get_hour_string(current_time);
  hour_terminator_string = get_on_the_hour(current_time);
  seconds_string = current_time.getSeconds().toString();
}, 1000)


function get_hour_string(date) {
  let hour = date.getHours() + get_hour_adder(date);

  return hours_in_words[get_hour_in_non_military_format(hour)];
}

function get_hour_in_non_military_format(hour) {
  if (hour > 12) {
    return (hour - 12)
  } else if (hour >=0 && hour <= 12) {
    return hour
  } else {
    return false
  }
}

function get_minutes_breakdown(date) {
  let minutes = date.getMinutes();

  let five_minute_block =
    Math.floor(minutes / 5);

  let minutes_past_block = minutes % 5

  return {
    block: five_minute_block,
    minutes: minutes_past_block
  }
}

function get_hour_adder(date) {
  if (get_minutes_breakdown(date).block > 7) {
    return 1
  } else {
    return 0
  }
}

function get_minute_block_string(date) {
  return minute_block_in_words[get_minutes_breakdown(date).block];
}

function get_minutes_string(date) {
  let minutes_display = "";
  for (i = 1; i <= get_minutes_breakdown(current_time).minutes; i += 1) {
    minutes_display = minutes_display + "· "
  }
  return minutes_display;
}

function get_time_joiner_string(date) {
  if (get_minutes_breakdown(date).block === 0) {
    return ""
  } else if (get_minutes_breakdown(date).block <= 6){
    return " past"
  } else {
    return " to"
  }
}

function get_on_the_hour(date) {
  if (get_minutes_breakdown(date).block === 0) {
    return "o'clock"
  } else {
    return ""
  }
}
</script>

<style>
    .info .fas {
        color: #3A53FF;
    }

    .info {
        font-size: 20;
        horizontal-align: center;
        vertical-align: center;
    }
</style>
