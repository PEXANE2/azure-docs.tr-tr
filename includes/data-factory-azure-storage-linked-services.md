---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 37917e0ed663675677f1d0452b5796120ca2694e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75469342"
---
### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
**Azure Depolama bağlantılı hizmet,** veri fabrikasına Azure Depolama'ya küresel erişim sağlayan hesap **anahtarını**kullanarak bir Azure depolama hesabını Azure veri fabrikasına bağlamanızı sağlar. Aşağıdaki tablo, Azure Depolama bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureStorage** |Evet |
| Connectionstring |ConnectionString özelliği için Azure depolamasına bağlanmak için gereken bilgileri belirtin. |Evet |

Depolama hesabı erişim anahtarlarını nasıl alınız hakkında bilgi için [bkz.](../articles/storage/common/storage-account-keys-manage.md)

**Örnek:**  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="azure-storage-sas-linked-service"></a>Azure Depolama Sas Bağlantılı Hizmet
Paylaşılan erişim imzası (SAS), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir istemciye, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan, belirli bir süre için ve belirli bir izin kümesiyle depolama hesabınızdaki nesnelere sınırlı izinler verme olanağı sağlar. SAS, sorgu parametrelerinde bir depolama kaynağına doğrulanmış erişim için gerekli tüm bilgileri kapsayan bir URI'dir. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca SAS'ta uygun oluşturucu ya da yönteme geçmesi gerekir. SAS hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../articles/storage/common/storage-sas-overview.md)ver.

> [!IMPORTANT]
> Azure Veri Fabrikası artık yalnızca **Hizmet SAS'ı** destekler, ancak Hesap SAS'ı desteklemez. Azure portalından veya Storage Explorer'dan generable SAS URL'sinin desteklenmeyen bir Hesap SAS olduğunu unutmayın.

> [!TIP]
> Depolama hesabınız için bir Service SAS oluşturmak için PowerShell komutlarının altında uygulayabilirsiniz (yer tutucuları değiştirin ve gerekli izni verin):`$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Azure Depolama SAS bağlantılı hizmeti, Paylaşılan Erişim İmzası (SAS) kullanarak bir Azure Depolama Hesabı'nı Azure veri fabrikasına bağlamanızı sağlar. Veri fabrikasına depolamadaki tüm/belirli kaynaklara (blob/konteyner) sınırlı/zamana bağlı erişim sağlar. Aşağıdaki tablo, Azure Depolama SAS bağlantılı hizmetine özgü JSON öğeleri için açıklama sağlar. 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Depolama kaynaklarına Paylaşılan Erişim İmza URI'sini belirtin.  |Evet |

**Örnek:**

```json
{
    "name": "StorageSasLinkedService",
    "properties": {
        "type": "AzureStorageSas",
        "typeProperties": {
            "sasUri": "<Specify SAS URI of the Azure Storage resource>"
        }
    }
}
```

Bir **SAS URI**oluştururken, aşağıdakileri göz önünde bulundurarak:  

* Veri fabrikanızda bağlantılı hizmetin (okuma, yazma, okuma/yazma) nasıl kullanıldığına bağlı olarak nesneler üzerinde uygun okuma/yazma **izinleri** ayarlayın.
* **Son kullanma süresini** uygun şekilde ayarlayın. Azure Depolama nesnelerine erişimin ardışık ardışık aygıtın etkin süresi içinde sona ermediğinden emin olun.
* Uri ihtiyaca göre doğru konteyner/blob veya Tablo düzeyinde oluşturulmalıdır. Bir Azure blob için Bir SAS Uri Veri Fabrikası hizmeti belirli bir blob erişmesine olanak sağlar. Azure blob konteynerine bir SAS Uri, Veri Fabrikası hizmetinin bu kapsayıcıdaki lekeler aracılığıyla tekrarlanmasına olanak tanır. Daha sonra daha fazla/daha az nesneye erişim sağlamanız veya SAS URI'yi güncelleştirmeniz gerekiyorsa, bağlantılı hizmeti yeni URI ile güncelleştirmeyi unutmayın.   

