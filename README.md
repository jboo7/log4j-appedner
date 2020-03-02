# log4j-appedner
log4j appender implementation

The implementation of the custom log4j appender would be as follows:
1. Use the log4j2 API because it's more convinient to extends the implementations with custom appenders.
2. Extend the class AbstractAppender and name the new implementation DataSourceAppender with use of creating a data source with connections to store the passed log events.
3. I would add Hikary Pool and Postgres dependencies to handle data source connections and the db connections.
4. Implementation would have some kind of buffer to store a specific amount of events before the actual saving into db.
5. The amount of connection should be appropriate to handle huge increase of the amount of log events comming.
6. Events could be saved in different order in time (some buffer might be saving events longer than the other), but this would be an issue since we can save the datetimestamp of the message in a column to sort the messages later.
This would be a simple implementation.

Pros: simple
Cons: no tests, no integration tests, better to use the JDBC log4j appender

FTS implementation:
1. Well, first of all i would not implement the FTS by myself, because there are libraries which are already does this (Elastic, Lucene)
2. On second thought, I think Postgres has it's own implementation of FTS, so I would also see into this case.
3. If implement it by hand I would split the log messages in tokens and would build an index in the db which would connect this tokens to the log messages or events (since there are a lot of log events probably better to store token as words, since db size could increase drastically).
4. Taking into account my proposed implementation of the custom log4j appender I would process the buffered messages to update the FTS index in the db and then save the buffer.

Cons: simple and naive
Pros: Elastic is better