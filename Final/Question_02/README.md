 Let's do that again with a slightly different crash/recover scenario for each process. With all three members (mongod's) up and running, once again:

$ mongo --shell --port 27003 a.js
> // be sure 27003 is the primary.
> // use rs.stepDown() elsewhere if it isn't.
> testRollback()

Now this time, shut down the mongod on port 27003 (in addition to the other two members being shut down by testRollback() already) before doing anything else:

$ ps -A | grep mongod
$ # should see the 27003 one running (only)
$ killall mongod
$ # wait a little for the shutdown perhaps...then:
$ ps -A | grep mongod
$ # should get that none are present…

Now restart just the 27001 and 27002 members. Wait for them to get healthy -- check this with rs.status() in the shell. Then query

> db.foo.find()

Then add another document:

> db.foo.insert( { _id : "last" } )

After this, restart the third set member (mongod on port 27003). Wait for it to come online and enter a health state (secondary or primary).

Run (on any member -- try multiple if you like) :

> db.foo.find()

You should see a difference from problem 1 in the result above.

Question: which one of the following is the true statement about mongodb's operation in these scenarios? Please select ONLY ONE of the choices below.

Note: This should have been written using radio buttons instead of check boxes. Unfortunately, we didn't catch the error until the final was released. Once students have already answered the question, changing to radio buttons is non-trivial so please just select one answer below.

* The MongoDB primary does not write to its datafiles until a majority acknowledgement comes back from the rest of the cluster.
* When 27003 was primary, that was never received for writes 7,8,9. When 27003 came back up, it transmitted its write ops that the other members had not seen yet to those members so that they would have them also.
* MongoDB preserves the order of writes in a collection in its consistency model. In this problem, 27003's oplog was effectively a "fork" and to preserve write ordering a rollback was necessary during 27003's recovery phase.

----

* The MongoDB primary does not write to its datafiles until a majority acknowledgement comes back from the rest of the cluster.
