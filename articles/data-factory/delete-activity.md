---
title: Azure Data Factory’de Silme Etkinliği
description: Azure Veri Fabrikası'ndaki Silme Etkinliği ile çeşitli dosya mağazalarındaki dosyaları nasıl sileyin öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f265cdc955becd53ae7ba61ad827b2be69b92907
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618276"
---
# <a name="delete-activity-in-azure-data-factory"></a>Azure Data Factory’de Silme Etkinliği

Azure Veri Fabrikası'ndaki Silme Etkinliğini, şirket içi depolama mağazalarından veya bulut depolama mağazalarından dosya veya klasörleri silmek için kullanabilirsiniz. Artık ihtiyaç duyulmadığında dosyaları temizlemek veya arşivlemek için bu etkinliği kullanın.

> [!WARNING]
> Silinen dosya veya klasörler geri yüklenemez (depolama alanı yumuşak silme etkin değilse). Dosya veya klasörleri silmek için Silme etkinliğini kullanırken dikkatli olun.

## <a name="best-practices"></a>En iyi uygulamalar

Sil etkinliğini kullanmak için bazı öneriler şunlardır:

-   Gelecekte geri yüklemeniz gerektiğinde dosyalarınızı Sil etkinliğiyle silmeden önce yedekleyin.

-   Veri Fabrikası'nın depolama deposundan klasörleri veya dosyaları silmek için yazma izinlerine sahip olduğundan emin olun.

-   Aynı anda yazılan dosyaları silmediğinizden emin olun. 

-   Dosyaları veya klasörü şirket içi bir sistemden silmek istiyorsanız, 3,14'ten büyük bir sürümü olan kendi kendine barındırılan bir tümleştirme çalışma zamanı kullandığınızdan emin olun.

## <a name="supported-data-stores"></a>Desteklenen veri depoları

