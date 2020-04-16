---
title: Azure Veri Fabrikası'nda kopyalama etkinliğini kullanarak meta verileri ve AM'leri koruyun
description: Azure Veri Fabrikası'ndaki kopyalama etkinliğini kullanarak kopyalama sırasında meta verileri ve ABM'leri nasıl koruyacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: jingwang
ms.openlocfilehash: 5ce1b85394a7bb604841f7fb941bdebf12c0bca2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414164"
---
#  <a name="preserve-metadata-and-acls-using-copy-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda kopyalama etkinliğini kullanarak meta verileri ve AM'leri koruyun

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verileri kaynaktan batmaya kopyalamak için Azure Veri Fabrikası kopyalama etkinliğini kullandığınızda, aşağıdaki senaryolarda meta verileri ve ALA'ları da koruyabilirsiniz.

## <a name="preserve-metadata-for-lake-migration"></a><a name="preserve-metadata"></a>Göl geçişi için meta verileri koruma

Verileri bir veri gölünden Diğerine Amazon [S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md)ve [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md)gibi aktardığınızda, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz.

Kopyalama etkinliği, veri kopyalama sırasında aşağıdaki özniteliklerin korunmasını destekler:

- **Tüm müşteri belirtilen meta veri** 
- Ve aşağıdaki beş veri deposu yerleşik `contentType`sistem `contentLanguage` **özellikleri:**, (Amazon `contentDisposition` `cacheControl`S3 hariç), `contentEncoding`, , .

Dosyaları Amazon S3/Azure Veri Gölü Depolama Gen2/Azure Blob'dan Azure Veri Gölü Depolama Gen2/Azure Blob'a ikili biçimle kopyalarken, etkinlik yazma için > Etkinlik**Ayarlarını** **Kopyala**sekmesinde veya Veri Kopyalama Aracı'ndaki **Ayarlar** sayfasında **Koruma** seçeneğini bulabilirsiniz.

![Kopyalama etkinliği meta verileri koruma](./media/copy-activity-preserve-metadata/copy-activity-preserve-metadata.png)

Burada kopya etkinliği JSON yapılandırma bir `preserve`örnek (bakınız): 

```json
"activities":[
    {
        "name": "CopyAndPreserveMetadata",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "BinarySource",
                "storeSettings": {
                    "type": "AmazonS3ReadSettings",
                    "recursive": true
                }
            },
            "sink": {
                "type": "BinarySink",
                "storeSettings": {
                    "type": "AzureBlobFSWriteSettings"
                }
            },
            "preserve": [
                "Attributes"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Binary dataset Amazon S3/Azure Blob/ADLS Gen2 source>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Binary dataset for Azure Blob/ADLS Gen2 sink>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="preserve-acls-from-data-lake-storage-gen1gen2-to-gen2"></a><a name="preserve-acls"></a>Data Lake Storage Gen1/Gen2'den Gen2'ye ATL'leri Koruyun

Azure Veri Gölü Depolama Gen1'den Gen2'ye yükselttiğinde veya ADLS Gen2 arasındaki verileri kopyalarken, veri dosyalarıyla birlikte POSIX erişim denetim listelerini (ACD'ler) korumayı seçebilirsiniz. Erişim denetimi hakkında daha fazla bilgi için Azure [Veri Gölü Depolama Gen1'de Erişim denetimine](../data-lake-store/data-lake-store-access-control.md) ve [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](../storage/blobs/data-lake-storage-access-control.md)bakın.

Kopyalama etkinliği, veri kopyalama sırasında aşağıdaki ALA türlerinin korunmasını destekler. Bir veya daha fazla tür seçebilirsiniz:

- **ACL**: Dosyalar ve dizinler üzerindeki POSIX erişim kontrol listelerini kopyalayın ve koruyun. Varolan aPARAtların tamamını kaynaktan lavaboya kopyalar. 
- **Sahibi**: Dosya ve dizin sahibi kullanıcıyı kopyalayın ve saklayın. Veri Gölü Depolama Gen2 lavabo süper kullanıcı erişimi gereklidir.
- **Grup**: Sahip olduğu dosya ve dizin grubunu kopyalayın ve saklayın. Veri Gölü Depolama Gen2'ye veya sahibi kullanıcıya (sahibi kullanıcı da hedef grubun bir üyesiyse) süper kullanıcı erişimi gereklidir.

Bir klasörden kopyalamayı belirtirseniz, Veri Fabrikası o klasörün ALA'larını ve altında yer `recursive` alan dosyaları ve dizinleri doğru olarak ayarlanmışsa çoğaltır. Tek bir dosyadan kopyalamayı belirtirseniz, o dosyadaki ALA'lar kopyalanır.

>[!NOTE]
>Data Lake Storage Gen1/Gen2'den Gen2'ye kadar ATL'leri korumak için ADF kullandığınızda, lavabo Gen2'nin ilgili klasör/dosyalarındaki mevcut ACD'ler üzerine yazılır.

>[!IMPORTANT]
>ALA'ları korumayı seçtiğinizde, Veri Fabrikası'nın lavabo Veri Gölü Depolama Gen2 hesabınıza karşı çalışması için yeterince yüksek izin ler verdiğinizden emin olun. Örneğin, hesap anahtarı kimlik doğrulamasını kullanın veya Depolama Blob Veri Sahibi rolünü hizmet sorumlusuna veya yönetilen kimliğe atayın.

Kaynağı Ikili biçim veya ikili kopyalama seçeneğiyle Veri Gölü Depolama Gen1/Gen2 olarak yapılandırdığınızda ve ikili biçimli veya ikili kopyalama seçeneğiyle Veri Gölü Depolama Gen2 olarak battığınızda, Veri Aracı'nda **Ayarlar** sayfasında veya etkinlik yazma için **Etkinlik** > **Ayarları** sekmesinde **Koruma** seçeneğini bulabilirsiniz.

![Veri Gölü Depolama Gen1/Gen2 Gen2 Preserve ACL için](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

Burada kopya etkinliği JSON yapılandırma bir `preserve`örnek (bakınız): 

```json
"activities":[
    {
        "name": "CopyAndPreserveACLs",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "BinarySource",
                "storeSettings": {
                    "type": "AzureDataLakeStoreReadSettings",
                    "recursive": true
                }
            },
            "sink": {
                "type": "BinarySink",
                "storeSettings": {
                    "type": "AzureBlobFSWriteSettings"
                }
            },
            "preserve": [
                "ACL",
                "Owner",
                "Group"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Binary dataset name for Azure Data Lake Storage Gen1/Gen2 source>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Binary dataset name for Azure Data Lake Storage Gen2 sink>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Diğer Kopyalama Etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performansı](copy-activity-performance.md)
