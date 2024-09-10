Mono<ResponseEntity<String>> responseMono = webClient.put()
        .uri("/your-resource-path/{id}", "your-resource-id")
        .contentType(MediaType.APPLICATION_JSON)
        .body(Mono.just(yourRequestBody), String.class)
        .retrieve()
        .toEntity(String.class);

ResponseEntity<String> responseEntity = responseMono.block();
if (responseEntity != null) {
    HttpStatus statusCode = responseEntity.getStatusCode();
    String responseBody = responseEntity.getBody();

    System.out.println("Status Code: " + statusCode.value());
    System.out.println("Response Body: " + responseBody);
} else {
    System.out.println("Response is null");
}
