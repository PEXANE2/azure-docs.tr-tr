---
title: Azure VM Ölçek kümesi VM 'lerinin örnek kimliklerini anlayın
description: Azure VM Ölçek Kümeleri sanal makinelerinin örnek kimliklerini ve bunların yüzeysel çeşitli yollarını anlayın.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: manayar
ms.openlocfilehash: aa2b0013818f897f01945d394266a57016ecb0bb
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79250770"
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Azure VM Ölçek kümesi VM 'lerinin örnek kimliklerini anlayın
Bu makalede ölçek kümeleri için örnek kimlikleri ve bunların yüzeylerinde yer alan çeşitli yollar açıklanmaktadır.

## <a name="scale-set-instance-ids"></a>Ölçek kümesi örnek kimlikleri

Bir ölçek kümesindeki her VM, kendisini benzersiz bir şekilde tanımlayan bir örnek KIMLIĞI alır. Bu örnek KIMLIĞI ölçek kümesindeki belirli bir VM üzerinde işlemler yapmak için ölçek kümesi API 'Lerinde kullanılır. Örneğin, ReImage API 'sini kullanırken yeniden görüntüye yönelik belirli bir örnek KIMLIĞI belirtebilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (daha fazla bilgi için [REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage)bakın)

PowerShell: `Set-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (daha fazla bilgi için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/module/az.compute/set-azvmssvm))

CLı: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (daha fazla bilgi için bkz. [CLI belgeleri](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)).

Bir ölçek kümesindeki tüm örnekleri listeleyerek örnek kimliklerinin listesini alabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (daha fazla bilgi için [REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list)bakın)

PowerShell: `Get-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (daha fazla bilgi için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/module/az.compute/get-azvmssvm))

CLı: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (daha fazla bilgi için bkz. [CLI belgeleri](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)).

Bir ölçek kümesindeki VM 'Leri listelemek için [Resources.Azure.com](https://resources.azure.com) veya [Azure SDK](https://azure.microsoft.com/downloads/) 'larını de kullanabilirsiniz.

Çıktının tam sunumu, komuta verdiğiniz seçeneklere bağlıdır, ancak burada CLı 'dan örnek çıktı verilmiştir:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
[
  {
    "instanceId": "85",
    "latestModelApplied": true,
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Gördüğünüz gibi, "InstanceId" özelliği yalnızca ondalık bir sayıdır. Eski örnekler silindikten sonra örnek kimlikleri yeni örnekler için yeniden kullanılabilir.

>[!NOTE]
> Ölçek kümesindeki VM 'lere örnek kimliklerinin atanması için **garanti yoktur** . Ardışık olarak sürekli artan görünebilir, ancak bu her zaman durum değildir. VM 'lere atanan örnek kimliklerinin belirli bir bağımlılığı yoktur.

## <a name="scale-set-vm-names"></a>Ölçek kümesi VM adları

Yukarıdaki örnek çıktıda, sanal makine için de bir "ad" vardır. Bu ad "{Scale-set-name} _ {Instance-id}" biçimini alır. Bu ad, bir ölçek kümesindeki örnekleri listelerseniz Azure portal gördüğünüz bir addır:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

Adın {instance-id} bölümü, daha önce ele alınan "InstanceId" özelliği ile aynı ondalık sayıdır.

## <a name="instance-metadata-vm-name"></a>Örnek meta veri VM adı

[Örnek meta verilerini](../virtual-machines/windows/instance-metadata-service.md) bir ölçek kümesi sanal makinesinin içinden sorgulerseniz çıktıda bir "ad" görürsünüz:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Bu ad, daha önce tartışılan adla aynıdır.

## <a name="scale-set-vm-computer-name"></a>Ölçek kümesi VM bilgisayar adı

Bir ölçek kümesindeki her VM kendisine atanan bir bilgisayar adı da alır. Bu bilgisayar adı, [sanal ağ Içindeki Azure tarafından BELIRTILEN DNS adı çözünürlüğünde](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)VM 'nin ana bilgisayar adıdır. Bu bilgisayar adı "{bilgisayar-adı-önek} {Base-36-Instance-id}" biçimindedir.

{Base-36-Instance-id}, [base 36](https://en.wikipedia.org/wiki/Base36) ' dir ve her zaman altı basamaktan oluşmalıdır. Sayının Base 36 temsili altıdan az basamak alırsa, {Base-36-Instance-id}, altı basamaklı bir değer olacak şekilde sıfırlar ile doldurulur. Örneğin, {bilgisayar-adı-prefix} "nsgvmss" ve örnek KIMLIĞI 85 olan bir örnek bilgisayar adı "nsgvmss00002D" olacaktır.

>[!NOTE]
> Bilgisayar adı ön eki, ayarlayabileceğiniz ölçek kümesi modelinin bir özelliğidir, bu sayede ölçek kümesi adından farklı olabilir.
