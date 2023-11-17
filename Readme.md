<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128553731/14.2.5%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/E4130)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
<!-- default badges end -->

# Scheduler for ASP.NET MVC -  - How to implement the FetchAppointment delegate method

This example demonstrates how to implenent the [FetchAppointmentsMethod](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.FetchAppointmentsMethod) delegate method to dynamically limit the number of appointments loaded into the [Scheduler](https://docs.devexpress.com/AspNetMvc/11675/components/scheduler/scheduler-overview) storage. This approach can be useful when a scheduler contains a large amount of data, and only a small part of it needs to be loaded at one time.

Call [SchedulerExtension.Bind(FetchAppointmentsMethod)](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.SchedulerExtension.Bind(DevExpress.Web.Mvc.FetchAppointmentsMethod)) or [SchedulerExtension.Bind(FetchAppointmentsMethod, Object)](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.SchedulerExtension.Bind(DevExpress.Web.Mvc.FetchAppointmentsMethod-System.Object)) method to bind the Scheduler component to an appointment data source modified dynamically. 

```
@Html.DevExpress().Scheduler(SchedulerSettingsHelper.CommonSchedulerSettings).Bind(Model.FetchAppointments, Model.Resources).GetHtml()
```

The `FetchAppointmentsMethod` delegate allows you to dynamically limit the number of appointments loaded into the Scheduler storage.

```
public static SchedulerDataObject DataObject {
    get {
        SchedulerDataObject sdo = new SchedulerDataObject();
        sdo.Appointments = GetAppointments();
        sdo.Resources = GetResources();
        sdo.FetchAppointments = FetchAppointmentsHelperMethod;
        return sdo;
    }
}

public static object FetchAppointmentsHelperMethod(FetchAppointmentsEventArgs args) {
    args.ForceReloadAppointments = true;
    SchedulingDataClassesDataContext db = new SchedulingDataClassesDataContext();
    return db.DBAppointments.Where(e => e.StartDate > args.Interval.Start && e.EndDate < args.Interval.End);
}
```

The [Interval](https://docs.devexpress.com/CoreLibraries/DevExpress.XtraScheduler.TimeIntervalEventArgs.Interval) property specifies the visible time interval. You can use this argument to calculate a new time interval to query a data source for appointments. Fetched appointments are returned to the caller to populate the Scheduler.

## Files to Review

* [HomeController.cs](./CS/DevExpressMvcSchedulerFetchAppointments/Controllers/HomeController.cs) (VB: [HomeController.vb](./VB/DevExpressMvcSchedulerFetchAppointments/Controllers/HomeController.vb))
* [SchedulerDataHelper.cs](./CS/DevExpressMvcSchedulerFetchAppointments/Models/SchedulerDataHelper.cs) (VB: [SchedulerDataHelper.vb](./VB/DevExpressMvcSchedulerFetchAppointments/Models/SchedulerDataHelper.vb))
* [SchedulerSettingsHelper.cs](./CS/DevExpressMvcSchedulerFetchAppointments/Models/SchedulerSettingsHelper.cs) (VB: [SchedulerSettingsHelper.vb](./VB/DevExpressMvcSchedulerFetchAppointments/Models/SchedulerSettingsHelper.vb))

## Documentation

* [ASPxSchedulerDataWebControlBase.FetchAppointments Event](https://docs.devexpress.com/AspNet/DevExpress.Web.ASPxScheduler.ASPxSchedulerDataWebControlBase.FetchAppointments)
