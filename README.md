# mongoQuery
some complex mongo query for self learning pupose

```
let data = await eventModel.aggregate([{
    $lookup : {
        from : 'user',
        localField : 'userId',
        foreignField : '_id',
        as : 'actorData'
    }
}, {
    $unwind : '$actorData'
}, {
    $project : {
        "completePlateformCommEarning" : '$completePlateformCommEarning',
        "tipAmtCommissionEarning" : '$tipAmtCommissionEarning',
        "eventEndTime" : '$eventEndTime',
        "isSessionEnd" : '$isSessionEnd',
        "eventDate" : '$eventDate',
        "eventTime" : '$eventTime',
        "setTimeZone" : '$setTimeZone',
        "actorCountry" : "$actorData.actorDocIssueCountry",
        amountReceived : {
            $divide: [{
                $trunc: {
                    $multiply: [{$sum : ['$allTransectionAmt', '$voucherAmt']}, 100]
                }
            }, 100]
        },
        soldTickets : '$soldTickets',
        isCancelled : '$isCancelled',
        tipAmt : '$tipAmt',
        voucherAmt : '$voucherAmt' ,
        eventId : '$randomEventId',
        eventName : '$eventTitle',
        evetDateTime : '$eventDateAndTime',
        actorName : '$creatorName',
        commision : '$commision',
        tipAmtActorEarning : "$tipAmtActorEarning",
        allTransectionAmt : {
             $divide: [{
                $trunc: {
                    $multiply: [{$sum : ['$allTransectionAmt', '$voucherAmt']}, 100]
                }
            }, 100]
        },
        artistAmt : {
            $divide: [{
                $trunc: {
                    $multiply: ["$earnedAmount", 100]
                }
            }, 100]
        },
        paymentMailEnable : '$paymentMailEnable',
        emailSent : '$paymentMailEnable',
        paymentMailSent : '$paymentMailSent'
    }
}, {$match : query}])
```
