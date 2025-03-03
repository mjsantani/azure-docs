---
title: Application Insights transaction diagnostics | Microsoft Docs
description: This article explains Application Insights end-to-end transaction diagnostics.
ms.topic: conceptual
ms.date: 10/31/2022
ms.reviewer: sdash
---

# Unified cross-component transaction diagnostics

The unified diagnostics experience automatically correlates server-side telemetry from across all your Application Insights monitored components into a single view. It doesn't matter if you have multiple resources. Application Insights detects the underlying relationship and allows you to easily diagnose the application component, dependency, or exception that caused a transaction slowdown or failure.

## What is a component?

Components are independently deployable parts of your distributed or microservice application. Developers and operations teams have code-level visibility or access to telemetry generated by these application components.

* Components are different from "observed" external dependencies, such as SQL and event hubs, which your team or organization might not have access to (code or telemetry).
* Components run on any number of server, role, or container instances.
* Components can be separate Application Insights instrumentation keys, even if subscriptions are different. Components also can be different roles that report to a single Application Insights instrumentation key. The new experience shows details across all components, regardless of how they were set up.

> [!NOTE]
> Are you missing the related item links? All the related telemetry is on the left side in the [top](#cross-component-transaction-chart) and [bottom](#all-telemetry-with-this-operation-id) sections.

## Transaction diagnostics experience

This view has four key parts: a results list, a cross-component transaction chart, a time-sequence list of all telemetry related to this operation, and the details pane for any selected telemetry item on the left.

![Screenshot that shows the four key parts of the view.](media/transaction-diagnostics/4partsCrossComponent.png)

## Cross-component transaction chart

This chart provides a timeline with horizontal bars during requests and dependencies across components. Any exceptions that are collected are also marked on the timeline.

1. The top row on this chart represents the entry point. It's the incoming request to the first component called in this transaction. The duration is the total time taken for the transaction to complete.
1. Any calls to external dependencies are simple noncollapsible rows, with icons that represent the dependency type.
1. Calls to other components are collapsible rows. Each row corresponds to a specific operation invoked at the component.
1. By default, the request, dependency, or exception that you selected appears on the right side. Select any row to see its [details](#details-of-the-selected-telemetry).

> [!NOTE]
> Calls to other components have two rows. One row represents the outbound call (dependency) from the caller component. The other row corresponds to the inbound request at the called component. The leading icon and distinct styling of the duration bars help differentiate between them.

## All telemetry with this Operation ID

This section shows a flat list view in a time sequence of all the telemetry related to this transaction. It also shows the custom events and traces that aren't displayed in the transaction chart. You can filter this list to telemetry generated by a specific component or call. You can select any telemetry item in this list to see corresponding [details on the right](#details-of-the-selected-telemetry).

![Screenshot that shows the time sequence of all telemetry.](media/transaction-diagnostics/allTelemetryDrawerOpened.png)

## Details of the selected telemetry

This collapsible pane shows the detail of any selected item from the transaction chart or the list. **Show all** lists all the standard attributes that are collected. Any custom attributes are listed separately under the standard set. Select the ellipsis button (...) under the **Call Stack** trace window to get an option to copy the trace. **Open profiler traces** and **Open debug snapshot** show code-level diagnostics in corresponding detail panes.

![Screenshot that shows exception details.](media/transaction-diagnostics/exceptiondetail.png)

## Search results

This collapsible pane shows the other results that meet the filter criteria. Select any result to update the respective details of the preceding three sections. We try to find samples that are most likely to have the details available from all components, even if sampling is in effect in any of them. These samples are shown as suggestions.

![Screenshot that shows search results.](media/transaction-diagnostics/searchResults.png)

## Profiler and Snapshot Debugger

[Application Insights Profiler](./profiler.md) or [Snapshot Debugger](snapshot-debugger.md) help with code-level diagnostics of performance and failure issues. With this experience, you can see Profiler traces or snapshots from any component with a single selection.

If you can't get Profiler working, contact serviceprofilerhelp\@microsoft.com.

If you can't get Snapshot Debugger working, contact snapshothelp\@microsoft.com.

![Screenshot that shows Profiler integration.](media/transaction-diagnostics/profilerTraces.png)

## FAQ

This section provides answers to common questions.

### Why do I see a single component on the chart and the other components only show as external dependencies without any details?

Potential reasons:

* Are the other components instrumented with Application Insights?
* Are they using the latest stable Application Insights SDK?
* If these components are separate Application Insights resources, do you have required [access](resources-roles-access-control.md)?
If you do have access and the components are instrumented with the latest Application Insights SDKs, let us know via the feedback channel in the upper-right corner.

### I see duplicate rows for the dependencies. Is this behavior expected?

Currently, we're showing the outbound dependency call separate from the inbound request. Typically, the two calls look identical with only the duration value being different because of the network round trip. The leading icon and distinct styling of the duration bars help differentiate between them. Is this presentation of the data confusing? Give us your feedback!

### What about clock skews across different component instances?

Timelines are adjusted for clock skews in the transaction chart. You can see the exact timestamps in the details pane or by using Log Analytics.

### Why is the new experience missing most of the related items queries?

This behavior is by design. All the related items, across all components, are already available on the left side in the top and bottom sections. The new experience has two related items that the left side doesn't cover: all telemetry from five minutes before and after this event and the user timeline.

### Is there a way to see fewer events per transaction when I use the Application Insights JavaScript SDK?

The transaction diagnostics experience shows all telemetry in a [single operation](correlation.md#data-model-for-telemetry-correlation) that shares an [Operation ID](data-model-context.md#operation-id). By default, the Application Insights SDK for JavaScript creates a new operation for each unique page view. In a single-page application (SPA), only one page view event will be generated and a single Operation ID will be used for all telemetry generated. As a result, many events might be correlated to the same operation.

In these scenarios, you can use Automatic Route Tracking to automatically create new operations for navigation in your SPA. You must turn on [enableAutoRouteTracking](javascript.md#single-page-applications) so that a page view is generated every time the URL route is updated (logical page view occurs). If you want to manually refresh the Operation ID, call `appInsights.properties.context.telemetryTrace.traceID = Microsoft.ApplicationInsights.Telemetry.Util.generateW3CId()`. Manually triggering a PageView event also resets the Operation ID.

### Why do transaction detail durations not add up to the top-request duration?

Time not explained in the Gantt chart is time that isn't covered by a tracked dependency. This issue can occur because external calls weren't instrumented, either automatically or manually. It can also occur because the time taken was in process rather than because of an external call.

If all calls were instrumented, in process is the likely root cause for the time spent. A useful tool for diagnosing the process is the [Application Insights profiler](./profiler.md).

### What if I see the message ***Error retrieving data*** while navigating Application Insights in the Azure portal? 

This error indicates that the browser was unable to call into a required API or the API returned a failure response. To troubleshoot the behavior, open a browser [InPrivate window](https://support.microsoft.com/microsoft-edge/browse-inprivate-in-microsoft-edge-cd2c9a48-0bc4-b98e-5e46-ac40c84e27e2) and [disable any browser extensions](https://support.microsoft.com/microsoft-edge/add-turn-off-or-remove-extensions-in-microsoft-edge-9c0ec68c-2fbc-2f2c-9ff0-bdc76f46b026) that are running, then identify if you can still reproduce the portal behavior. If the portal error still occurs, try testing with other browsers, or other machines, investigate DNS or other network related issues from the client machine where the API calls are failing. If the portal error persists and requires further investigations, then [collect a browser network trace](../../azure-portal/capture-browser-trace.md#capture-a-browser-trace-for-troubleshooting) while you reproduce the unexpected portal behavior and open a support case from the Azure portal.