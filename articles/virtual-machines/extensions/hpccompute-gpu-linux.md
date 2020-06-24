---
title: NVıDıA GPU sürücü uzantısı-Azure Linux VM 'Leri
description: Linux çalıştıran N serisi işlem VM 'lerine NVıDıA GPU sürücülerini yüklemek için Microsoft Azure uzantısı.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/11/2019
ms.author: akjosh
ms.openlocfilehash: 68dddde965900b966efa96fbd7da7141f1ed8a94
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84753553"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Linux için NVıDıA GPU sürücü uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Linux N serisi VM 'Lere NVıDıA GPU sürücülerini yüklüyor. VM ailesine bağlı olarak, uzantı CUDA veya KıLAVUZ sürücülerini de yüklüyor. Bu uzantıyı kullanarak NVıDıA sürücülerini yüklediğinizde, [NVIDIA Son Kullanıcı Lisans sözleşmesinin](https://go.microsoft.com/fwlink/?linkid=874330)şartlarını kabul etmiş ve kabul etmiş olursunuz. Yükleme işlemi sırasında, sanal makine, Sürücü kurulumunu tamamlayacak şekilde yeniden başlayabilir.

Sürücülerin ve desteklenen geçerli sürümlerin el ile yüklenmesiyle ilgili yönergeler [burada](
https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup)bulunabilir.
[Windows N serisi VM 'LERE](hpccompute-gpu-windows.md)NVIDIA GPU sürücülerini yüklemek için de bir uzantı kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı, belirli işletim sistemi sürümü için sürücü desteğine bağlı olarak aşağıdaki işletim sistemi destekleri 'nı destekler.

| Dağıtım | Sürüm |
|---|---|
| Linux: Ubuntu | 16,04 LTS, 18,04 LTS |
| Linux: Red Hat Enterprise Linux | 7,3, 7,4, 7,5, 7,6, 7,7 |
| Linux: CentOS | 7,3, 7,4, 7,5, 7,6, 7,7 |

### <a name="internet-connectivity"></a>İnternet bağlantısı

NVıDıA GPU sürücüleri için Microsoft Azure uzantısı, hedef sanal makinenin internet 'e bağlı ve erişime sahip olmasını gerektirir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON uzantı için şemayı gösterir.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.3",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Özellikler

| Name | Değer/örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| yayımcı | Microsoft. HpcCompute | string |
| tür | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.3 | int |

### <a name="settings"></a>Ayarlar

Tüm ayarlar isteğe bağlıdır. Varsayılan davranış, sürücü yüklemesi için gerekli değilse çekirdeği güncelleştirmemelidir, desteklenen en son sürücüyü ve CUDA araç setini (varsa) yükler.

| Name | Description | Varsayılan değer | Geçerli Değerler | Veri Türü |
| ---- | ---- | ---- | ---- | ---- |
| updateOS | Sürücü yüklemesi için gerekli olmasa bile çekirdeği Güncelleştir | yanlış | doğru, yanlış | boole |
| driverVersion | NV: GRID sürücü sürümü<br> NC/ND: CUDA araç seti sürümü. Seçilen CUDA için en son sürücüler otomatik olarak yüklenir. | en son | KıLAVUZ: "430,30", "418,70", "410,92", "410,71", "390,75", "390,57", "390,42"<br> CUDA: "10.0.130", "9.2.88", "9.1.85" | string |
| ınstallcuda | CUDA araç setini yükler. Yalnızca NC/ND serisi VM 'Ler için geçerlidir. | true | doğru, yanlış | boole |


## <a name="deployment"></a>Dağıtım


### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu 

Azure VM uzantıları, Azure Resource Manager şablonlarıyla dağıtılabilir. Dağıtım sonrası yapılandırması gerektiren bir veya daha fazla sanal makine dağıtıldığında şablonlar idealdir.

Bir sanal makine uzantısının JSON yapılandırması, sanal makine kaynağının içinde iç içe veya Kaynak Yöneticisi JSON şablonunun kök veya üst düzeyine yerleştirilmiş olabilir. JSON yapılandırmasının yerleştirilmesi, kaynak adının ve türün değerini etkiler. Daha fazla bilgi için bkz. [alt kaynaklar için ad ve tür ayarlama](../../azure-resource-manager/resource-manager-template-child-resource.md). 

Aşağıdaki örnek, uzantının sanal makine kaynağının içinde iç içe olduğunu varsayar. Uzantı kaynağını yuvalama sırasında JSON, `"resources": []` sanal makinenin nesnesine yerleştirilir.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.3",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.3 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnek, yukarıdaki Azure Resource Manager ve PowerShell örneklerini yansıtır.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name NvidiaGpuDriverLinux \
  --publisher Microsoft.HpcCompute \
  --version 1.3 
```

Aşağıdaki örnek, varsayılan olmayan sürücü yüklemesi için örnek olarak iki isteğe bağlı özel ayar de ekler. Özellikle, işletim sistemi çekirdeğini en son sürümüne güncelleştirir ve belirli bir CUDA Araç Seti sürüm sürücüsünü kurar. '--Settings ', isteğe bağlı ve varsayılan ' i aklınızda bulunur. Çekirdeği güncelleştirmenin Uzantı yükleme sürelerini artırabileceğini unutmayın. Ayrıca, belirli (eski) CUDA tolkit sürümünü seçmek, her zaman daha yeni çekirdekler ile uyumlu olmayabilir.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name NvidiaGpuDriverLinux \
  --publisher Microsoft.HpcCompute \
  --version 1.3 \
  --settings '{ \
    "updateOS": true, \
    "driverVersion": "10.0.130" \
  }'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtımlarının durumu hakkındaki veriler Azure portal ve Azure PowerShell ve Azure CLı kullanılarak alınabilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkışı aşağıdaki dosyaya kaydedilir. Durumu izlemek için bu dosyaya başvurun (herhangi bir uzun süre çalışan) yükleme ve tüm hata sorunlarını giderme.

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Çıkış kodları

| Çıkış Kodu | Anlamı | Olası eylem |
| :---: | --- | --- |
| 0 | İşlem başarılı |
| 1 | Uzantının yanlış kullanımı | Yürütme çıkış günlüğünü denetle |
| 10 | Hyper-V ve Azure için Linux Tümleştirme Hizmetleri kullanılabilir değil veya yüklü değil | Lspcı çıkışını denetle |
| 11 | NVıDıA GPU bu VM boyutunda bulunamadı | Desteklenen bir [VM boyutu ve işletim sistemi](../linux/n-series-driver-setup.md) kullan |
| 12 | Görüntü teklifi desteklenmiyor |
| 13 | VM boyutu desteklenmiyor | Dağıtmak için N serisi VM kullanma |
| 14 | İşlem başarısız oldu | Yürütme çıkış günlüğünü denetle |


### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi için bkz. [Linux Için sanal makine uzantıları ve özellikleri](features-linux.md).

N serisi VM 'Ler hakkında daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../linux/sizes-gpu.md).
