---
title: Azure Data Factory kopyalama etkinliğini kullanarak meta verileri ve ACL 'Leri koruma
description: Azure Data Factory kopyalama etkinliğini kullanarak kopyalama sırasında meta verileri ve ACL 'Leri koruma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: jingwang
ms.openlocfilehash: 056909f5fd5838e5ae50fb84bd3535029d862acf
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79260845"
---
#  <a name="preserve-metadata-and-acls-using-copy-activity-in-azure-data-factory"></a>Azure Data Factory kopyalama etkinliğini kullanarak meta verileri ve ACL 'Leri koruma

Verileri kaynaktan havuza kopyalamak için Azure Data Factory kopyalama etkinliğini kullandığınızda, aşağıdaki senaryolarda meta verileri ve ACL 'Leri de koruyabilirsiniz.

## <a name="preserve-metadata"></a>Lake geçişi için meta verileri koruma

Bir Data Lake 'tan [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md)ve [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md)dahil olmak üzere başka bir veri geçirdiğinizde, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz.

Kopyalama etkinliği, veri kopyalama sırasında aşağıdaki özniteliklerin korunması destekler:

- **Müşterinin belirttiği tüm meta veriler** 
- Ve aşağıdaki **beş veri deposu yerleşik sistem özellikleri**: `contentType`, `contentLanguage` (Amazon S3 hariç), `contentEncoding`, `contentDisposition`, `cacheControl`.

Dosyaları Amazon S3/Azure Data Lake Storage 2./Azure blobundan ikili biçimdeki Azure Data Lake Storage 2./Azure Blobuna kopyalamak için, **koruma** seçeneğini, etkinlik yazma için **etkinlik** > **ayarları** sekmesine veya veri kopyalama aracındaki **Ayarlar** sayfasına bulabilirsiniz.

![Kopyalama etkinliği meta verilerini koru](./media/copy-activity-preserve-metadata/copy-activity-preserve-metadata.png)

Kopyalama etkinliği JSON yapılandırmasına bir örnek aşağıda verilmiştir (bkz. `preserve`): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
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

## <a name="preserve-acls"></a>Data Lake Storage 1. ACL 'Leri koruma ile Gen2

Azure Data Lake Storage 1. 'den Gen2 'e yükselttiğinizde, POSIX erişim denetim listelerini (ACL 'Ler) veri dosyalarıyla birlikte korumayı seçebilirsiniz. Access Control hakkında daha fazla bilgi için Azure Data Lake Storage 2. Azure Data Lake Storage 1. ve [erişim denetimi](../storage/blobs/data-lake-storage-access-control.md) [' nde erişim denetimi](../data-lake-store/data-lake-store-access-control.md) ' ne bakın.

Kopyalama etkinliği, veri kopyalama sırasında aşağıdaki ACL türlerinin korunması destekler. Bir veya daha fazla tür seçebilirsiniz:

- **ACL**: dosya ve dizinlerde POSIX erişim denetim listelerini kopyalayın ve koruyun. Var olan tüm ACL 'Leri kaynaktan havuza kopyalar. 
- **Sahip**: dosya ve dizinlerin sahip olduğu kullanıcıyı kopyalayın ve koruyun. Data Lake Storage 2. havuza Süper Kullanıcı erişimi gerekiyor.
- **Grup**: sahip olan dosya ve dizinlerin grubunu kopyalayın ve koruyun. Havuz Data Lake Storage 2. veya sahip olan kullanıcıya yönelik süper kullanıcı erişimi (sahip olan kullanıcı aynı zamanda hedef grubun üyesiyse) gerekli.

Bir klasörden kopyalamayı belirtirseniz Data Factory, bu klasör için ACL 'Leri ve `recursive` doğru olarak ayarlandıysa, altındaki dosyaları ve dizinleri çoğaltır. Tek bir dosyadan kopyalamayı belirtirseniz, bu dosyadaki ACL 'Ler kopyalanır.

>[!NOTE]
>Data Lake Storage 1. ACL 'Leri Gen2 ile korumak için ADF kullandığınızda, buna karşılık gelen klasöre/dosyalara yönelik var olan ACL 'Lerin üzerine yazılır.

>[!IMPORTANT]
>ACL 'Leri korumayı seçtiğinizde, Data Factory havuz Data Lake Storage 2. hesabınızda çalışması için yeterince yüksek izinler verdiğinizden emin olun. Örneğin, hesap anahtarı kimlik doğrulamasını kullanın veya Depolama Blobu veri sahibi rolünü hizmet sorumlusu veya yönetilen kimliğe atayın.

Kaynağı ikili biçimi veya ikili kopya seçeneğini içeren Data Lake Storage 1. olarak, ikili biçimi veya ikili kopya seçeneğiyle Data Lake Storage 2. olarak yeniden yapılandırdığınızda, Veri Kopyalama aracındaki **Ayarlar** sayfasında veya etkinlik yazma Için **etkinliği Kopyala** > **Ayarlar** sekmesinde **koru** seçeneğini bulabilirsiniz.

![ACL 'yi korumak için Data Lake Storage 1. Gen2](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

Kopyalama etkinliği JSON yapılandırmasına bir örnek aşağıda verilmiştir (bkz. `preserve`): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
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
                "referenceName": "<Binary dataset name for Azure Data Lake Storage Gen1 source>",
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

Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Etkinlik performansını Kopyala](copy-activity-performance.md)
