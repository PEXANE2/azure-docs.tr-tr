---
title: Linux için Log Analytics sanal makine uzantısı
description: Sanal makine uzantısını kullanarak Log Analytics aracısını Linux sanal makinesine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: akjosh
ms.openlocfilehash: 9ddac229fc38a91a8b97b24dc2807080b2295758
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250562"
---
# <a name="log-analytics-virtual-machine-extension-for-linux"></a>Linux için Log Analytics sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

Azure Monitor Günlükleri, bulut ve şirket içi varlıklar arasında izleme, uyarı ve uyarı düzeltme özellikleri sağlar. Linux için Log Analytics sanal makine uzantısı Microsoft tarafından yayınlanır ve desteklenir. Uzantı, Log Analytics aracısını Azure sanal makinelerine yükler ve sanal makineleri varolan bir Log Analytics çalışma alanına kaydeder. Bu belge, Linux için Log Analytics sanal makine uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

>[!NOTE]
>Microsoft Operations Management Suite'ten (OMS) Azure Monitor'a devam eden geçişin bir parçası olarak, Windows veya Linux için OMS Aracısı, Windows için Log Analytics aracısı ve Linux için Log Analytics aracısı olarak anılacaktır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Linux dağıtımları hakkında daha fazla bilgi için [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) makalesine bakın.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM Uzantı sürümü
Aşağıdaki tablo, her sürüm için Log Analytics VM uzantısı ve Log Analytics aracı paketinin sürümünün eşlemesini sağlar. Log Analytics aracısı paketi sürümü için sürüm notlarına bir bağlantı dahildir. Sürüm notları, hata düzeltmeleri ve belirli bir aracı sürümü için kullanılabilen yeni özellikler le ilgili ayrıntıları içerir.  

| Log Analytics Linux VM uzantılı versiyonu | Log Analytics Agent paket sürümü | 
|--------------------------------|--------------------------|
| 1.12.25 | [1.12.15-0](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.12.15-0) |
| 1.11.15 | [1.11.0-9](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.11.0-9) |
| 1.10.0 | [1.10.0-1](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.10.0-1) |
| 1.9.1 | [1.9.0-0](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.9.0-0) |
| 1.8.11 | [1.8.1-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.8.1.256)| 
| 1.8.0 | [1.8.0-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/1.8.0-256)| 
| 1.7.9 | [1.6.1-3](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.1.3)| 
| 1.6.42.0 | [1.6.0-42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2-125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Log Analytics aracısını otomatik olarak sağlar ve Azure aboneliğinizde ASC tarafından oluşturulan varsayılan Log Analytics çalışma alanına bağlar. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunu yapmak, yapılandırılan çalışma alanının üzerine yazar ve Azure Güvenlik Merkezi ile bağlantıyı koparır.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Log Analytics Agent uzantısı hedef sanal makine internete bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Log Analytics Agent uzantısı için şema gösterir. Uzantı, hedef Log Analytics çalışma alanından çalışma alanı kimliği ve çalışma alanı anahtarı gerektirir; bu değerler Azure [portalındaki Log Analytics çalışma alanınızda bulunabilir.](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) Çalışma alanı anahtarı hassas veri olarak ele alınması gerektiğinden, korumalı bir ayar yapılandırmasında depolanmalıdır. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. **WorkspaceId** ve **workspaceKey'in** büyük/küçük harf duyarlı olduğunu unutmayın.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

>[!NOTE]
>Yukarıdaki şema, şablonun kök düzeyine yerleştirileceğini varsayar. Şablondaki sanal makine kaynağının içine koyarsanız, `name` daha [aşağıda](#template-deployment)açıklandığı gibi, `type` ve özellikleri değiştirilmelidir.

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| yayımcı | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| çalışma alanıId (örn. | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| çalışma alanıAnahtar (örn. ) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwvv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Şablonlar, Azure Monitör Günlükleri'ne binme gibi dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıtılırken idealdir. Log Analytics Agent VM uzantısını içeren örnek bir Kaynak Yöneticisi şablonu [Azure Quickstart Galerisi'nde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm)bulunabilir. 

Sanal makine uzantısı için JSON yapılandırması sanal makine kaynağının içine yerleştirilebilir veya Kaynak Yöneticisi JSON şablonunun köküne veya üst seviyesine yerleştirilebilir. JSON yapılandırmasının yerleşimi kaynak adı ve türünün değerini etkiler. Daha fazla bilgi için [bkz.](../../azure-resource-manager/templates/child-resource-name-type.md) 

Aşağıdaki örnek, VM uzantısısanal makine kaynağının içinde iç içe olduğunu varsayar. Uzantı kaynağını iç içe alırken, JSON `"resources": []` sanal makinenin nesnesine yerleştirilir.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

UzantıJSON'u şablonun köküne yerleştirirken, kaynak adı üst sanal makineye bir başvuru içerir ve tür iç içe yapılan yapılandırmayı yansıtır.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, Log Analytics Agent VM uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. Aşağıdaki *myWorkspaceKey* değerini çalışma alanı anahtarınızla ve *myWorkspaceId* değerini çalışma alanı kimliğinizle değiştirin. Bu değerler, *Gelişmiş Ayarlar*altında Azure portalındaki Log Analytics çalışma alanınızda bulunabilir. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.10.1 --protected-settings '{"workspaceKey":"myWorkspaceKey"}' \
  --settings '{"workspaceId":"myWorkspaceId"}'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası Eylem |
| :---: | --- | --- |
| 9 | Erken çağrıyı etkinleştirme | [Azure Linux Aracısını](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) kullanılabilir en son sürüme güncelleştirin. |
| 10 | VM zaten bir Log Analytics çalışma alanına bağlı | VM'yi uzantı şemasında belirtilen çalışma alanına bağlamak için stopOnMultipleConnections'ı genel ayarlarda false olarak ayarlayın veya bu özelliği kaldırın. Bu VM, bağlı olduğu her çalışma alanı için bir kez faturalandırılır. |
| 11 | Uzantıya sağlanan geçersiz konfig | Dağıtım için gerekli tüm özellik değerlerini ayarlamak için önceki örnekleri izleyin. |
| 17 | Log Analytics paket yükleme hatası | 
| 19 | OMI paket yükleme hatası | 
| 20 | SCX paket yükleme hatası |
| 51 | Bu uzantı VM'nin işletim sisteminde desteklenmez | |
| 55 | Azure Monitor hizmetine bağlanılamıyor veya gerekli paketler eksik veya dpkg paket yöneticisi kilitli| Sistemin Internet erişimi ne olduğundan veya geçerli bir HTTP proxy'sinin sağlandığını denetleyin. Ayrıca, çalışma alanı kimliğinin doğruluğunu denetleyin ve kıvırma ve katran yardımcı programlarının yüklü olduğunu doğrulayın. |

Linux için [Sorun Giderme Kılavuzu'nda log analytics-Agent-for-Linux Sorun Giderme Kılavuzu'nda](../../azure-monitor/platform/vmext-troubleshoot.md)ek sorun giderme bilgileri bulunabilir.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
