---
title: Azure Stack Hub 的 Get-VirtualDisk 特权终结点
description: PowerShell Azure Stack 特权终结点参考 - Get-VirtualDisk
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: b94f696dd66b32e509b36cd3f1c49103526f1ae5
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096862"
---
# <a name="get-virtualdisk"></a>Get-VirtualDisk

## <a name="syntax"></a>语法

```
Get-VirtualDisk [-NoRedundancy] [-MaskingSet <Object>] [-StorageEnclosure <Object>]
 [-SourceVirtualDisk <Object>] [-StoragePool <Object>] [-StorageRack <Object>] [-InitiatorPort <Object>]
 [-OtherUsageDescription <Object>] [-TargetPort <Object>] [-PhysicalRangeMax <Object>]
 [-PhysicalRangeMin <Object>] [-StorageNode <Object>] [-StorageScaleUnit <Object>] [-IsSnapshot <Object>]
 [-PhysicalDisk <Object>] [-CimSession <Object>] [-Disk <Object>] [[-FriendlyName] <Object>]
 [-TargetVirtualDisk <Object>] [-StorageSubSystem <Object>] [-Usage <Object>] [-HealthStatus <Object>]
 [-StorageTier <Object>] [-UniqueId <Object>] [-StorageChassis <Object>] [-InitiatorId <Object>]
 [-AssociatedObjects] [-Name <Object>] [-ThrottleLimit <Object>] [-AsJob] [-StorageJob <Object>]
```

## <a name="parameters"></a>parameters

### <a name="-asjob"></a>-AsJob

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-associatedobjects"></a>-AssociatedObjects

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-cimsession"></a>-CimSession

```yaml
Type: Object
Parameter Sets: (All)
Aliases: Session

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-disk"></a>-Disk

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-friendlyname"></a>-FriendlyName

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-healthstatus"></a>-HealthStatus

```yaml
Type: Object
Parameter Sets: (All)
Aliases:
Accepted values: Healthy, Warning, Unhealthy, Unknown

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-initiatorid"></a>-InitiatorId

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-initiatorport"></a>-InitiatorPort

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-issnapshot"></a>-IsSnapshot
```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-maskingset"></a>-MaskingSet

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-name"></a>-Name

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-noredundancy"></a>-NoRedundancy

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-otherusagedescription"></a>-OtherUsageDescription

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-physicaldisk"></a>-PhysicalDisk

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-physicalrangemax"></a>-PhysicalRangeMax

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-physicalrangemin"></a>-PhysicalRangeMin
```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-sourcevirtualdisk"></a>-SourceVirtualDisk
```yaml
Type: Object
Parameter Sets: (All)
Aliases: VirtualDisk

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagechassis"></a>-StorageChassis
```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storageenclosure"></a>-StorageEnclosure

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagejob"></a>-StorageJob
```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagenode"></a>-StorageNode

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagepool"></a>-StoragePool

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagerack"></a>-StorageRack

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagescaleunit"></a>-StorageScaleUnit

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagesubsystem"></a>-StorageSubSystem

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-storagetier"></a>-StorageTier

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-targetport"></a>-TargetPort

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-targetvirtualdisk"></a>-TargetVirtualDisk

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-throttlelimit"></a>-ThrottleLimit

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-uniqueid"></a>-UniqueId

```yaml
Type: Object
Parameter Sets: (All)
Aliases: Id

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-usage"></a>-Usage

```yaml
Type: Object
Parameter Sets: (All)
Aliases:
Accepted values: Unknown, Other, Unrestricted, ReservedForComputerSystem, ReservedForReplicationServices, ReservedForMigrationServices, LocalReplicaSource, RemoteReplicaSource, LocalReplicaTarget, RemoteReplicaTarget, LocalReplicaSourceOrTarget, RemoteReplicaSourceOrTarget, DeltaReplicaTarget, ElementComponent, ReservedAsPoolContributer, CompositeVolumeMember, CompositeVirtualDiskMember, ReservedForSparing

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="inputs"></a>输入

### <a name="systemstring"></a>System.String[]

### <a name="microsoftmanagementinfrastructureciminstance"></a>Microsoft.Management.Infrastructure.CimInstance

## <a name="outputs"></a>Outputs

### <a name="microsoftmanagementinfrastructureciminstance"></a>Microsoft.Management.Infrastructure.CimInstance

### <a name="microsoftmanagementinfrastructureciminstancerootmicrosoftwindowsstoragemsft_virtualdisk"></a>Microsoft.Management.Infrastructure.CimInstance#ROOT/Microsoft/Windows/Storage/MSFT_VirtualDisk


## <a name="next-steps"></a>后续步骤

若要了解如何访问和使用特权终结点，请参阅[使用 Azure Stack Hub 中的特权终结点](/azure-stack/operator/azure-stack-privileged-endpoint)。