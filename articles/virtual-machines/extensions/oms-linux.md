---
title: Linux için Log Analytics sanal makine uzantısı
description: Log Analytics aracısını, sanal makine uzantısı kullanarak Linux sanal makinesine dağıtın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79250562"
---
# <a name="log-analytics-virtual-machine-extension-for-linux"></a>Linux için Log Analytics sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

Azure Izleyici günlükleri, bulut ve şirket içi varlıklar arasında izleme, uyarı ve uyarı düzeltme özellikleri sağlar. Linux için Log Analytics sanal makine uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı Log Analytics aracısını Azure sanal makinelerine yükleyip sanal makineleri mevcut bir Log Analytics çalışma alanına kaydeder. Bu belgede, Linux için Log Analytics sanal makine uzantısı için desteklenen platformlar, konfigürasyonlar ve dağıtım seçenekleri ayrıntılı olarak bulunmaktadır.

>[!NOTE]
>Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, Windows veya Linux için OMS aracısına Windows için Log Analytics Aracısı ve Linux için Log Analytics Aracısı olarak bahsedilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Linux dağıtımları hakkında daha fazla bilgi için [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) makalesine bakın.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM Uzantısı sürümü
Aşağıdaki tabloda, her sürüm için Log Analytics VM uzantısının ve Log Analytics Aracı paketinin sürümünün bir eşlemesi verilmiştir. Log Analytics aracı paketi sürümü için sürüm notlarına bir bağlantı dahildir. Sürüm notları, hata düzeltmeleriyle ilgili ayrıntıları ve belirli bir aracı sürümü için kullanılabilen yeni özellikleri içerir.  

| Log Analytics Linux VM Uzantısı sürümü | Log Analytics aracı paketi sürümü | 
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

Azure Güvenlik Merkezi, Log Analytics aracısını otomatik olarak sağlar ve Azure aboneliğinizde ASC tarafından oluşturulan varsayılan bir Log Analytics çalışma alanına bağlar. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımlarda çalıştırılmayın. Bunun yapılması, yapılandırılan çalışma alanının üzerine yazar ve Azure Güvenlik Merkezi ile bağlantıyı keser.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Log Analytics Aracısı uzantısı, hedef sanal makinenin Internet 'e bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Log Analytics aracı uzantısının şemasını gösterir. Uzantı, hedef Log Analytics çalışma alanından çalışma alanı KIMLIĞI ve çalışma alanı anahtarı gerektirir; Bu değerler, Azure portal [Log Analytics çalışma alanınızda bulunabilir](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) . Çalışma alanı anahtarı gizli veriler olarak değerlendirilmelidir, bu, korumalı bir ayar yapılandırmasında depolanmalıdır. Azure VM Uzantısı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. Çalışma alanı **kimliği** ve **workspaceKey** büyük/küçük harfe duyarlı olduğunu unutmayın.

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
>Yukarıdaki şemada, şablonun kök düzeyinde yerleştirileceği varsayılır. Şablonu şablondaki sanal makine kaynağının içine yerleştirirseniz, `type` `name` [daha](#template-deployment)sonra açıklandığı gibi, ve özellikleri değiştirilmelidir.

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer/örnek |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| yayımcı | Microsoft. EnterpriseCloud. Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| çalışma alanı kimliği (ör.) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (ör.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ = = |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonlarıyla dağıtılabilir. Azure Izleyici günlüklerine ekleme gibi dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıtımında şablonlar idealdir. Log Analytics Aracısı VM uzantısını içeren örnek bir Kaynak Yöneticisi şablonu [Azure hızlı başlangıç galerisinde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm)bulunabilir. 

Bir sanal makine uzantısının JSON yapılandırması, sanal makine kaynağının içinde iç içe veya Kaynak Yöneticisi JSON şablonunun kök veya üst düzeyine yerleştirilmiş olabilir. JSON yapılandırmasının yerleştirilmesi, kaynak adının ve türün değerini etkiler. Daha fazla bilgi için bkz. [alt kaynaklar için ad ve tür ayarlama](../../azure-resource-manager/templates/child-resource-name-type.md). 

Aşağıdaki örnek, VM uzantısının sanal makine kaynağının içinde iç içe olduğunu varsayar. Uzantı kaynağını yuvalama sırasında JSON, sanal makinenin `"resources": []` nesnesine yerleştirilir.

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

JSON uzantısını şablon köküne yerleştirirken, kaynak adı üst sanal makineye bir başvuru içerir ve tür iç içe geçmiş yapılandırmayı yansıtır.  

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

## <a name="azure-cli-deployment"></a>Azure CLı dağıtımı

Azure CLı, Log Analytics Agent VM uzantısını var olan bir sanal makineye dağıtmak için kullanılabilir. Aşağıdaki *myworkspacekey anahtarınızla* değerini çalışma alanı anahtarınızla ve *MyWorkspace ID* değerini çalışma alanı Kimliğinizle değiştirin. Bu değerler, *Gelişmiş ayarlar*altında Azure Portal Log Analytics çalışma alanınızda bulunabilir. 

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

Uzantı dağıtımlarının durumu hakkındaki veriler Azure portal ve Azure CLı kullanılarak alınabilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için, Azure CLı 'yı kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkışı aşağıdaki dosyaya kaydedilir:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylem |
| :---: | --- | --- |
| 9 | Etkinleştirme erken çağrıldı | [Azure Linux aracısını](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) kullanılabilir en son sürüme güncelleştirin. |
| 10 | VM zaten bir Log Analytics çalışma alanına bağlı | Sanal makineyi uzantı şemasında belirtilen çalışma alanına bağlamak için, genel ayarlarda stopOnMultipleConnections 'ı false olarak ayarlayın veya bu özelliği kaldırın. Bu VM, bağlandığı her çalışma alanı için bir kez faturalandırılır. |
| 11 | Uzantıya geçersiz yapılandırma belirtildi | Dağıtım için gerekli tüm özellik değerlerini ayarlamak için yukarıdaki örnekleri izleyin. |
| 17 | Log Analytics paketi yükleme hatası | 
| 19 | OMı paketi yükleme hatası | 
| 20 | SCX paket yükleme hatası |
| 51 | Bu uzantı, sanal makinenin işletim sisteminde desteklenmiyor | |
| 55 | Azure Izleyici hizmetine bağlanılamıyor veya gerekli paketler eksik veya dpkg Paket Yöneticisi kilitli| Sistemin Internet erişimi olup olmadığını ya da geçerli bir HTTP proxy 'si sağlandığını denetleyin. Ayrıca, çalışma alanı KIMLIĞININ doğruluğunu denetleyin ve kıvrımlı ve bataklık yardımcı programlarının yüklü olduğunu doğrulayın. |

Ek sorun giderme bilgileri [için Log Analytics-Agent-for-Linux sorun giderme kılavuzunda](../../azure-monitor/platform/vmext-troubleshoot.md)bulabilirsiniz.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
