---
title: NVIDIA GPU Sürücü Uzantısı - Azure Linux VMs
description: Linux çalıştıran N-serisi bilgisayar vm'lerine NVIDIA GPU Sürücüleri yüklemek için Microsoft Azure Uzantısı.
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
ms.openlocfilehash: 6ea61acfc2db3c8f1f5c9c0ac8da8f19897d441e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250575"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Linux için NVIDIA GPU Sürücü Uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Linux N serisi VM'lere NVIDIA GPU sürücülerini yükler. VM ailesine bağlı olarak, uzantı CUDA veya GRID sürücüleri yükler. Bu uzantıyı kullanarak NVIDIA sürücülerini yüklediğinizde, [NVIDIA Son Kullanıcı Lisans Sözleşmesi'nin](https://go.microsoft.com/fwlink/?linkid=874330)koşullarını kabul ediyor ve kabul etmektesiniz. Yükleme işlemi sırasında, VM sürücü kurulumunu tamamlamak için yeniden başlatılabilir.

Sürücülerin manuel kurulum talimatları ve geçerli desteklenen sürümleri [burada](
https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup)mevcuttur.
[Windows N serisi VM'lere](hpccompute-gpu-windows.md)NVIDIA GPU sürücüleri yüklemek için bir uzantı da mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı, belirli işletim sistemi sürümü için sürücü desteğine bağlı olarak aşağıdaki işletim sistemi dağıtımlarını destekler.

| Dağıtım | Sürüm |
|---|---|
| Linux: Ubuntu | 16.04 LTS, 18.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4, 7.5, 7.6 |
| Linux: CentOS | 7.3, 7.4, 7.5, 7.6 |

### <a name="internet-connectivity"></a>İnternet bağlantısı

NVIDIA GPU Sürücüleri için Microsoft Azure Uzantısı, hedef VM'nin internete bağlı olmasını ve erişime sahip olmasını gerektirir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON uzantısı için şema gösterir.

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
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Özellikler

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| yayımcı | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.2 | int |

### <a name="settings"></a>Ayarlar

Tüm ayarlar isteğe bağlıdır. Varsayılan davranış, sürücü yüklemesi için gerekli değilse çekirdeği güncelleştirmemek, desteklenen en son sürücüyü ve CUDA araç kitini (geçerli olduğu şekilde) yüklemektir.

| Adı | Açıklama | Varsayılan Değer | Geçerli Değerler | Veri Türü |
| ---- | ---- | ---- | ---- | ---- |
| updateOS | Sürücü yüklemesi için gerekli olmasa bile çekirdeği güncelleştirin | yanlış | doğru, yanlış | boole |
| driverVersion | NV: GRID sürücü sürümü<br> NC/ND: CUDA araç seti versiyonu. Seçilen CUDA için en son sürücüler otomatik olarak yüklenir. | en son | IZGARA: "430,30", "418,70", "410,92", "410,71", "390,75", "390,57", "390,42"<br> CUDA: "10.0.130", "9.2.88", "9.1.85" | string |
| installCUDA | CUDA araç kitini yükleyin. Yalnızca NC/ND serisi VM'ler için geçerlidir. | true | doğru, yanlış | boole |


## <a name="deployment"></a>Dağıtım


### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu 

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Şablonlar, dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıtılırken idealdir.

Sanal makine uzantısı için JSON yapılandırması sanal makine kaynağının içine yerleştirilebilir veya Kaynak Yöneticisi JSON şablonunun köküne veya üst seviyesine yerleştirilebilir. JSON yapılandırmasının yerleşimi kaynak adı ve türünün değerini etkiler. Daha fazla bilgi için [bkz.](../../azure-resource-manager/resource-manager-template-child-resource.md) 

Aşağıdaki örnek, uzantın sanal makine kaynağının içine girdiğini varsayar. Uzantı kaynağını iç içe alırken, JSON `"resources": []` sanal makinenin nesnesine yerleştirilir.

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
    "typeHandlerVersion": "1.2",
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
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnek, yukarıdaki Azure Kaynak Yöneticisi ve PowerShell örneklerini yansıtacak ve varsayılan olmayan sürücü yüklemesi için örnek olarak özel ayarlar ekler. Özellikle, işletim sistemi çekirdeğini güncelleştirir ve belirli bir CUDA araç seti sürüm sürücüsü yükler.

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
    "updateOS": true, `
    "driverVersion": "9.1.85", `
  }'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantılı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure PowerShell ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Çıkış kodları

| Çıkış Kodu | Anlamı | Olası Eylem |
| :---: | --- | --- |
| 0 | İşlem başarılı |
| 1 | Uzantının yanlış kullanımı | Yürütme çıktı günlüğünü denetleme |
| 10 | Hyper-V ve Azure için Linux Entegrasyon Hizmetleri kullanılamıyor veya yüklü değil | lspci çıktısını kontrol edin |
| 11 | NVIDIA GPU bu VM boyutunda bulunamadı | Desteklenen [VM boyutu ve işletim sistemi](../linux/n-series-driver-setup.md) kullanma |
| 12 | Görüntü teklifi desteklenmiyor |
| 13 | VM boyutu desteklenmiyor | Dağıtmak için N serisi VM kullanma |
| 14 | İşlem başarısız oldu | Yürütme çıktı günlüğünü denetleme |


### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi [için, Linux için Sanal makine uzantıları ve özellikleri](features-linux.md)ne bakın.

N serisi SANAL Makineler hakkında daha fazla bilgi için [GPU optimize edilmiş sanal makine boyutlarına](../linux/sizes-gpu.md)bakın.
