 HttpClient httpClient = HttpClient.create().secure(t -> t.sslContext(sslContextBuilder.build()));
        ReactorClientHttpConnector connector = new ReactorClientHttpConnector(httpClient);

        webClient = WebClient.builder()
                .baseUrl("https://your-api-endpoint")
                .clientConnector(connector)
                .build();
