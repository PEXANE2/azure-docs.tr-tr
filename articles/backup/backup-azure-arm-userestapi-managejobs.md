---
title: REST API'yi kullanarak Yedekleme İşlerini Yönetme
description: Bu makalede, REST API'yi kullanarak Azure Yedekleme'nin yedekleme ve geri yükleme işlerini nasıl izleyeceğinizi ve yöneteceğimizi öğrenin.
ms.topic: conceptual
ms.date: 08/03/2018
ms.assetid: b234533e-ac51-4482-9452-d97444f98b38
ms.openlocfilehash: 628569c547aa776ec2fbb7ec7e32edad7c1fe7dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273533"
---
# <a name="track-backup-and-restore-jobs-using-rest-api"></a>REST API'yi kullanarak yedekleme ve işleri geri yükleme

Azure Yedekleme hizmeti, yedeklemeyi tetikleme, işlemleri geri yükleme, yedeklemeyi devre dışı bırakma gibi çeşitli senaryolarda arka planda çalışan işleri tetikler. Bu işler kimliklerini kullanarak izlenebilir.

## <a name="fetch-job-information-from-operations"></a>Operasyonlardan İş bilgilerini getir

Yedeklemeyi tetikleme gibi bir işlem her zaman bir jobID döndürecektir. Örneğin: [Bir tetikleyici yedekleme REST API işleminin](backup-azure-arm-userestapi-backupazurevms.md#example-responses-3) son yanıtı aşağıdaki gibidir:

```http
{
  "id": "cd153561-20d3-467a-b911-cc1de47d4763",
  "name": "cd153561-20d3-467a-b911-cc1de47d4763",
  "status": "Succeeded",
  "startTime": "2018-09-12T02:16:56.7399752Z",
  "endTime": "2018-09-12T02:16:56.7399752Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "41f3e94b-ae6b-4a20-b422-65abfcaf03e5"
  }
}
```

Azure VM yedekleme işi "jobId" alanı tarafından tanımlanır ve [burada](https://docs.microsoft.com/rest/api/backup/jobdetails/) basit bir *GET* isteği kullanılarak izlenebilir.

## <a name="tracking-the-job"></a>İşi izleme

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2019-05-13
```

Yukarıda `{jobName}` bahsedilen "jobId" olduğunu. Yanıt her zaman 200 Tamam "durum" alanı işin geçerli durumunu gösteren. Bir kez "Tamamlandı" veya "TamamlanmışUyarılar" , 'genişletilmişInfo' bölümü iş hakkında daha fazla bilgi ortaya koymaktadır.

### <a name="response"></a>Yanıt

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     | [İş Kaynağı](https://docs.microsoft.com/rest/api/backup/jobdetails/get#jobresource)        | Tamam        |

#### <a name="example-response"></a>Örnek yanıt

*GET* URI gönderildikten sonra 200 (Tamam) yanıtı döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-client-request-id: ba4dff71-1655-4c1d-a71f-c9869371b18b; ba4dff71-1655-4c1d-a71f-c9869371b18b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14989
x-ms-correlation-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-routing-request-id: SOUTHINDIA:20180521T102317Z:e9702101-9da2-4681-bdf3-a54e17329a56
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:23:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-RecoveryServices-ResourceGroup-centralindia/providers/microsoft.recoveryservices/vaults/abdemovault/backupJobs/7ddead57-bcb9-4269-ac31-6a1b57588700",
  "name": "7ddead57-bcb9-4269-ac31-6a1b57588700",
  "type": "Microsoft.RecoveryServices/vaults/backupJobs",
  "properties": {
    "jobType": "AzureIaaSVMJob",
    "duration": "00:20:23.0896697",
    "actionsInfo": [
      1
    ],
    "virtualMachineVersion": "Compute",
    "extendedInfo": {
      "tasksList": [
        {
          "taskId": "Take Snapshot",
          "duration": "00:00:00",
          "status": "Completed"
        },
        {
          "taskId": "Transfer data to vault",
          "duration": "00:00:00",
          "status": "Completed"
        }
      ],
      "propertyBag": {
        "VM Name": "uttestvmub1",
        "Backup Size": "2332 MB"
      }
    },
    "entityFriendlyName": "uttestvmub1",
    "backupManagementType": "AzureIaasVM",
    "operation": "Backup",
    "status": "Completed",
    "startTime": "2018-05-21T08:35:40.9488967Z",
    "endTime": "2018-05-21T08:56:04.0385664Z",
    "activityId": "7df8e874-1d66-4f81-8e91-da2fe054811d"
  }
}
}

```
