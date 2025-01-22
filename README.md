 @Aggregation(pipeline = {
            "{ $match: { 'id': ?0 } }",                // Match the Report by reportId
            "{ $project: { notes: 1 } }",              // Project only the "notes" field
            "{ $unwind: '$notes' }",                   // Unwind the "notes" array to sort
            "{ $sort: { 'notes.createdate': 1 } }",    // Sort notes by createdate (ascending)
            "{ $group: { _id: '$_id', notes: { $push: '$notes' } } }"  // Group notes back into an array
    })
