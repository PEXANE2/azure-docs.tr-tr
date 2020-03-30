---
title: Azure VM ölçeği kümesi VM'ler için örnek i'ler anlama
description: Azure VM ölçeği için örnek lidleri anlayın sanal makineleri ve bunların yüzeye çıkma çeşitli yollarını ayarlar.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: manayar
ms.openlocfilehash: 99ad4249a4134bcc1b1cf5aba92b8a95a034db33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534431"
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Azure VM ölçeği kümesi VM'ler için örnek i'ler anlama
Bu makalede, ölçek kümeleri ve yüzey çeşitli şekillerde örnek adlar açıklanır.

## <a name="scale-set-instance-ids"></a>Ölçek kümesi örnek adlar

Bir ölçek kümesindeki her VM, onu benzersiz olarak tanımlayan bir örnek kimliği alır. Bu örnek id, ölçek kümesinde belirli bir VM'de işlem yapmak için ÖLÇEK Kümesi API'lerinde kullanılır. Örneğin, reimage API kullanırken yeniden görüntülemek için belirli bir örnek kimliği belirtebilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (daha fazla bilgi [için, REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage)bakın)

Powershell: `Set-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (daha fazla bilgi için [Powershell belgelerine](https://docs.microsoft.com/powershell/module/az.compute/set-azvmssvm)bakın)

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (daha fazla bilgi için [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)bakın).

Örnek iLiklerin listesini, bir ölçek kümesindeki tüm örnekleri listeleyerek alabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (daha fazla bilgi [için, REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list)bakın)

Powershell: `Get-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (daha fazla bilgi için [Powershell belgelerine](https://docs.microsoft.com/powershell/module/az.compute/get-azvmssvm)bakın)

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (daha fazla bilgi için [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)bakın).

[Resources.azure.com](https://resources.azure.com) veya [Azure SDK'larını,](https://azure.microsoft.com/downloads/) VM'leri bir ölçek kümesinde listelemek için de kullanabilirsiniz.

Çıktının tam sunumu komuta sağladığınız seçeneklere bağlıdır, ancak burada CLI'den bazı örnek çıktı verilmiştir:

```azurecli
az vmss show -g {resourceGroupName} -n {vmScaleSetName}
```

```output
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

Gördüğünüz gibi, "instanceId" özelliği sadece bir ondalık sayıdır. Eski örnekler silindikten sonra örnek adlar yeni örnekler için yeniden kullanılabilir.

>[!NOTE]
> Örnek iD'lerin ölçek kümesindeki VM'lere atanması konusunda **hiçbir garanti yoktur.** Onlar sırayla zaman artan görünebilir, ama bu her zaman böyle değildir. Örneğin IY'lerin VM'lere atandığı belirli bir şekilde bağımlılık yapmayın.

## <a name="scale-set-vm-names"></a>Ölçek kümesi VM adları

Yukarıdaki örnek çıktıda VM için bir "ad" da vardır. Bu ad "{scale-set-name}_{instance-id}" formunu alır. Bu ad, bir ölçek kümesindeki örnekleri listelediğinizde Azure portalında gördüğünüz addır:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

Adın {instance-id} bölümü, daha önce tartışılan "instanceId" özelliğiyle aynı ondalık sayıdır.

## <a name="instance-metadata-vm-name"></a>Örnek Metadata VM adı

Örnek meta [verilerini](../virtual-machines/windows/instance-metadata-service.md) vm ölçeği kümesi nden sorgularsanız, çıktıda bir "ad" görürsünüz:

```output
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

Bir ölçek kümesindeki her VM,ona atanmış bir bilgisayar adı da alır. Bu bilgisayar adı, sanal ağ içinde [Azure tarafından sağlanan DNS ad çözümünde](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)VM'nin ana bilgisayarıdır. Bu bilgisayar adı "{computer-name-prefix}{base-36-instance-id}" biçimindedir.

{base-36-instance-id} taban [36'dadır](https://en.wikipedia.org/wiki/Base36) ve her zaman altı basamaklı uzunluktadır. Sayının temel 36 gösterimi altı basamaktan az alıyorsa, {base-36-instance-id} sıfırlarla eklenmiştir ve altı basamaklı uzunlukta olur. Örneğin, {computer-name-prefix} "nsgvmss" ve instance ID 85 olan bir örnekte "nsgvmss00002D" bilgisayar adı olacaktır.

>[!NOTE]
> Bilgisayar adı öneki, ayarlayabildiğiniz ölçek kümesi modelinin bir özelliğidir, bu nedenle ölçek kümesi adının kendisinden farklı olabilir.
