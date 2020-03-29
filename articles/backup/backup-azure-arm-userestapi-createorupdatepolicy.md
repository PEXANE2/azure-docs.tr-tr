---
title: REST API'yi kullanarak yedekleme ilkeleri oluşturma
description: Bu makalede, REST API'yi kullanarak yedekleme ilkeleri (zamanlama ve bekletme) oluşturmayı ve nasıl yöneteceğinizi öğreneceksiniz.
ms.topic: conceptual
ms.date: 08/21/2018
ms.assetid: 5ffc4115-0ae5-4b85-a18c-8a942f6d4870
ms.openlocfilehash: 0718ebc3612f53f1c2cc279096dd92de69bb5ef6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76963861"
---
# <a name="create-azure-recovery-services-backup-policies-using-rest-api"></a>REST API'yi kullanarak Azure Kurtarma Hizmetleri yedekleme ilkeleri oluşturun

Azure Kurtarma Hizmetleri kasası için yedekleme ilkesi oluşturma [adımları, ilke REST API belgesinde](/rest/api/backup/protectionpolicies/createorupdate)özetlenmiştir. Bu belgeyi Azure VM yedeklemesi için bir ilke oluşturmak için başvuru olarak kullanalım.

## <a name="create-or-update-a-policy"></a>İlke oluşturma veya güncelleştirme

Bir Azure Yedekleme ilkesi oluşturmak veya güncelleştirmek için aşağıdaki *PUT* işlemini kullanın

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupPolicies/{policyName}?api-version=2019-05-13
```

ve `{policyName}` `{vaultName}` URI sağlanmaktadır. İstek gövdesinde ek bilgiler sağlanır.

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Örneğin, Azure VM yedeklemesi için bir ilke oluşturmak için istek gövdesinin bileşenleri şunlardır.

|Adı  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|properties     |   True      |  ProtectionPolicy:[AzureIaaSVMProtectionPolicy](/rest/api/backup/protectionpolicies/createorupdate#azureiaasvmprotectionpolicy)      | ProtectionPolicyResource özellikleri        |
|etiketler     |         | Nesne        |  Kaynak etiketleri       |

İstek gövdesindeki tanımların tam listesi [için, yedekleme ilkesi REST API belgesine](/rest/api/backup/protectionpolicies/createorupdate)bakın.

### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi Azure VM yedeklemeleri için bir yedekleme ilkesi tanımlar.

Politika diyor ki:

- Pasifik Standart Saati ile her Pazartesi, Çarşamba, Perşembe saat 10:00'da haftalık destek alın.
- Her Pazartesi, Çarşamba, Perşembe günü alınan yedekleri bir hafta boyunca saklar.
- Her ilk Çarşamba ve üçüncü Perşembe günü alınan yedeklemeleri iki ay boyunca iki ay boyunca saklar (varsa önceki bekletme koşullarını geçersiz kılar).
- Dördüncü Pazartesi ve dördüncü Perşembe günü Şubat ve Kasım aylarında dört yıl boyunca alınan yedekleri koruyun (varsa önceki bekletme koşullarını geçersiz kılar).

```json
{
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "timeZone": "Pacific Standard Time",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ]
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    }
  }
}
```

> [!IMPORTANT]
> Zamanlama ve bekletme için zaman biçimleri yalnızca DateTime'ı destekler. Tek başlarına Zaman biçimini desteklemez.

## <a name="responses"></a>Yanıtlar

Yedekleme ilkesi oluşturma/güncelleştirme, eşzamanlı bir [işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |    [Koruma PolitikasıKaynak](/rest/api/backup/protectionpolicies/createorupdate#protectionpolicyresource)     |  Tamam       |
|202 Kabul Edildi     |         |     Accepted    |

### <a name="example-responses"></a>Örnek yanıtlar

*İlke* oluşturma veya güncelleştirme için PUT isteğini gönderdikten sonra, ilk yanıt 202 (Kabul edilir) ve bir konum üstbilgisi veya Azure-async-üstbilgidir.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operations/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Ardından, basit bir *GET* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2019-05-13
```

İşlem tamamlandıktan sonra, yanıt gövdesindeki ilke içeriğiyle birlikte 200 (Tamam) döndürür.

```json
{
  "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1",
  "name": "testPolicy1",
  "type": "Microsoft.RecoveryServices/vaults/backupPolicies",
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ],
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleWeeklyFrequency": 0
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    },
    "timeZone": "Pacific Standard Time",
    "protectedItemsCount": 0
  }
}
```

Bir öğeyi korumak için zaten bir ilke kullanılıyorsa, ilkedeki herhangi bir güncelleştirme, ilişkili tüm öğeler için [korumanın değiştirilmesine](backup-azure-arm-userestapi-backupazurevms.md#changing-the-policy-of-protection) neden olur.

## <a name="next-steps"></a>Sonraki adımlar

[Korumasız bir Azure VM için korumayı etkinleştirin.](backup-azure-arm-userestapi-backupazurevms.md)

Azure Yedekleme REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Kurtarma Hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
