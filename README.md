
## Code lookup from DED screen.

ie. GTR:cbr="PTDlst",countrycode="AU",field="DED CODE",maxlines="40",showtranslation,usekey="1"

From DED screen the lookup button is call ng-click=vm.OnEllipsisClicked()

This function is in the **WCL project** (std-filelookup.controller.ts -> function onEllipsisClicked()) where there is a generic method for returning lookup lists like this.

```
function onEllipsisClicked() {
            if (vm.viewmodel.value.fieldType === 14) {
                init();
            }
            vm.selectedRow = null;
            vm.IsValueBeingEdited = false;
            filelookupUtilsService.showDialog(vm, vm.viewmodel.layout.SummaryList);
        }
```

## GTR Send and Receive
GTR are sent from HR via formItemSvc.get and formItemSvc.query<br>
formItemSvc is part of the WCL in the form-item.service.ts file<br>
$webapi service handles actaully sending a reciving the GTR<br>
$webapi is a reference to the  \ApplicationServices\Frontier.ApplicationServices.WebApi  project (ie. Applicaition Services).

```
window.appSettings = {
    apiVersion: 'v1',
    
    // using hr21 install with BRE Portal
     /* applicationServicesUrl: 'http://localhost/hr21/ApplicationServices/',
        clientId: '502f0dda-0463-495c-ad4e-8d669d12f0b1', */
    
    // using HR21_spa_dev install with BRE Portal    
    applicationServicesUrl: 'http://localhost/hr21_spa_dev/ApplicationServices/',
    clientId: 'f6cfcc0e-3227-45be-a3b4-12344ca95f16',  

    logonApplication: 'HR21',
    supportedInterfaceNames: [
        'ichris',
        'hr21'
    ],
    authenticationType: 'bre',
    clickSuperId: '11',
    payslipEngineId: '12',
    enableComponentListCaching: true
};
```
