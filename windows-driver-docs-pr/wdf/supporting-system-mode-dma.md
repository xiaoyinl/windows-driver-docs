---
title: Supporting System-Mode DMA
description: Describes the code that a KMDF driver provides in its event callback functions to handle I/O requests for a system-mode DMA device.
ms.assetid: CCC77C15-69CA-44CB-8DEB-29F3EAEA44F6
ms.date: 04/20/2017
ms.localizationpriority: medium
---

# Supporting System-Mode DMA


\[Applies to KMDF only\]

System-mode DMA, in contrast to *bus-master* DMA, describes a configuration in which multiple devices share a single, typically multichannel DMA controller.

Starting in Kernel-Mode Driver Framework (KMDF) version 1.11, the framework supports system-mode DMA on System on a Chip (SoC)–based systems running on Windows 8 or later versions of the Windows operating system.

This topic describes the code that a KMDF driver must provide in its event callback functions, as well as optional event callback functions it can register, to handle I/O requests for a system-mode DMA device.

For information about KMDF and bus-master DMA, see [Handling I/O Requests in a KMDF Driver for a Bus-Master DMA Device](handling-i-o-requests-in-a-kmdf-driver-for-a-bus-master-dma-device.md).

The following figure shows the event callback functions that your driver uses to support system-mode DMA:

![system-mode dma implementation in kmdf drivers](images/sys-mode-dma-in-kmdf.png)

## Creating a System-Mode DMA Enabler


Creating a system-mode DMA profile is a two-step process. The following steps represent a typical scenario:

1.  Typically in its [*EvtDriverDeviceAdd*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdriver/nc-wdfdriver-evt_wdf_driver_device_add) callback function, the driver calls [**WDF\_DMA\_ENABLER\_CONFIG\_INIT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdf_dma_enabler_config_init), setting the **Profile** parameter to **SystemMode** or **SystemModeDuplex**. The driver then calls [**WdfDmaEnablerCreate**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdfdmaenablercreate), passing the [**WDF\_DMA\_ENABLER\_CONFIG**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/ns-wdfdmaenabler-_wdf_dma_enabler_config) structure that it just received.

    The driver might alternatively create the enabler during [*EvtDevicePrepareHardware*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware).

2.  Your driver's [*EvtDevicePrepareHardware*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware) callback function associates the DMA enabler with its DMA resources by calling the [**WdfDmaEnablerConfigureSystemProfile**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdfdmaenablerconfiguresystemprofile) method. For a duplex enabler, the driver calls [**WdfDmaEnablerConfigureSystemProfile**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdfdmaenablerconfiguresystemprofile) twice, once to configure each transfer direction.

    The driver can call [**WdfDmaEnablerConfigureSystemProfile**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdfdmaenablerconfiguresystemprofile) after [*EvtDevicePrepareHardware*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware) has completed, but the driver must call this method before it initializes DMA transactions.

## Providing Optional Callback Functions


### <a href="" id="configuring-a-system-mode-dma-enabler"></a>Configuring a DMA Channel

Typically, KMDF drivers do not configure DMA channels. However, in certain circumstances, drivers may need to perform channel-specific configuration. For example, a driver might call a custom function that is implemented by the DMA controller by using the following steps:

1.  In one of the driver's [request handlers](request-handlers.md), the driver calls [**WdfDmaTransactionSetChannelConfigurationCallback**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nf-wdfdmatransaction-wdfdmatransactionsetchannelconfigurationcallback) to register a [*EvtDmaTransactionConfigureDmaChannel*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nc-wdfdmatransaction-evt_wdf_dma_transaction_configure_dma_channel) callback function.
2.  Your driver's [*EvtDmaTransactionConfigureDmaChannel*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nc-wdfdmatransaction-evt_wdf_dma_transaction_configure_dma_channel) callback function calls [**WdfDmaEnablerWdmGetDmaAdapter**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmaenabler/nf-wdfdmaenabler-wdfdmaenablerwdmgetdmaadapter) to retrieve a pointer to the WDM [**DMA\_ADAPTER**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdm/ns-wdm-_dma_adapter). This structure is the adapter object that represents the driver's system-mode DMA channel.
3.  The driver can then call [**ConfigureAdapterChannel**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdm/nc-wdm-pconfigure_adapter_channel) to enable custom functions implemented by the DMA controller. This routine is callable only by pointer from the address returned in a [**DMA\_OPERATIONS**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdm/ns-wdm-_dma_operations) structure.
4.  Your driver's [*EvtDmaTransactionConfigureDmaChannel*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nc-wdfdmatransaction-evt_wdf_dma_transaction_configure_dma_channel) callback function returns TRUE if it successfully configures the DMA channel.
5.  The framework calls the driver's [*EvtProgramDma*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nc-wdfdmatransaction-evt_wdf_program_dma) callback function.

### Receiving Notification of Transfer Completion

Unlike devices that use bus-mastering controllers, the hardware for a system-mode DMA device might not signal DMA transfer completion by issuing an interrupt.

If your device does not raise an interrupt to signal DMA transfer completion, your driver can provide an [*EvtDmaTransactionDmaTransferComplete*](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nc-wdfdmatransaction-evt_wdf_dma_transaction_dma_transfer_complete) event callback function that the framework calls when a system-mode DMA transfer has completed.

To register this callback function, a driver calls [**WdfDmaTransactionSetTransferCompleteCallback**](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdmatransaction/nf-wdfdmatransaction-wdfdmatransactionsettransfercompletecallback) from one of its [request handlers](request-handlers.md).

 

 





