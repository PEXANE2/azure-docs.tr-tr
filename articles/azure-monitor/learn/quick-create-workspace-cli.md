---
title: Azure CLı kullanarak Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure CLı ile bulut ve şirket içi ortamlarınızdaki yönetim çözümlerini ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/26/2020
ms.openlocfilehash: d7c3613a0ddce217e9d07c3fc2eba55b78c53a1b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86516575"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Azure CLı 2,0 ile Log Analytics çalışma alanı oluşturma

Azure CLI 2.0, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta, Azure Izleyici 'de bir Log Analytics çalışma alanı dağıtmak için Azure CLı 2,0 'nin nasıl kullanılacağı gösterilmektedir. Log Analytics çalışma alanı, Azure Izleyici günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamayı planlıyorsanız bir Log Analytics çalışma alanı gerekir:

* Aboneliğinizdeki Azure kaynakları  
* System Center Operations Manager tarafından izlenen şirket içi bilgisayarlar  
* Configuration Manager cihaz koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  

Ortamınızdaki Azure VM 'Leri ve Windows veya Linux VM 'Leri gibi diğer kaynaklar için aşağıdaki konulara bakın:

* [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md)
* [Karma Linux bilgisayarından veri topla](../learn/quick-collect-linux-computer.md)
* [Karma Windows bilgisayarından veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
[Az Group Deployment Create](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)komutuyla bir çalışma alanı oluşturun. Aşağıdaki örnek, yerel makinenizden Kaynak Yöneticisi şablonu kullanarak *eastus* konumunda bir çalışma alanı oluşturur. JSON şablonu yalnızca size çalışma alanının adını isteyecek şekilde yapılandırılmıştır ve ortamınızda standart bir yapılandırma olarak kullanılabilecek diğer parametreler için varsayılan bir değer belirtir. İsterseniz, şablonu kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında da saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Desteklenen bölgeler hakkında daha fazla bilgi için, bkz. [Log Analytics bölgeler kullanılabilir](https://azure.microsoft.com/regions/services/) ve **bir ürün Için aramadan** Azure izleyici araması yapın.

Aşağıdaki parametreler varsayılan bir değer ayarlar:

* konum-varsayılan olarak Doğu ABD
* SKU-Nisan 2018 fiyatlandırma modelinde yayınlanan yeni GB başına fiyatlandırma katmanına varsayılan olarak sahiptir

>[!WARNING]
>Yeni Nisan 2018 fiyatlandırma modelini kabul eden bir abonelikte Log Analytics çalışma alanı oluşturuyor veya yapılandırıyorsanız, geçerli Log Analytics fiyatlandırma katmanı yalnızca **PerGB2018**olur.
>

### <a name="create-and-deploy-template"></a>Şablon oluşturma ve dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Gereksinimlerinizi karşılayacak şekilde şablonu düzenleyin. Hangi özelliklerin ve değerlerin desteklendiğini öğrenmek için [Microsoft. Operationalınsights/Workspaces şablon](/azure/templates/microsoft.operationalinsights/2015-11-01-preview/workspaces) başvurusunu gözden geçirin.
3. Bu dosyayı yerel bir klasöre **deploylaworkspacetemplate.js** olarak kaydedin.   
4. Bu şablonu dağıtmaya hazırsınız. Şablonu içeren klasörden aşağıdaki komutları kullanın. Bir çalışma alanı adı sorulduğunda, tüm Azure abonelikleri genelinde genel olarak benzersiz bir ad sağlayın.

    ```azurecli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonuç](media/quick-create-workspace-cli/template-output-01.png)

## <a name="troubleshooting"></a>Sorun giderme
Son 14 gün içinde silinen bir çalışma alanı oluşturduğunuzda ve [geçici silme durumunda](../platform/delete-workspace.md#soft-delete-behavior)işlem, çalışma alanı yapılandırmanıza bağlı olarak farklı bir sonuca sahip olabilir:
1. Silinen çalışma alanında aynı çalışma alanı adı, kaynak grubu, abonelik ve bölge sağlarsanız, çalışma alanınız veri, yapılandırma ve bağlı aracılar dahil kurtarılacak.
2. Aynı çalışma alanı adını, ancak farklı kaynak grubunu, aboneliği veya bölgeyi kullanıyorsanız, *' Workspace-Name ' çalışma alanı adı benzersiz değil*veya *Çakışma*olduğunda bir hata alırsınız. Geçici silme işlemini geçersiz kılmak ve çalışma alanınızı kalıcı olarak silmek ve aynı ada sahip yeni bir çalışma alanı oluşturmak için, önce çalışma alanını kurtarmak ve kalıcı silme gerçekleştirmek için şu adımları izleyin:
   * Çalışma alanınızı [kurtarın](../platform/delete-workspace.md#recover-workspace)
   * Çalışma alanınızı [kalıcı olarak silme](../platform/delete-workspace.md#permanent-workspace-delete)
   * Aynı çalışma alanı adını kullanarak yeni bir çalışma alanı oluştur

## <a name="next-steps"></a>Sonraki adımlar
Artık kullanılabilir bir çalışma alanınız olduğuna göre, izleme telemetri koleksiyonunu yapılandırabilir, bu verileri çözümlemek için günlük aramalarını çalıştırabilir ve ek veri ve analitik Öngörüler sağlamak için bir yönetim çözümü ekleyebilirsiniz.  

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için, bkz. [Log Analytics Kullanım Için Azure hizmet günlükleri ve ölçümleri toplama](../platform/resource-logs.md#send-to-log-analytics-workspace).  
* Operations Manager yönetim grubunuzu raporlayan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [veri kaynağı olarak System Center Operations Manager](../platform/om-agents.md) ekleyin.  
* Hiyerarşideki koleksiyonların üyesi olan bilgisayarları içeri aktarmak için [Configuration Manager](../platform/collect-sccm.md) bağlanın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümün nasıl ekleneceğini ve kaldırılacağını gözden geçirin.
