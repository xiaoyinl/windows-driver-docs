---
title: OID_GEN_MEDIA_CONNECT_STATUS_EX
description: As a query, the OID_GEN_MEDIA_CONNECT_STATUS_EX OID returns the connection state of an interface. Windows Vista and laterSupported. NDIS 6.0 and later miniport driversNot requested. For NDIS interface providers only.
ms.assetid: 8239616c-788a-4073-8bbe-41f493a461de
ms.date: 08/08/2017
keywords: 
 -OID_GEN_MEDIA_CONNECT_STATUS_EX Network Drivers Starting with Windows Vista
ms.localizationpriority: medium
---

# OID\_GEN\_MEDIA\_CONNECT\_STATUS\_EX


As a query, the OID\_GEN\_MEDIA\_CONNECT\_STATUS\_EX OID returns the connection state of an interface.

<a href="" id="windows-vista-and-later"></a>Windows Vista and later  
Supported.

<a href="" id="ndis-6-0-and-later-miniport-drivers"></a>NDIS 6.0 and later miniport drivers  
Not requested. For NDIS interface providers only.

Remarks
-------

NDIS uses this OID to query the connection state of an [NDIS network interface](https://docs.microsoft.com/windows-hardware/drivers/network/ndis-network-interfaces2) provider. Only NDIS interface providers, and therefore not miniport drivers or filter drivers, must support this OID as an OID request.

If the query succeeds, the interface provider returns NDIS\_STATUS\_SUCCESS, and the result of the query can be one of the values in the [**NET\_IF\_MEDIA\_CONNECT\_STATE**](https://docs.microsoft.com/windows/desktop/api/ifdef/ne-ifdef-_net_if_media_connect_state) enumeration.

Miniport drivers supply the media connect status during initialization and provide updates with status indications.

To specify the connection state in a miniport driver, set the **MediaConnectState** member of the [**NDIS\_MINIPORT\_ADAPTER\_GENERAL\_ATTRIBUTES**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/ns-ndis-_ndis_miniport_adapter_general_attributes) structure that the miniport driver passes to the [**NdisMSetMiniportAttributes**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndismsetminiportattributes) function.

Requirements
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Header</p></td>
<td>Ntddndis.h (include Ndis.h)</td>
</tr>
</tbody>
</table>

## See also


[**NDIS\_MINIPORT\_ADAPTER\_GENERAL\_ATTRIBUTES**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/ns-ndis-_ndis_miniport_adapter_general_attributes)

[**NdisMSetMiniportAttributes**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ndis/nf-ndis-ndismsetminiportattributes)

[**NET\_IF\_MEDIA\_CONNECT\_STATE**](https://docs.microsoft.com/windows/desktop/api/ifdef/ne-ifdef-_net_if_media_connect_state)

[NDIS Network Interface OIDs](https://docs.microsoft.com/windows-hardware/drivers/network/ndis-network-interface-oids)

 

 




