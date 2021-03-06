---
title: TVOT\_3STATES
description: TVOT\_3STATES
ms.assetid: baa9fa5e-521e-446b-bd6c-7910d61c7764
keywords: ["TVOT_3STATES Print Devices"]
topic_type:
- apiref
api_name:
- TVOT_3STATES
api_location:
- compstui.h
api_type:
- HeaderDef
ms.date: 11/28/2017
ms.localizationpriority: medium
---

# TVOT\_3STATES


## <span id="ddk_tvot_3states_gg"></span><span id="DDK_TVOT_3STATES_GG"></span>


The TVOT\_3STATES option type consists of three radio buttons inside a group box.

<span id="OPTITEM_Structure"></span><span id="optitem_structure"></span><span id="OPTITEM_STRUCTURE"></span>[**OPTITEM**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_optitem) Structure  

<span id="Sel_pSel"></span><span id="sel_psel"></span><span id="SEL_PSEL"></span>**Sel/pSel**  
Index into the [**OPTPARAM**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_optparam) array that is pointed to by the **pOptParam** member of the option's [**OPTTYPE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_opttype) structure. This specifies the currently selected option parameter.

<span id="OPTPARAM_Structure_Array__pOptParam_member_of_OPTTYPE_"></span><span id="optparam_structure_array__poptparam_member_of_opttype_"></span><span id="OPTPARAM_STRUCTURE_ARRAY__POPTPARAM_MEMBER_OF_OPTTYPE_"></span>[**OPTPARAM**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_optparam) Structure Array (**pOptParam** member of [**OPTTYPE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_opttype))  

<span id="pData"></span><span id="pdata"></span><span id="PDATA"></span>**pData**  
**pOptParam**\[0\]-&gt;**pData** points to a text string describing state 1, used as a button label.

**pOptParam**\[1\]-&gt;**pData** points to a text string describing state 2, used as a button label.

**pOptParam**\[2\]-&gt;**pData** points to a text string describing state 3, used as a button label.

<span id="IconID"></span><span id="iconid"></span><span id="ICONID"></span>**IconID**  
**pOptParam**\[0\]-&gt;**IconID** identifies an icon to be associated with state 1.

**pOptParam**\[1\]-&gt;**IconID** identifies an icon to be associated with state 2.

**pOptParam**\[2\]-&gt;**IconID** identifies an icon to be associated with state 3.

<span id="lParam"></span><span id="lparam"></span><span id="LPARAM"></span>**lParam**  
Not used.

<span id="OPTTYPE_Structure"></span><span id="opttype_structure"></span><span id="OPTTYPE_STRUCTURE"></span>[**OPTTYPE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_opttype) Structure  

<span id="Type"></span><span id="type"></span><span id="TYPE"></span>**Type**  
TVOT\_3STATES

<span id="Count"></span><span id="count"></span><span id="COUNT"></span>**Count**  
3

<span id="Style"></span><span id="style"></span><span id="STYLE"></span>**Style**  
Not used.

<span id="BegCtrlID"></span><span id="begctrlid"></span><span id="BEGCTRLID"></span>**BegCtrlID**  
If **pDlgPage** in [**COMPROPSHEETUI**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_compropsheetui) identifies a CPSUI-supplied page, or if **DlgTemplateID** in [**DLGPAGE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/compstui/ns-compstui-_dlgpage) identifies a CPSUI-supplied template, **BegCtrlID** is not used.

Otherwise, **BegCtrlID** must contain the first control identifier of a sequentially numbered set of control identifiers. Control identifiers must identify the following Windows controls:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Control Identifier</th>
<th>Windows Control</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>BegCtrlID</strong> contents</p></td>
<td><p>Group box</p></td>
</tr>
<tr class="even">
<td><p><strong>BegCtrlID</strong> contents+1</p></td>
<td><p>Title text</p></td>
</tr>
<tr class="odd">
<td><p><strong>BegCtrlID</strong> contents+2</p></td>
<td><p>State 1 radio button</p></td>
</tr>
<tr class="even">
<td><p><strong>BegCtrlID</strong> contents+3</p></td>
<td><p>State 1 icon</p></td>
</tr>
<tr class="odd">
<td><p><strong>BegCtrlID</strong> contents+4</p></td>
<td><p>State 2 radio button</p></td>
</tr>
<tr class="even">
<td><p><strong>BegCtrlID</strong> contents+5</p></td>
<td><p>State 2 icon</p></td>
</tr>
<tr class="odd">
<td><p><strong>BegCtrlID</strong> contents+6</p></td>
<td><p>State 3 radio button</p></td>
</tr>
<tr class="even">
<td><p><strong>BegCtrlID</strong> contents+7</p></td>
<td><p>State 3 icon</p></td>
</tr>
<tr class="odd">
<td><p><strong>BegCtrlID</strong> contents+8</p></td>
<td><p>Extended check box or extended push button (optional)</p></td>
</tr>
<tr class="even">
<td><p><strong>BegCtrlID</strong> contents+9</p></td>
<td><p>Extended check box or extended push button icon (optional)</p></td>
</tr>
</tbody>
</table>

 

For additional information, see [Customizing CPSUI-Supported Window Controls](https://docs.microsoft.com/windows-hardware/drivers/print/customizing-cpsui-supported-window-controls).

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
<td>Compstui.h (include Compstui.h)</td>
</tr>
</tbody>
</table>

 

 




