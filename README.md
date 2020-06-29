
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

.
