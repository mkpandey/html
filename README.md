import java.time.LocalDateTime;
import java.time.ZoneOffset;
import java.time.format.DateTimeFormatter;
 
 public static String convertToUTCString(LocalDateTime localDateTime) {
        ZonedDateTime zonedDateTime = localDateTime.atOffset(ZoneOffset.UTC);
        return zonedDateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss'Z'"));
    }

    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        String utcString = convertToUTCString(now);
        System.out.println(utcString);
    }


    public static String generateRequestId() {
        return "KT" + new Random().ints(10000000, 99999999).findFirst().getAsInt();
    }
 private int counter = 1;

    public String generateOrderId() {
        return String.format("ORDER-%04d", counter++);
    }
