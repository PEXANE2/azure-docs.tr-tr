---
title: Azure Veri Fabrikası'nda Azure tümleştirme çalışma zamanı oluşturma
description: Verileri kopyalamak ve dönüştürme etkinlikleri göndermek için kullanılan Azure Veri Fabrikası'nda Azure tümleştirme çalışma zamanı nasıl oluşturup oluşturabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/15/2018
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.openlocfilehash: 87633abaaae1f6034709c6e552be6647533115ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260767"
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Azure Tümleştirme Çalışma Süresi oluşturma ve yapılandırma
Tümleştirme Çalışma Süresi (IR), Azure Veri Fabrikası tarafından farklı ağ ortamlarında veri tümleştirme özellikleri sağlamak için kullanılan bilgi işlem altyapısıdır. IR hakkında daha fazla bilgi için [Tümleştirme çalışma zamanı'na](concepts-integration-runtime.md)bakın.

Azure IR, veri hareketini yerel olarak gerçekleştirmek ve HDInsight gibi hizmetleri hesaplamak için veri dönüştürme etkinlikleri göndermek için tam olarak yönetilen bir bilgi işlem sağlar. Azure ortamında barındırılır ve ortak ağ ortamındaki kaynaklara genel olarak erişilebilen uç noktalarıyla bağlanmayı destekler.

Bu belge, Azure Tümleştirme Çalışma Süresini nasıl oluşturabileceğinizi ve yapılandırabileceğinizi tanıtır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-azure-ir"></a>Varsayılan Azure IR
Varsayılan olarak, her veri fabrikasının arka ucunda, ortak ağdaki bulut veri depoları ve bilgi işlem hizmetleri yle ilgili işlemleri destekleyen bir Azure IR'si vardır. Azure IR'nin konumu otomatik çözümdür. **connectVia** özelliği bağlı hizmet tanımında belirtilmemişse, varsayılan Azure IR kullanılır. Ir'nin konumunu açıkça tanımlamak istediğinizde veya etkinlik yürütmelerini yönetim amacıyla farklı IR'lerde sanal olarak gruplandırmak istediğinizde açıkça bir Azure IR oluşturmanız gerekir. 

## <a name="create-azure-ir"></a>Azure IR oluşturma
Entegrasyon Runtime **Set-AzDataFactoryV2IntegrationRuntime** PowerShell cmdlet kullanılarak oluşturulabilir. Azure IR oluşturmak için, komuta adını, konumunu ve türünü belirtirsiniz. Burada konum "Batı Avrupa" olarak ayarlanmış bir Azure IR oluşturmak için örnek bir komut:

```powershell
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Azure IR için tür **Yönetilen**olarak ayarlanmalıdır. Bulutta elastik olarak tamamen yönetildiği için bilgi işlem ayrıntılarını belirtmeniz gerekmez. Azure-SSIS IR oluşturmak istediğinizde düğüm boyutu ve düğüm sayısı gibi bilgi işlem ayrıntılarını belirtin. Daha fazla bilgi için [bkz.](create-azure-ssis-integration-runtime.md)

Varolan bir Azure IR'sini Set-AzDataFactoryV2IntegrationRuntime PowerShell cmdlet'i kullanarak konumunu değiştirecek şekilde yapılandırabilirsiniz. Azure IR'nin konumu hakkında daha fazla bilgi için [bkz.](concepts-integration-runtime.md)

## <a name="use-azure-ir"></a>Azure IR'yi kullanma

Azure IR oluşturulduktan sonra, Bağlantılı Hizmet tanımınızda bu radıyama başvuruda bulunabilirsiniz. Aşağıda, yukarıda oluşturulan Azure Depolama Bağlantılı Hizmetinden oluşturulan Azure Tümleştirme Çalışma Süresi'ne nasıl başvuruyapabileceğinizne bir örnek verilmiştir:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=..."
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
Diğer tümleştirme çalışma süreleri türlerinin nasıl oluşturulacak ile ilgili aşağıdaki makalelere bakın:

- [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](create-self-hosted-integration-runtime.md)
- [Azure-SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)
 
