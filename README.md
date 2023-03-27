# mongoQuery
some complex mongo query for self learning pupose

# 1. Fix after decimal value
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

# 2. Condition in mongo query
```
let paymentData = await bookingModel.aggregate([{$lookup : {
    from : 'eventDetails',
    localField : 'eventId',
    foreignField : '_id',
    as : 'eventData'
}
},{$unwind : '$eventData'}, {$lookup : {
    from : 'user',
    localField : 'eventData.userId',
    foreignField : '_id',
    as : 'actorData'
}},
{$unwind : '$actorData'},
{$lookup : {
    from : 'cardDetails',
    localField : 'cardId',
    foreignField : '_id',
    as : 'cardData'
}}, {$unwind : '$cardData'},
{$project : {
    "actorEarning" : {
        $cond : {
            if : {$eq : ['CY', "$actorData.companyEstablishmentCountry"]},
            then :  { '$add' : [ '$actorEarning', {'$multiply' : [ '$actorEarning', 0]}] },
            else : '$actorEarning'
        }
    },
    "cardType" : '$cardData.cardType',
    "paymentId" : '$transectionId',
    "transectionTimestamp" : "$transectionTimestamp",
    "method" : "$cardData.cardType",
    "amountReceived" : "$totalPrice",
    "isVoucherApplied" : '$isVoucherApplied',
    "voucherAmt" : '$voucherAmt',
    'createdAt' : '$createdAt',
    "eventId" : "$eventData.randomEventId",
    "uniqueEventId" : '$eventData._id',
    "commision" : "$eventData.commision",
    "eventName" : "$eventData.eventTitle",
    "eventDateTime" : "$eventData.eventDateAndTime",
    "artistName" : "$actorData.firstName",
    "artistLastName" : "$actorData.lastName",
    "artistAmount" : '0',
    "emailSent" : 'false',
    "paymentType" : 'Ticket purchase'

}}, {$match : query}])

```


# 3. If Condition not matched return data with geonear query

```
db.serviceIndividual.aggregate([
      {
        $geoNear: {
                    near: {
                        type: "Point",
                        coordinates: [77.1025, 28.7041 ]
                    },
                   distanceField: "distance",
                   maxDistance: 5000,
                   spherical: true
                }   
    },
  
    {
        $lookup : {
            from : 'providerService',
            localField : 'serviceType',
            foreignField : '_id',
            as : 'serviceData'
        }
    },
    {$unwind :{
        "path": "$serviceData",
        "preserveNullAndEmptyArrays": true
   
    }

    },
    {
        $lookup : {
            from : 'service',
            localField : 'serviceData.subCategoryId',
            foreignField : '_id',
            as : 'subCatData'
        }
    },
    
    {$unwind : {
        "path": "$subCatData",
        "preserveNullAndEmptyArrays": true
    }},
    
    ])
```
