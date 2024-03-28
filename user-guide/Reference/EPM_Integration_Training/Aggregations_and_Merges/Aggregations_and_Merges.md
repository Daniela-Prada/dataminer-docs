# Aggregate Actions
Because of the way the EPM architecture is structured it allows us to calculate many aggregated KPI's. These KPI's are calculated by using [aggregation](https://docs.dataminer.services/develop/devguide/Connector/Actions/ActionAggregate.html) actions.

```xml
<Action id="8004">
    <Name>aggregateRegionNumberEndpoints</Name>
    <On id="7005">parameter</On>
    <Type options="groupby:2;type:sum;return:8004;ignoreValues:-1,2/-1;threaded;avoidZeroInResult;defaultValue:8004,-1">aggregate</Type>
</Action>
```

You are able to see what all of the options do in the documentation
Aggregation actions are used when aggregating data from within the same element i.e. the BE’s will aggregate the Region KPI’s from the Sub-Region table since all the information is in the element. 
-	The groupBy option is not necessary  if a valid relation is found but is added to reduce load.
-	NOTE: for recursive and many to many relations, aggregations may be done by removing the groupBy option.

# Merge Actions
If KPI data is located outside of the element, i.e. Network KPI’s (located at the FE) aggregated from the Region KPI’s (located at the BE level), then [merge](https://docs.dataminer.services/develop/devguide/Connector/Actions/ActionMerge.html) actions are used.

These actions are an extension of aggregation actions. A merge request is sent out to multiple elements, and then the responses are “merged” together to set the table.

```xml
<Action id="9003">
    <Name>mergeNetworkNumberEndpoints</Name>
    <On>protocol</On>
    <Type options="remoteElements:801;trigger:19003;type:sum;destination:9003;mergeResult;limitresult:9001;defaultValue:9003,-1">merge</Type>
</Action>
```

The action is like an aggregation action, but instead of being on a column where it is retrieving the data, it is on protocol. Other important options are the remoteElement points to a columnPid containing all the BE elements and the trigger option denotes a parameter ID that will be triggered to initiate the merge request. The flow of the receiver of the merge request is as follows:

1. Parameter:
```xml
<Param id="19003">
    <Name>mergeNetworkNumberEndpoints</Name>
    <Description>Merge Network Number Endpoints</Description>
    <Type>write</Type>
    <Interprete>
        <RawType>numeric text</RawType>
        <LengthType>next param</LengthType>
        <Type>double</Type>
    </Interprete>
</Param>
```
2. Trigger:
```xml
<Trigger id="19003">
    <Name>onChangeNetworkNumberEndpoints</Name>
    <On id="19003">parameter</On>
    <Time>change</Time>
    <Type>action</Type>
    <Content>
        <Id>19003</Id>
    </Content>
</Trigger>
```
3. Action: 
```xml
<Action id="19003">
    <Name>aggregateNetworkNumberEndpoints</Name>
    <On id="8004">parameter</On>
    <Type options="groupby:2;type:sum;threaded;ignoreValues:-1,2/-1">aggregate</Type>
</Action>
```

The action is an aggregation action and there is no need to add the return option since we do not want the BE’s to set their tables with this information, we only want the KPI’s to be calculated and then sent to the FE.