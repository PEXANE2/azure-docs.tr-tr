---
title: NVIDIA GPU Sürücü Uzantısı - Azure Windows VMs
description: NVIDIA GPU Sürücüleri'ni Windows çalıştıran N-serisi bilgisayar vm'lerine yüklemek için Microsoft Azure uzantısı.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: akjosh
ms.openlocfilehash: c388f433327b5328483f10fbef637a6fdfd08832
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250549"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>Windows için NVIDIA GPU Sürücü Uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Windows N serisi VM'lere NVIDIA GPU sürücülerini yükler. VM ailesine bağlı olarak, uzantı CUDA veya GRID sürücüleri yükler. Bu uzantıyı kullanarak NVIDIA sürücülerini yüklediğinizde, [NVIDIA Son Kullanıcı Lisans Sözleşmesi'nin](https://go.microsoft.com/fwlink/?linkid=874330)koşullarını kabul ediyor ve kabul etmektesiniz. Yükleme işlemi sırasında, VM sürücü kurulumunu tamamlamak için yeniden başlatılabilir.

Sürücülerin manuel kurulum talimatları ve geçerli desteklenen sürümleri [burada](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup)mevcuttur.
[Linux N serisi VM'lere](hpccompute-gpu-linux.md)NVIDIA GPU sürücüleri yüklemek için bir uzantı da mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı aşağıdaki OS'leri destekler:

| Dağıtım | Sürüm |
|---|---|
| Windows 10 | Çekirdek |
| Windows Server 2016 | Çekirdek |
| Windows Server 2012 R2 | Çekirdek |

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
    "type": "NvidiaGpuDriverWindows",
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
| type | NvidiaGpuDriverWindows | string |
| typeHandlerVersion | 1.2 | int |


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
    "type": "NvidiaGpuDriverWindows",
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
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
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

Uzantı yürütme çıktısı aşağıdaki dizine kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverMicrosoft\
```

### <a name="error-codes"></a>Hata kodları

| Hata Kodu | Anlamı | Olası Eylem |
| :---: | --- | --- |
| 0 | İşlem başarılı |
| 1 | Operasyon başarılı. Yeniden başlatma gerekli. |
| 100 | İşlem desteklenemedi veya tamamlanamadı. | Olası nedenler: PowerShell sürümü desteklenmez, VM boyutu N-serisi VM, Failure indirme verileri değildir. Hata nedenini belirlemek için günlük dosyalarını denetleyin. |
| 240, 840 | Operasyon zaman anına. | Yeniden deneyin işlemi. |
| -1 | Özel durum oluştu. | Özel durum nedenini belirlemek için günlük dosyalarını denetleyin. |
| -5x | Bekleyen yeniden başlatma nedeniyle işlem kesildi. | VM'yi yeniden başlatın. Yükleme yeniden başlatıldıktan sonra devam edecektir. Kaldır el ile çağrılmalıdır. |


### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi için [Windows için Sanal makine uzantıları ve özellikleri](features-windows.md)ne bakın.

N serisi SANAL Makineler hakkında daha fazla bilgi için [GPU optimize edilmiş sanal makine boyutlarına](../windows/sizes-gpu.md)bakın.
