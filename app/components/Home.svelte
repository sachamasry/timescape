<page>
    <rootLayout>
        <gridLayout
            columns="40, *, 40"
            rows="40, *, 40"
            backgroundColor="#000" color="#eee">
            <label row="0" column="0"
                   class="minute-pip"
                   style="margin-top: 24; margin-left: 24;"
                   textAlignment="center"
                   text="{minutes_array[0]}"
            />
            <label row="0" column="2"
                   class="minute-pip"
                   style="margin-top: 24; margin-right: 24;"
                   textAlignment="center"
                   text="{minutes_array[1]}"
            />
            <label class="info"
                   row="1"
                   column="1"
                   textTransform="uppercase"
            >
                <formattedString
                >
                    <span text="{time_message}"
                          textAlignment="center"
                          letterSpacing="2"
                    />
                </formattedString>
            </label>
            <label row="2" column="0"
                   class="minute-pip"
                   style="margin-bottom: 24; margin-left: 24;"
                   textAlignment="center"
                   text="{minutes_array[2]}"
            />
            <label row="2" column="2"
                   class="minute-pip"
                   style="margin-bottom: 24; margin-right: 24;"
                   textAlignment="center"
                   text="{minutes_array[3]}"
            />
        </gridLayout>
    </rootLayout>
</page>

<script lang="ts">
let hours_in_words = ["twelve", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve"];
let minute_block_in_words = ["", "five", "ten", "quarter", "twenty", "twenty-five", "half", "thirty-five", "twenty", "quarter", "ten", "five"];

let current_time = new Date();
let time_message_list = ["it is", "", "", "", "", ""];
let time_message = time_message_list.join(' ');
let message: string = "it is"
let minute_block_string = get_minute_block_string(current_time);
let minutes_display = get_minutes_string(current_time);
let time_joiner_string = get_time_joiner_string(current_time);
let hour_string = get_hour_string(current_time);
let hour_terminator_string = get_on_the_hour(current_time);
let minutes_array = ["", "", "", ""]

var x = setInterval(() => {
    current_time = new Date();
    minute_block_string = get_minute_block_string(current_time);
    time_message_list[1] = minute_block_string;
    time_joiner_string = get_time_joiner_string(current_time);
    time_message_list[2] = time_joiner_string;
    hour_string = get_hour_string(current_time);
    time_message_list[3] = hour_string;
    hour_terminator_string = get_on_the_hour(current_time);
    time_message_list[4] = hour_terminator_string;
    // minutes_display = get_minutes_string(current_time);
    minutes_array = get_minute_array(current_time);
    // time_message_list[5] = minutes_display;
    time_message = time_message_list.filter(entry => entry.trim()).join(' ');
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

function get_minute_array(date) {
    let minute_array = ["", "", "", ""]
    let minutes = get_minutes_breakdown(date).minutes;
    if (minutes === 0) {
        minute_array = ["", "", "", ""]
    } else if (minutes === 1) {
        minute_array = ["•", "", "", ""]
    } else if (minutes === 2) {
        minute_array = ["•", "•", "", ""]
    } else if (minutes === 3) {
        minute_array = ["•", "•", "•", ""]
    } else if (minutes === 4) {
        minute_array = ["•", "•", "•", "•"]
    }
    return minute_array;
}

function get_time_joiner_string(date) {
  if (get_minutes_breakdown(date).block === 0) {
    return ""
  } else if (get_minutes_breakdown(date).block <= 7){
    return "past"
  } else {
    return "to"
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

.info, .minute-pip {
    font-family: 'HelveticaNeue-Bold';
    font-size: 24;
    letter-spacing: 0.4rem;
}

    .info {
        horizontal-align: center;
        vertical-align: center;
    }

.minute-pip {
    height: 16;
    horizontal-align: center;
    vertical-align: center;
    width: 16;
}
</style>
