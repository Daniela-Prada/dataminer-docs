---
uid: EPMManagerGeneralConfiguration
---

# General configuration EPM Manager connector

When you set up a DataMiner EPM Solution, you must configure the header of the EPM Manager connector.

The [Protocol.Type](#type-configuration) and [Protocol.Display](#display-configuration) configuration are essential for this.

## Type configuration

| Tag      | Attribute         | Value                 | Description                                                |
|----------|-------------------|-----------------------|------------------------------------------------------------|
| `<Type>` | `options`         | `disableViewRefresh`  | Disables updates of view tables and partial subscriptions. |
| `<Type>` | `databaseOptions` | `partitionedTrending` | Activates partitioning on trend tables.                    |

For example:

`<Type relativeTimers="true" options="disableViewRefresh" databaseOptions="partitionedTrending">virtual</Type>`

> [!NOTE]
> The connector is defined as virtual, as it does not require any communication settings.

> [!TIP]
> See also: [Protocol.Type](xref:Protocol.Type)

## Display configuration

| Tag         | Attribute     | Value             | Description                                        |
|-------------|---------------|-------------------|----------------------------------------------------|
| `<Display>` | `type`        | `element manager` | Enables the EPM interface.                         |
| `<Display>` | `pageOptions` | `*;CPEOnly`       | Data pages are no longer displayed in the element. |

For example:

`<Display type="element manager" pageOptions="*;CPEOnly" defaultPage="General" pageOrder="General" wideColumnPages="">`

> [!TIP]
> See also: [Protocol.Display](xref:Protocol.Display)