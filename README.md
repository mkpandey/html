public Optional<Customer> findByType(String type, String identifier) {
        switch (type) {
            case "cust":
                return findByCustId(identifier);
            case "profile":
                return findByProfileId(identifier);
            case "vin":
                return findByVinId(identifier);
            default:
                return Optional.empty();
        }
    }
