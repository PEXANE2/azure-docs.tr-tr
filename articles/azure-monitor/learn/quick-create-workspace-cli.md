---
title: Azure CLI kullanarak Günlük Analizi çalışma alanı oluşturun | Microsoft Dokümanlar
description: Azure CLI ile bulut ve şirket içi ortamlarınızdan yönetim çözümleri ve veri toplama yı etkinleştirmek için Bir Günlük Analizi çalışma alanı oluşturmayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2019
ms.openlocfilehash: 89d397574c423e28bcbb0fec5ddd45959a737a93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659896"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Azure CLI 2.0 ile Günlük Analizi çalışma alanı oluşturma

Azure CLI 2.0, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Azure Monitor'da Bir Günlük Analizi çalışma alanını dağıtmak için Azure CLI 2.0'ı nasıl kullanacağınızı gösterir. Log Analytics çalışma alanı, Azure Monitor günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolayabilmek üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamak istiyorsanız bir Log Analytics çalışma alanına ihtiyacınız vardır:

* Aboneliğinizdeki Azure kaynakları  
* Sistem Merkezi Operasyon Yöneticisi tarafından izlenen şirket içi bilgisayarlar  
* Configuration Manager'dan aygıt koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  

Ortamınızdaki Azure VM'leri ve Windows veya Linux VM'leri gibi diğer kaynaklar için aşağıdaki konulara bakın:

* [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md)
* [Karma Linux bilgisayarından veri toplama](../learn/quick-collect-linux-computer.md)
* [Karma Windows bilgisayarından veri toplama](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
az grup dağıtımı oluşturmak ile bir çalışma alanı [oluşturun.](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create) Aşağıdaki örnek, yerel makinenizden gelen Kaynak Yöneticisi şablonunu kullanarak *doğu daki* konumda bir çalışma alanı oluşturur. JSON şablonu yalnızca çalışma alanının adını istenecek şekilde yapılandırılır ve ortamınızda standart yapılandırma olarak kullanılabilecek diğer parametreler için varsayılan bir değer belirtir. Veya kuruluşunuzdaki paylaşılan erişim için şablonu bir Azure depolama hesabında saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-cli.md) bakın

Desteklenen bölgeler hakkında bilgi için, günlük [analizinin bulunduğu bölgelere](https://azure.microsoft.com/regions/services/) bakın ve **ürün arama** alanından Azure Monitörünü arayın.

Aşağıdaki parametreler varsayılan değer kümesi:

* yer - Doğu ABD varsayılan
* sku - Nisan 2018 fiyatlandırma modelinde yayımlanan yeni GB Başına fiyatlandırma katmanı için varsayılan

>[!WARNING]
>Yeni Nisan 2018 fiyatlandırma modeline dahil olan bir abonelikte Log Analytics çalışma alanı oluşturmak veya yapılandırmak ise, tek geçerli Log Analytics fiyatlandırma katmanı **PerGB2018'dir.**
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

2. Gereksinimlerinizi karşılamak için şablonu edin. Hangi özelliklerin ve değerlerin desteklenildiğini öğrenmek için [Microsoft.OperationalInsights/çalışma alanları şablonu](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvurularını inceleyin.
3. Bu dosyayı yerel bir klasöre **deploylaworkspacetemplate.json** olarak kaydedin.   
4. Bu şablonu dağıtmaya hazırsınız. Şablonu içeren klasörden aşağıdaki komutları kullanın. Bir çalışma alanı adı için istendiğinde, tüm Azure aboneliklerinde genel olarak benzersiz bir ad sağlayın.

    ```azurecli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Bittiğinde, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonuç](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık kullanılabilir bir çalışma alanınız olduğuna göre, izleme telemetrisi koleksiyonunu yapılandırabilir, bu verileri çözümlemek için günlük aramaları çalıştırabilir ve ek veriler ve analitik öngörüler sağlamak için bir yönetim çözümü ekleyebilirsiniz.  

* Azure Tanılama veya Azure depolama alanıyla Azure kaynaklarından veri toplamayı etkinleştirmek [için, Log Analytics'te kullanılmak üzere Azure hizmet günlüklerini ve ölçümlerini topla'ya](../platform/collect-azure-metrics-logs.md)bakın.  
* Operasyon Yöneticisi yönetim grubunuzu bildiren aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [Sistem Merkezi Operasyon Yöneticisi'ni veri kaynağı olarak](../platform/om-agents.md) ekleyin.  
* [Configuration Manager'ı](../platform/collect-sccm.md) hiyerarşideki koleksiyonların üyesi olan bilgisayarları içe aktarmaya bağlayın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümü nasıl ekleyeceğiniz veya kaldırılanızı gözden geçirin.
