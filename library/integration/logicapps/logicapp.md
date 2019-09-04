# Logic App Deployment

## Export your Logic App

After you build your logic app in your development environment, you must export it to a deployable template. This is done via `armclient`. If you do not have this installed on your PC or laptop, install it by [following these instructions](https://github.com/projectkudu/ARMClient)

Next, run the `armclient` commant to export your application as a template.

```bash
$SubscriptionId = "cecea5c9-0754-4a7f-b5a9-46ae68dcafd3"
armclient login
armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp trniel-test -ResourceGroup trniel-logicapptest -SubscriptionId $SubscriptionId -Verbose | Out-File trniel-test.json
```

## Defining the parameter file

Most parameters will be generated from the steps defined in your Logic App. These steps should remain in your template file "as is". However, you must have the following organizational parameters defined in your .params.json file.

* `location`
* `resourceGroup`
* `subscriptionId`
* `logicAppName`

## Testing the deployment
