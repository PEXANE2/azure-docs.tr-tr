---
title: Linux için sanal makine uzantısı Analytics oturum
description: Log Analytics aracısını sanal makine uzantısını kullanarak Linux sanal makinesine dağıtın.
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
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468788"
---
# <a name="log-analytics-virtual-machine-extension-for-linux"></a>Linux için sanal makine uzantısı Analytics oturum

## <a name="overview"></a>Genel Bakış

Azure Izleyici günlükleri, bulut ve şirket içi varlıklar arasında izleme, uyarı ve uyarı düzeltme özellikleri sağlar. Linux için Log Analytics sanal makine uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve sanal makinelerin mevcut bir Log Analytics çalışma alanına kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Linux için Log Analytics VM uzantısı için dağıtım seçenekleri açıklanmaktadır.

>[!NOTE]
>Azure İzleyici devam eden Microsoft Operations Management Suite (OMS) gelen geçiş işleminin bir parçası olarak için OMS aracısını Windows veya Linux için Windows ve Log Analytics aracısını Linux için Log Analytics aracısını olarak adlandırılır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Linux dağıtımları hakkında daha fazla bilgi için [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) makalesine bakın.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM uzantısı sürümü
Aşağıdaki tabloda, her sürüm için Log Analytics aracısını paket ve Log Analytics VM uzantısı sürümünü bir eşleme sağlar. Log Analytics aracı Paket sürümü için sürüm notları için bir bağlantı bulunur. Sürüm Notları, hata düzeltmeleri ve belirli bir aracı sürüm için yeni özellikler hakkında ayrıntılı bilgi içerir.  

| Log Analytics Linux VM uzantı sürümü | Log Analytics aracısını Paket sürümü | 
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

Azure Güvenlik Merkezi, otomatik olarak Log Analytics aracısını sağlar ve Azure aboneliğinizde ASC tarafından oluşturulan varsayılan Log Analytics çalışma alanına bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması, yapılandırılmış çalışma alanı üzerine yazar ve Azure Güvenlik Merkezi ile bağlantısını keser.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Log Analytics aracısını uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Log Analytics aracısını Uzantı Şeması aşağıdaki JSON'u göstermektedir. Uzantı, hedef Log Analytics çalışma alanından çalışma alanı KIMLIĞI ve çalışma alanı anahtarı gerektirir; Bu değerler, Azure portal [Log Analytics çalışma alanınızda bulunabilir](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) . Çalışma alanı anahtarı hassas verileri olarak değerlendirilip olduğundan, bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi. Çalışma alanı **kimliği** ve **workspaceKey** büyük/küçük harfe duyarlı olduğunu unutmayın.

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
>Yukarıdaki şemayı şablonunun kök düzeyinde yerleştirilir varsayar. Şablonu şablondaki sanal makine kaynağının içine yerleştirirseniz, [daha](#template-deployment)sonra açıklandığı gibi `type` ve `name` özellikleri değiştirilmelidir.

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / örnek |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| workspaceId (örn.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örn.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Azure Izleyici günlüklerine ekleme gibi dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıtımında şablonlar idealdir. Log Analytics Aracısı VM uzantısını içeren örnek bir Kaynak Yöneticisi şablonu [Azure hızlı başlangıç galerisinde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm)bulunabilir. 

Sanal makine uzantısı için JSON yapılandırma içinde sanal makine kaynağı iç içe geçmiş veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yapılandırma yerleşimini etkiler. Daha fazla bilgi için bkz. [alt kaynaklar için ad ve tür ayarlama](../../azure-resource-manager/templates/child-resource-name-type.md). 

Aşağıdaki örnekte, VM uzantısını sanal makine kaynağı içinde iç içe varsayılır. Uzantı kaynağını yuvalama sırasında JSON, sanal makinenin `"resources": []` nesnesine yerleştirilir.

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

Uzantı JSON şablonu kökünde yerleştirilirken, kaynak adı üst sanal makineye bir başvuru içerir ve iç içe geçmiş yapılandırma türü yansıtır.  

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

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, mevcut bir sanal makine için Log Analytics Aracısı VM uzantısını dağıtmak için kullanılabilir. Aşağıdaki *myworkspacekey anahtarınızla* değerini çalışma alanı anahtarınızla ve *MyWorkspace ID* değerini çalışma alanı Kimliğinizle değiştirin. Bu değerler, *Gelişmiş ayarlar*altında Azure Portal Log Analytics çalışma alanınızda bulunabilir. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.10.1 --protected-settings '{"workspaceKey":"myWorkspaceKey"}' \
  --settings '{"workspaceId":"myWorkspaceId"}'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 9 | Etkinleştirme beklenenden önce çağırılır | [Azure Linux aracısını](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) kullanılabilir en son sürüme güncelleştirin. |
| 10 | VM, Log Analytics çalışma alanınıza zaten bağlı | VM uzantısı şemasında belirtilen çalışma alanına bağlanmak için stopOnMultipleConnections genel ayarları false olarak ayarlayın veya bu özelliği kaldırın. Bu VM için bağlı her bir çalışma alanı için bir kez faturalandırılır. |
| 11 | Uzantı için sağlanan geçersiz yapılandırma | Dağıtım için gerekli tüm özellik değerlerini ayarlamak için Yukarıdaki örneklerde izleyin. |
| 17 | Günlük analizi paket yükleme hatası | 
| 19 | OMI paket yükleme hatası | 
| 20 | SCX paket yükleme hatası |
| 51 | Bu uzantı sanal makinenin işletim sistemi üzerinde desteklenmiyor | |
| 55 | Azure Izleyici hizmetine bağlanılamıyor veya gerekli paketler eksik veya dpkg Paket Yöneticisi kilitli| Sistem ya da Internet erişimi veya geçerli bir HTTP proxy'sinin sağlanan sahip olmadığını denetleyin. Ayrıca, çalışma alanı kimliği doğruluğunu denetleyin ve curl ve tar yardımcı programları yüklü olmadığını doğrulayın. |

Ek sorun giderme bilgileri [için Log Analytics-Agent-for-Linux sorun giderme kılavuzunda](../../azure-monitor/platform/vmext-troubleshoot.md)bulabilirsiniz.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayına dosya. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
