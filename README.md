## Docker Cheat Sheet

<b>Run HR Test Data</b></br>
docker pull frontiersoftware/bre-testdata:hr-develop.latest
docker run -it --rm -p "8005:7005" -p "8021:7021" frontiersoftware/bre-testdata:hr-develop.latest

<b>Run GSA Test Data</b></br>
docker pull frontiersoftware/bre-testdata:hr-gsa-develop.latest
docker run -it --rm -p "8005:7005" -p "8021:7021" frontiersoftware/bre-testdata:hr-gsa-develop.latest

<b>Run ichris browser Test Data<br></b>
docker pull frontiersoftware/bre-testdata:develop.latest
docker run -it --rm -p "8005:7005" -p "8021:7021" frontiersoftware/bre-testdata:develop.latest

<b>Run Latest BRE against local DAT folder<br></b>
docker pull frontiersoftware/bre-testdata:hr-forupdate-develop.latest
docker run -it --rm -p "8005:7005" -p "8021:7021" -v "$(pwd)\dat:C:\Frontier\Production\chris21\dat" frontiersoftware/bre-testdata:hr-forupdate-develop.latest

<b>Get Latest Password<br></b>
https://frontiersoftware.visualstudio.com/FrontierSoftware/_wiki/wikis/DevOps%20Docs/1474/AzureDevTest



## Adding Payslip Options to HR Admin Settings
1/ Admin Options are stored in the SQL Database.<br>
2/ ApplicationServices is the gateway for HR to access this SQL Data<br>
3/ HR has a ApplicationSettingsService which posts to applicationsettings/getClientSettingValue<br>
4/ This hits AppServices in public IHttpActionResult GetClientSettingLayoutValueList() where the values are returned.<br>
5/ AppServices -> public List<ClientSetting> GetClientSettingsList(string clientId) is where the data is loaded for the given client ID<br>
public class ApplicationSettingsController<br>
6/ ClientSettings table is loaded into AppServices (step 5/ into  public virtual DbSet<ClientSetting> ClientSettings { get; set; }<br>
So can add a record to ClientSettings for PayslipSettings and it will auto load into AppServices.<br>
7/ HR populates Administration Setting screen in preferences-screen-administration.html<br>
preferences-screen.controller.js is loading Administration settings from applicationSettingsService.getLayoutAndValues() in the init function<br>
<br><br>
Just insert a record in to clientsettings and it will auto populate onto the HR Administration screen with binding to SQL Database.<br>
No code changes needed. <br><br>
delete from ApplicationServices.ClientSettings  where [key] = 'PayslipSettings' <br>
insert into ApplicationServices.ClientSettings <br>
values ('F6CFCC0E-3227-45BE-A3B4-12344CA95F16', 'PayslipSettings',<br>
'{"addressType": "H", "validFileTypes": null }',<br>
'{"description":"Payslip Settings","fields":[{"fieldCode":"addressType","label":"Address Type","type":10,"isEnabled":true}]}');<br><br>

Running ApplicationServices from Visual Studio<br>
1/ Load ApplicationServices into VS.<br>
2/ Find the app pool of the instance of ApplicationService in IIS you will replace for Debugging<br>
Right CLick on ApplicationServices -> Manage Application -> Advanced Settings shows the Application Poools<br>
3/ In VS. Click Debug -> Attach To Process and find Process = w3wp.exe and User Name = 'FrontierAppPool_HR21V5'<br>
Application Services will start runing from VS in place of the default in IIS<br>
Set break points. eg. public IHttpActionResult GetClientSettingLayoutValueList()<br>

            
<br>
<hr>

Latest Password for e2e test data<br>
https://frontiersoftware.visualstudio.com/FrontierSoftware/_wiki/wikis/DevOps%20Docs/1474/AzureDevTest
<br><br>Test Data used by Luci/Karen<br>https://athena-test.ftrdevops.com/

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
