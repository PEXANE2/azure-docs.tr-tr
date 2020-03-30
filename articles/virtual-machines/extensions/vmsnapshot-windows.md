---
title: Azure Yedekleme için VM Snapshot Windows uzantısı
description: VM anlık görüntü uzantısını kullanarak Azure Yedekleme'den sanal makinenin uygulama tutarlı yedeklemesini alın
services: backup, virtual-machines-windows
documentationcenter: ''
author: trinadhkotturu
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 12/17/2018
ms.author: trinadhk
ms.openlocfilehash: 5ee3b97eb835322a0ad6c8a69c20776a243713fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415130"
---
# <a name="vm-snapshot-windows-extension-for-azure-backup"></a>Azure Yedekleme için VM Snapshot Windows uzantısı

Azure Yedekleme, şirket içi iş yüklerini şirket içi buluta yedeklemek ve bulut kaynaklarını Kurtarma Hizmetleri kasasına yedeklemek için destek sağlar. Azure Yedekleme, VM'yi kapatmaya gerek kalmadan Azure sanal makinesinin uygulama tutarlı yedeklemesini almak için VM anlık görüntü uzantısını kullanır. VM Anlık Görüntü uzantısı, Azure Yedekleme hizmetinin bir parçası olarak Microsoft tarafından yayımlanır ve desteklenir. Azure Yedekleme, yedeklemeyi etkinleştiren ilk zamanlanmış yedekleme tetikleyici gönderinin bir parçası olarak uzantıyı yükler. Bu belge, VM Anlık Görüntü uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

VMSnapshot uzantısı, yalnızca yönetilmeyen VM'ler için Azure portalında görünür.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi
Desteklenen işletim sistemlerinin listesi için [Azure Yedekleme tarafından desteklenen İşletim Sistemleri](../../backup/backup-azure-arm-vms-prepare.md#before-you-start) bölümüne bakın

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON VM anlık uzantısı için şema gösterir. Uzantı görev kimliği gerektirir - bu VM, durum blob uri anlık görüntü tetikleyen yedekleme işi tanımlar - anlık görüntü işleminin durumu yazılır, anlık görüntü nün zamanlanan başlangıç saati, günlükleri blob uri - nerede günlükleri anlık göreve karşılık gelen VM disklerinin ve meta verilerinin objstr- gösterimi yazılır.  Bu ayarlar hassas veri olarak ele alınması gerektiğinden, korumalı bir ayar yapılandırmasında depolanmalıdır. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. Bu ayarların Yalnızca Yedekleme işinin bir parçası olarak Azure Yedekleme hizmetinden geçirilmesi önerilir.

```json
{
  "type": "extensions",
  "name": "VMSnapshot",
  "location":"<myLocation>",
  "properties": {
    "publisher": "Microsoft.RecoveryServices",
    "type": "VMSnapshot",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "locale":"<location>",
      "taskId":"<taskId used by Azure Backup service to communicate with extension>",
      "commandToExecute": "snapshot",
      "commandStartTimeUTCTicks": "<scheduled start time of the snapshot task>",
      "vmType": "microsoft.compute/virtualmachines"
    },
    "protectedSettings": {
      "objectStr": "<blob SAS uri representation of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| taskId | e07354cf-041e-4370-929f-25a319ce8933_1 | string |
| komutStartTimeUTCTicks | 6.36458E+17 | string |
| yerel ayar | tr-tr | string |
| nesneStr | Sas uri dizi kodlama- "blobSASUri":\/\/["https: sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc201652903941.vhd?sv=2014-02-14&sr=b&sig=TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", "https:\/\/sopattna8461.blob.core.windows.net\/vhds\/vmwin1404ltsc-20160629-122418.vhd?sv=2014-02-14&sr=b&sig= 5S0A6YDWvVwqPAkzWXVy%2BS%2FqMwzFMbamT5upwx05v8Q%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z sw=,&\/\/\/"https: sopattna8461.blob.core.windows.net bootdiagnostics-vmwintu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/v mubuntu1404ltsc-20160629-122541.vhd?sv=2014-02-14&sr=b&sig=X0Me2djByksBBMVXMGIUrcycvhQSfjYv qKLeRA7nBD4%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw", " https:\/\/\/\/sopattna5365.blob.core.windows.net vhds vmwin1404ltsc-20160701-163922.vhd?sv=2014-02-14&sr=b&sig=oXvtK2IXCNqWv7fpjc7TAzFDpc1GoX tT7r%2BC%2BNIAork%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z\/&\/sp=rw", "https:\/\/sopattna5365.blob.core.windows.net vhds vmwin1404ltsc-20170705-124311.vhd?sv=2014-02-14&sr=b&sig=ZUM9d28Mvvm% 2FfrhJ71TFZh0Ni90m38bBs3zMl%2FQ9rs0%3D&st=2017-11-09T14%3A23%3A28Z&se=2017-11-09T17%3A38%3A28Z&sp=rw"] | string |
| kütüklerBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |
| durumBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |



## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Ancak, sanal makineye VM anlık görüntüsü uzantısı eklemenin önerilen yolu, sanal makinede yedeklemeyi etkinleştirmektir. Bu, Kaynak Yöneticisi şablonu aracılığıyla elde edilebilir.  Sanal bir makinede yedekleme sağlayan örnek bir Kaynak Yöneticisi şablonu [Azure Hızlı Başlangıç Galerisi'nde](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/)bulunabilir.


## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, sanal bir makinede yedeklemeyi etkinleştirmek için kullanılabilir. Sonrası etkinleştirme yedekleme, ilk zamanlanmış yedekleme işi VM Vm anlık uzantısı yükler.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

```
C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

Sorun giderme bilgileri [Azure VM yedekleme sorun giderme kılavuzunda](../../backup/backup-azure-vms-troubleshoot.md)bulunabilir.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
