---
uid: EPMManagerChainsAndFields
---

# Chains and fields configuration

Within the EPM Manager connector, you will find a `<Chains>` tag that contains a number of `<Chain>` children. Each `<Chain>` represents a topology view visible in DataMiner.

For example:

```xml
<Chains>
    <Chain name="Customer Topology">
        <Field name="Customer" options="displayInFilter;showCPEChilds;ignoreEmptyFilterValues;tabs:3500-KPI;details:3500;ShowBubbleupAndInstanceAlarmLevel" pid="3502"/>
        <Field name="Device" options="displayInFilter;ignoreEmptyFilterValues;tabs:2500-KPI;details:2500;ShowBubbleupAndInstanceAlarmLevel" pid="2501"/>
    </Chain>
</Chains>
```

Within a `<Chain>`, you can define multiple fields. Each field represents a level from the respective topology.

For example:

```xml
<Field name="Customer" options="displayInFilter;showCPEChilds;ignoreEmptyFilterValues;tabs:3500-KPI;details:3500;ShowBubbleupAndInstanceAlarmLevel" pid="3502"/>
```

The `<Field>` tag has the following attributes:

| Attribute | Description                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
| `name`    | Specifies the name of the field representing the level.                                               |
| `options` | Used to define the topology options.                                                                  |
| `pid`     | Specifies the ID of the parameter to which the level is linked. The view table column should be used. |

Several options can be used. These are the options used in the example:

| Option                              | Description                                                                                                              |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `displayInFilter`                   | Display the field in the filter column on the left side of the EPM UI.                                                   |
| `ignoreEmptyFilterValues`           | Excludes empty values from the filter drop-down lists of the topology levels.                                            |
| `tab`                               | Display one or more links in the block and next to the filter corresponding to the field.                                |
| `detail`                            | Specify tables that will be displayed in the details pane with a filter on the selection.                                |
| `ShowBubbleupAndInstanceAlarmLevel` | Allow two alarm indications on every object: the alarm state of the object and the alarm state of all the objects below. |

> [!NOTE]
> For more info about the options, see [options attribute](xref:Protocol.Chains.Chain.Field-options).

## Visibility of chains and fields

Within the EPM Manager protocol, it is possible to configure the visibility of a chain or a field.

To enable this functionality:

- Add a `<display>` tag within the corresponding chain or field.
- Create a read-write parameter for each chain or field. This parameter will make it possible to configure the visibility of these items when necessary.

### Example parameter

```xml
   <Param id="301" trending="false" save="true">
      <Name>chainVisibilityCustomer</Name>
      <Description>Customer Topology</Description>
      <Type>read</Type>
      <Information>...</Information>
      <Interprete>...</Interprete>
      <Display>...</Display>
      <Measurement>
         <Type>discreet</Type>
         <Discreets>
            <Discreet>
               <Display>Disabled</Display>
               <Value>1</Value>
            </Discreet>
            <Discreet>
               <Display>Enabled</Display>
               <Value>2</Value>
            </Discreet>
         </Discreets>
      </Measurement>
   </Param>
   <Param id="351" setter="true">
      <Name>chainVisibilityCustomer</Name>
      <Description>Customer Topology</Description>
      <Type>write</Type>
      <Interprete>...</Interprete>
      <Display>...</Display>
      <Measurement>
         <Type>togglebutton</Type>
         <Discreets>
            <Discreet>
               <Display>Disabled</Display>
               <Value>1</Value>
            </Discreet>
            <Discreet>
               <Display>Enabled</Display>
               <Value>2</Value>
            </Discreet>
         </Discreets>
      </Measurement>
   </Param>
```

### Example chain

```xml
<Chain name="Customer Topology">
    <Display>
        <Visibility default="true">
            <Standalone pid="301">
                <Value>1</Value>
            </Standalone>
        </Visibility>
    </Display>
    <Field name="Customer" options="displayInFilter;showCPEChilds;ignoreEmptyFilterValues;tabs:3500-KPI;details:3500;ShowBubbleupAndInstanceAlarmLevel" pid="3502">
        <Display>
            <Selection>
                <Visibility default="true">
                    <Standalone pid="308">
                        <Value>1</Value>
                    </Standalone>
                </Visibility>
            </Selection>
        </Display>
    </Field>
    <Field name="Device" options="displayInFilter;ignoreEmptyFilterValues;tabs:2500-KPI;details:2500;ShowBubbleupAndInstanceAlarmLevel" pid="2501">
        <Display>
            <Selection>
                <Visibility default="true">
                    <Standalone pid="310">
                        <Value>1</Value>
                    </Standalone>
                </Visibility>
            </Selection>
        </Display>
    </Field>
</Chain>
```

#### Chain configuration

| Tag            | Attribute | Description                                                                                                                                       |
|----------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| `<Visibility>` | `default` | Specifies if the chain or field should be displayed by default (`true` or `false`).                                                               |
| `<Standalone>` | `pid`     | Specifies the ID of the configurable parameter linked to toggle visibility.                                                                       |
| `<Value>`      |           | Defines one of the possible values the parameter must have to toggle the visibility to the opposite setting in relation to the default attribute. |