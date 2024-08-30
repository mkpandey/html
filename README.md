import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

String date = "2025-08-08";
String time = "04:22";

// Parse the date and time components
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
LocalDateTime dateTime = LocalDateTime.parse(date   
 + " " + time, formatter);

  LocalDateTime adjustedDateTime = dateTime.withZone(ZoneId.of("UTC+3")).toLocalDateTime();
String formattedDateTime = adjustedDateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ssXXX"));