-   [Azure Blob depolama](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
-   [Azure Dosya Depolama](connector-azure-file-storage.md)

### <a name="file-system-data-stores"></a>Dosya sistemi veri depoları

-   [Dosya Sistemi](connector-file-system.md)
-   [FTP](connector-ftp.md)
-   [SFTP](connector-sftp.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)
-   [Google Cloud Storage](connector-google-cloud-storage.md)

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "DeleteActivity",
    "type": "Delete",
    "typeProperties": {
        "dataset": {
            "referenceName": "<dataset name>",
            "type": "DatasetReference"
        },
        "recursive": true/false,
        "maxConcurrentConnections": <number>,
        "enableLogging": true/false,
        "logStorageSettings": {
            "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference"
            },
            "path": "<path to save log file>"
        }
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Dataset | Hangi dosya veya klasörün silineceğini belirlemek için veri kümesi başvurusu sağlar | Evet |
| Özyinelemeli | Dosyaların alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak silinip silinmediğini gösterir.  | Hayır. Varsayılan değer: `false`. |
| maxConcurrentConnections | Klasör veya dosyaları silerken aynı anda depolama deposuna bağlanacak bağlantıların sayısı.   |  Hayır. Varsayılan değer: `1`. |
| etkinleştirme günlüğü | Silinen klasörü veya dosya adlarını kaydetmeniz gerekip gerekmediğini gösterir. Doğruysa, günlük dosyasını okuyarak Sil etkinliğinin davranışlarını izleyebilmeniz için günlük dosyasını kaydetmek için bir depolama hesabı sağlamanız gerekir. | Hayır |
| logStorageSettings | Yalnızca etkinleştirildiğinde uygulanabilir = doğru.<br/><br/>Sil etkinliği tarafından silinen klasör veya dosya adlarını içeren günlük dosyasını kaydetmek istediğiniz yerde belirtilebilen bir depolama özelliği grubu. | Hayır |
| linkedServiceName | Yalnızca etkinleştirildiğinde uygulanabilir = doğru.<br/><br/>Azure [Depolama](connector-azure-blob-storage.md#linked-service-properties), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md#linked-service-properties)veya Azure Veri Gölü Depolama [Gen2'nin](connector-azure-data-lake-storage.md#linked-service-properties) bağlantılı hizmeti, Sil etkinliği tarafından silinen klasör veya dosya adlarını içeren günlük dosyasını depolamak için. Dosyaları silmek için silme etkinliği tarafından kullanılan dan Tümleştirme Runtime aynı tür ile yapılandırılması gerektiğini unutmayın. | Hayır |
| yol | Yalnızca etkinleştirildiğinde uygulanabilir = doğru.<br/><br/>Günlük dosyasını depolama hesabınıza kaydetme yolu. Bir yol sağlamazsanız, hizmet sizin için bir kapsayıcı oluşturur. | Hayır |

## <a name="monitoring"></a>İzleme

Sil etkinliğinin sonuçlarını görebileceğiniz ve izleyebileceğiniz iki yer vardır: 
-   Sil etkinliğinin çıktısından.
-   Günlük dosyasından.

### <a name="sample-output-of-the-delete-activity"></a>Sil etkinliğinin örnek çıktısı

```json
{ 
  "datasetName": "AmazonS3",
  "type": "AmazonS3Object",
  "prefix": "test",
  "bucketName": "adf",
  "recursive": true,
  "isWildcardUsed": false,
  "maxConcurrentConnections": 2,  
  "filesDeleted": 4,
  "logPath": "https://sample.blob.core.windows.net/mycontainer/5c698705-a6e2-40bf-911e-e0a927de3f07",
  "effectiveIntegrationRuntime": "MyAzureIR (West Central US)",
  "executionDuration": 650
}
```

### <a name="sample-log-file-of-the-delete-activity"></a>Sil etkinliğinin örnek günlük dosyası

| Adı | Kategori | Durum | Hata |
|:--- |:--- |:--- |:--- |
| test1/yyy.json | Dosya | Silme |  |
| test2/hello789.txt | Dosya | Silme |  |
| test2/test3/hello000.txt | Dosya | Silme |  |
| test2/test3/zzz.json | Dosya | Silme |  |

## <a name="examples-of-using-the-delete-activity"></a>Sil etkinliğini kullanma örnekleri

### <a name="delete-specific-folders-or-files"></a>Belirli klasörleri veya dosyaları silme

Mağazaaşağıdaki klasör yapısına sahiptir:

Kök/<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt

Şimdi, veri kümesinden ve Sil etkinliğinden farklı özellik değeri birleşimi ile klasör veya dosyaları silmek için Sil etkinliğini kullanıyorsunuz:

| folderPath (dataset'ten) | fileName (dataset'ten) | özyinelemeli (Sil etkinliğinden) | Çıktı |
|:--- |:--- |:--- |:--- |
| Kök / Folder_A_2 | NULL | False | Kök/<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Kök / Folder_A_2 | NULL | True | Kök/<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_A_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_1/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>7.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8.txt</strike> |
| Kök / Folder_A_2 | *.txt | False | Kök/<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Kök / Folder_A_2 | *.txt | True | Kök/<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8.txt</strike> |

### <a name="periodically-clean-up-the-time-partitioned-folder-or-files"></a>Zaman bölümlü klasörü veya dosyaları düzenli aralıklarla temizleyin

Zaman bölümlenmiş klasörü veya dosyaları düzenli olarak temizlemek için bir ardışık hatlar oluşturabilirsiniz.  Örneğin, klasör yapısı benzer: `/mycontainer/2018/12/14/*.csv`.  ADF sistem değişkenini zamanlama tetikleyicisinden, her bir ardışık düzen çalışmasında hangi klasörün veya dosyaların silinmesi gerektiğini belirlemek için kullanabilirsiniz. 

#### <a name="sample-pipeline"></a>Örnek boru hattı

```json
{
    "name": "cleanup_time_partitioned_folder",
    "properties": {
        "activities": [
            {
                "name": "DeleteOneFolder",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "PartitionedFolder",
                        "type": "DatasetReference",
                        "parameters": {
                            "TriggerTime": {
                                "value": "@formatDateTime(pipeline().parameters.TriggerTime, 'yyyy/MM/dd')",
                                "type": "Expression"
                            }
                        }
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ],
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/pipelines"
}
```

#### <a name="sample-dataset"></a>Örnek veri kümesi

```json
{
    "name": "PartitionedFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": {
                "value": "@concat('mycontainer/',dataset().TriggerTime)",
                "type": "Expression"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/datasets"
}
```

#### <a name="sample-trigger"></a>Örnek tetikleyici

```json
{
    "name": "DailyTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipelines": [
            {
                "pipelineReference": {
                    "referenceName": "cleanup_time_partitioned_folder",
                    "type": "PipelineReference"
                },
                "parameters": {
                    "TriggerTime": "@trigger().scheduledTime"
                }
            }
        ],
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Day",
                "interval": 1,
                "startTime": "2018-12-13T00:00:00.000Z",
                "timeZone": "UTC",
                "schedule": {
                    "minutes": [
                        59
                    ],
                    "hours": [
                        23
                    ]
                }
            }
        }
    }
}
```

### <a name="clean-up-the-expired-files-that-were-last-modified-before-201811"></a>En son değiştirilen süresi dolan dosyaları 2018.1.1'den önce temizleme

Dosya öznitelik filtresinden yararlanarak eski veya süresi dolmuş dosyaları temizlemek için bir ardışık kaynak oluşturabilirsiniz: veri kümesinde "LastModified".  

#### <a name="sample-pipeline"></a>Örnek boru hattı

```json
{
    "name": "CleanupExpiredFiles",
    "properties": {
        "activities": [
            {
                "name": "DeleteFilebyLastModified",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "BlobFilesLastModifiedBefore201811",
                        "type": "DatasetReference"
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ]
    }
}
```

#### <a name="sample-dataset"></a>Örnek veri kümesi

```json
{
    "name": "BlobFilesLastModifiedBefore201811",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "*",
            "folderPath": "mycontainer",
            "modifiedDatetimeEnd": "2018-01-01T00:00:00.000Z"
        }
    }
}
```

### <a name="move-files-by-chaining-the-copy-activity-and-the-delete-activity"></a>Kopyalama etkinliğini ve Sil etkinliğini zincirleyerek dosyaları taşıma

Bir dosyayı kopyalamak için bir kopyalama etkinliği kullanarak bir dosyayı taşıyabilir ve ardından ardışık düzendeki bir dosyayı silmek için silme etkinliğini silebilirsiniz.  Birden çok dosya taşımak istediğinizde, GetMetadata etkinliğini + Filtre etkinliğini + Foreach etkinliğini + Kopyalama etkinliğini + Aşağıdaki örnekteki gibi silme etkinliğini kullanabilirsiniz:

> [!NOTE]
> Yalnızca klasör yolunu içeren bir veri kümesi tanımlayarak ve ardından bir klasörü temsil eden aynı veri kümesine başvurmak için bir kopyalama etkinliği ve Silme etkinliğini kullanarak tüm klasörü taşımak istiyorsanız, çok dikkatli olmanız gerekir. Bunun nedeni, kopyalama işlemi ile silme işlemi arasında klasöre yeni dosyaların gelmediğinden emin olmak zorunda olduğunuziçindir.  Kopyalama etkinliğinizin kopyalama işini tamamladığı ancak Silme etkinliğine bakılmadığını niçin klasöre gelen yeni dosyalar varsa, Delete etkinliği henüz tüm klasörü silerek hedefe kopyalanmamış olan bu yeni gelen dosyayı silebilir. 

#### <a name="sample-pipeline"></a>Örnek boru hattı

```json
{
    "name": "MoveFiles",
    "properties": {
        "activities": [
            {
                "name": "GetFileList",
                "type": "GetMetadata",
                "typeProperties": {
                    "dataset": {
                        "referenceName": "OneSourceFolder",
                        "type": "DatasetReference"
                    },
                    "fieldList": [
                        "childItems"
                    ]
                }
            },
            {
                "name": "FilterFiles",
                "type": "Filter",
                "dependsOn": [
                    {
                        "activity": "GetFileList",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('GetFileList').output.childItems",
                        "type": "Expression"
                    },
                    "condition": {
                        "value": "@equals(item().type, 'File')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "ForEachFile",
                "type": "ForEach",
                "dependsOn": [
                    {
                        "activity": "FilterFiles",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('FilterFiles').output.value",
                        "type": "Expression"
                    },
                    "batchCount": 20,
                    "activities": [
                        {
                            "name": "CopyAFile",
                            "type": "Copy",
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": false
                                },
                                "sink": {
                                    "type": "BlobSink"
                                },
                                "enableStaging": false,
                                "dataIntegrationUnits": 0
                            },
                            "inputs": [
                                {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "OneDestinationFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "DestinationFileName": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "name": "DeleteAFile",
                            "type": "Delete",
                            "dependsOn": [
                                {
                                    "activity": "CopyAFile",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "dataset": {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                },
                                "logStorageSettings": {
                                    "linkedServiceName": {
                                        "referenceName": "BloblinkedService",
                                        "type": "LinkedServiceReference"
                                    },
                                    "path": "Container/log"
                                },
                                "enableLogging": true
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```

#### <a name="sample-datasets"></a>Örnek veri kümeleri

Dosya listesini listelemek için GetMetadata etkinliği tarafından kullanılan veri kümesi.

```json
{
    "name": "OneSourceFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "",
            "folderPath": "myFolder"
        }
    }
}
```

Kopyalama etkinliği ve Sil etkinliği tarafından kullanılan veri kaynağı için veri kümesi.

```json
{
    "name": "OneSourceFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            },
            "filename": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().filename",
                "type": "Expression"
            },
            "folderPath": {
                "value": "@{dataset().path}",
                "type": "Expression"
            }
        }
    }
}
```

Kopyalama etkinliği tarafından kullanılan veri hedefi için veri kümesi.

```json
{
    "name": "OneDestinationFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "DestinationFileName": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().DestinationFileName",
                "type": "Expression"
            },
            "folderPath": "mycontainer/dest"
        }
    }
}
```

Dosyaları [buradan](solution-template-move-files.md)taşımak için şablonu da alabilirsiniz.

## <a name="known-limitation"></a>Bilinen sınırlama

-   Silme etkinliği joker karakter tarafından açıklanan klasörlerin listesini silmeyi desteklemez.

-   Dosya öznitelik filtresi kullanırken: modifiedDatetimeStart ve modifiedDatetimeSilinecek dosyaları seçmek için, veri kümesinde "fileName": "*" ayarlamak için emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda dosya taşıma hakkında daha fazla bilgi edinin.

-   [Azure Data Factory'deki Veri Kopyalama aracı](copy-data-tool.md)
