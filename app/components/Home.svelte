<page>
  <gridLayout backgroundColor="#000" color="#eee">
        <label class="info">
            <formattedString>
                <span text="{message}" />
                <span text=" {get_minutes_string(current_time)}" />
                <span text=" {time_joiner_string(current_time)}" />
                <span text=" {get_hour_string(current_time)}" />
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

let minute_block_in_words = ["o'clock", "five", "ten", "quarter", "twenty", "twenty-five", "half", "twenty-five", "quarter", "ten", "five"];

let hour_prefix = "";

let hour_suffix = "";

let current_time = new Date;

function get_hour_string(date) {
  let hour = date.getHours() + get_hour_adder(date);

  return hours_in_words[get_hour_in_non_military_format(hour)];
}

function get_hour_in_non_military_format(hour) {
  if (hour >= 0 && hour > 12) {
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
  if (get_minutes_breakdown(date).block > 6) {
    return 1
  } else {
    return 0
  }
}

function get_minutes_string(date) {
  return minute_block_in_words[get_minutes_breakdown(date).block];
}

function time_joiner_string(date) {
  if (get_minutes_breakdown(date).block > 6) {
    return "to"
  } else {
    return "past"
  }
}

    let message: string = "it is"
let hour_string = {get_hour_string(current_time)}
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
