 public static boolean isValidJson(String json) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            JsonNode node = mapper.readTree(json);
            return true;
        } catch (JsonProcessingException e) {
            return false;
        }
    }
