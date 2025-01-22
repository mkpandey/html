  @Aggregation(pipeline = {
            "{ $match: { 'msgUuId': ?0 } }",              
            "{ $unwind: '$notes' }",                      
            "{ $sort: { 'notes.createdate': 1 } }",      
            "{ $group: { _id: '$_id', notes: { $push: '$notes' } } }"  
    })
