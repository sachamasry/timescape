<page>
    <gridLayout>
        <label class="info">
            <formattedString>
                <span text=" {message}" />
                <span text=" {get_hour_string(current_time)}" />
                <span text=" o'clock" />
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

let current_time = new Date;

function get_hour_string(date) {
  let hour = date.getHours();

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
