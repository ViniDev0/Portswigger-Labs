
## Step No 1

When facing a Error based SQLi, trying to disclosure what kind of payload the website/application usually sends to the database is fundamental.

For that i'm using the *trackingId* as my way to exchange information.

*In Burpsuite, after intercepting the comms, i got a HTTP response and send it to the repeater*

*Then:*

> `TrackingId=ogAZZfxtOKUELbuJ'`

*A HTTP 500 error internal code is delivered to me*

*Inside the body of the req, i can find what kind and what caused the error*

![[Error_Res_trackingId.png]]

*This error gives me the exact type of information the application sends to the database.*

*After ignoring the rest of the payload using the **--***

*The error suddenly is no longer with us.*

*After that using the CAST(), i can obtain more information about the database and start to find more information about the administrator*

*Using:* 

> TrackingId=LsK7qhEOr75X95Iq'AND CAST((SELECT 1) AS int)--

*Another type of error is showed*

![[Boolean_Error_Res.png]]

*To solve that we add to the CAST() operator an equals comparison*

> TrackingId=LsK7qhEOr75X95Iq'AND 1=CAST((SELECT 1) AS int)--

*Finally, the error is gone. A 200 status code showed up, but without information, nothing is done.*

*For that we start fuzzing into the database using the CAST(), keeping the equals comparison and selecting the information we need (administrator password)*

*Now, we can continue to collect the administrator info using the SELECT and FROM (Limiting the response to 1 at a time)*

> TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--

*Finally we have our needed information, and we can close the lab by log in.*


