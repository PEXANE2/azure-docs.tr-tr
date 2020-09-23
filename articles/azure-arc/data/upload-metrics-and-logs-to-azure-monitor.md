---
title: Kaynak envanterini, kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme
description: Kaynak envanterini, kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: ac6ffd2b5bf48079db6a0cd261dbe2535e1821ac
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941555"
---
# <a name="upload-resource-inventory-usage-data-metrics-and-logs-to-azure-monitor"></a>Kaynak envanterini, kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme

Azure Arc veri Hizmetleri sayesinde ölçümleri ve günlükleri Azure Izleyici 'ye yükleyerek ölçümleri, günlükleri toplayabilir ve *analiz edebilir,* uyarılar oluşturabilir, bildirimler gönderebilir veya otomatik eylemleri tetikleyebilirsiniz. Verilerinizi Azure Izleyici 'ye gönderdiğinizde, izleme ve günlük verilerini ve verileri, gelişmiş analizler için uzun süreli depolamayı olanaklı kılarak çok büyük ölçekte depolamanıza olanak tanır.  Azure Arc veri Hizmetleri olan birden çok siteniz varsa, Azure Izleyici 'yi siteler genelinde tüm günlüklerinizi ve ölçümlerinizi toplamak için merkezi bir konum olarak kullanabilirsiniz.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Günlükleri ve ölçümleri karşıya yükleme senaryolarını etkinleştirmek için birkaç tek seferlik kurulum adımı gereklidir:

1) İstemci erişim parolası oluşturma ve hizmet sorumlusunu veritabanı örnek kaynaklarınızın bulunduğu abonelikler üzerinde ' Izleme ölçümleri yayımcısı ' rolüne atama dahil olmak üzere bir hizmet sorumlusu/Azure Active Directory uygulaması oluşturun.
2) Bir Log Analytics çalışma alanı oluşturun ve anahtarları alın ve ortam değişkenlerinde bilgileri ayarlayın.

İlk öğe, ölçümleri karşıya yüklemek için gereklidir ve günlüğü karşıya yüklemek için ikinci bir değer gereklidir.

Ölçüm karşıya yükleme hizmeti sorumlusunu oluşturmak için bu komutları izleyin ve hizmet sorumlusu ölçümleri karşıya yükleyebilir ve oluşturma ve karşıya yükleme işlemleri gerçekleştirebilmeleri için ' Izleme ölçümleri yayımcısı ' ve ' katkıda bulunan ' rollerine atayın.

## <a name="create-service-principal-and-assign-roles"></a>Hizmet sorumlusu oluşturma ve rol atama

Ölçümlerinizi karşıya yükleme hizmet sorumlusunu oluşturmak ve ' Izleme ölçümleri yayımcısı ' rolüne atamak için şu komutları izleyin:

Hizmet sorumlusu oluşturmak için şu komutu çalıştırın:

> [!NOTE]
> Hizmet sorumlusu oluşturmak için [Azure 'da belirli izinler](/active-directory/develop/howto-create-service-principal-portal#required-permissions)gerekir.

```console
az ad sp create-for-rbac --name <a name you choose>

#Example:
#az ad sp create-for-rbac --name azure-arc-metrics
```

Örnek çıktı:

```console
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

AppID ve kiracı değerlerini daha sonra kullanmak üzere bir ortam değişkenine kaydedin:

```console
#PowerShell

$Env:SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

#Linux/macOS

export SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
export SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
export SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

#Example (using Linux):
export SPN_CLIENT_ID='2e72adbf-de57-4c25-b90d-2f73f126e123'
export SPN_CLIENT_SECRET='5039d676-23f9-416c-9534-3bd6afc78123'
export SPN_TENANT_ID='72f988bf-85f1-41af-91ab-2d7cd01ad1234'
```

Hizmet sorumlusunu, veritabanı örneği kaynaklarınızın bulunduğu abonelikte bulunan ' Izleme ölçümleri yayımcısı ' rolüne atamak için bu komutu çalıştırın:

```console
az role assignment create --assignee <appId value from output above> --role 'Monitoring Metrics Publisher' --scope subscriptions/<sub ID>
az role assignment create --assignee <appId value from output above> --role 'Contributor' --scope subscriptions/<sub ID>

#Example:
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Monitoring Metrics Publisher' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Contributor' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
```

Örnek çıktı:

```console
{
  "canDelegate": null,
  "id": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Sonra, bir Log Analytics çalışma alanı oluşturmak ve erişim bilgilerini ortam değişkenlerine ayarlamak için bu komutları yürütün.

> [!NOTE]
> Zaten bir çalışma alanınız varsa bu adımı atlayın.

```console
az monitor log-analytics workspace create --resource-group <resource group name> --name <some name you choose>

#Example:
#az monitor log-analytics workspace create --resource-group MyResourceGroup --name MyLogsWorkpace
```

Örnek çıktı:

```console
{
  "customerId": "d6abb435-2626-4df1-b887-445fe44a4123",
  "eTag": null,
  "id": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/resourcegroups/user-arc-demo/providers/microsoft.operationalinsights/workspaces/user-logworkspace",
  "location": "eastus",
  "name": "user-logworkspace",
  "portalUrl": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "user-arc-demo",
  "retentionInDays": 30,
  "sku": {
    "lastSkuUpdate": "Thu, 30 Jul 2020 22:37:53 GMT",
    "maxCapacityReservationLevel": 3000,
    "name": "pergb2018"
  },
  "source": "Azure",
  "tags": null,
  "type": "Microsoft.OperationalInsights/workspaces"
}
```

## <a name="assign-id-and-shared-key-to-environment-variables"></a>Ortam değişkenlerine KIMLIK ve paylaşılan anahtar ata

CustomerID 'yi (çalışma alanı KIMLIĞI) daha sonra kullanılacak bir ortam değişkeni olarak Kaydet:

```console
#PowerShell
$Env:WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Linux/macOS
export WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Example (using Linux)
#export WORKSPACE_ID='d6abb435-2626-4df1-b887-445fe44a4123'
```

Bu komut, Log Analytics çalışma alanınıza bağlanmak için gereken erişim anahtarlarını yazdırır:

```console
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --name MyLogsWorkpace
```

Örnek çıktı:

```console
{
  "primarySharedKey": "JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123==",
  "secondarySharedKey": "p2XHSxLJ4o9IAqm2zINcEmx0UWU5Z5EZz8PQC0OHpFjdpuVaI0zsPbTv5VyPFgaCUlCZb2yEbkiR4eTuTSF123=="
}
```

Birincil anahtarı daha sonra kullanılacak bir ortam değişkenine kaydedin:

```console
#PowerShell:
$Env:WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Linux/macOS:
export WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Example (using Linux):
export WORKSPACE_SHARED_KEY='JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123=='

```

## <a name="set-final-environment-variables-and-confirm"></a>Son ortam değişkenlerini ayarlama ve onaylama

Bir ortam değişkeninde SPN yetkilisi URL 'sini ayarlayın:

```console
#PowerShell
$Env:SPN_AUTHORITY='https://login.microsoftonline.com'

#Linux/macOS:
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

İsterseniz gerekli tüm ortam değişkenlerinin ayarlandığından emin olmak için işaretleyin:

```console
#PowerShell
$Env:WORKSPACE_ID
$Env:WORKSPACE_SHARED_KEY
$Env:SPN_TENANT_ID
$Env:SPN_CLIENT_ID
$Env:SPN_CLIENT_SECRET
$Env:SPN_AUTHORITY

#Linux/macOS
echo $WORKSPACE_ID
echo $WORKSPACE_SHARED_KEY
echo $SPN_TENANT_ID
echo $SPN_CLIENT_ID
echo $SPN_CLIENT_SECRET
echo $SPN_AUTHORITY
```

## <a name="upload-metrics-to-azure-monitor"></a>Ölçümleri Azure Izleyici 'ye yükleyin

Azure SQL yönetilen örnekleriniz ve PostgreSQL için Azure veritabanı hiper ölçek sunucu grupları için ölçümleri karşıya yüklemek üzere aşağıdaki CLı komutlarını çalıştırın:

Bu komut, tüm ölçümleri belirtilen dosyaya aktarır:

```console
azdata arc dc export -t metrics --path metrics.json
```

Bu komut, ölçümleri Azure İzleyici'ye yükler:

```console
azdata arc dc upload --path metrics.json
```

## <a name="view-the-metrics-in-the-portal"></a>Ölçümleri portalda görüntüleme

Ölçümleriniz karşıya yüklendikten sonra Azure portalından görselleştirebilmeniz gerekir.

Portalda ölçümlerinizi görüntülemek için bu özel bağlantıyı kullanarak portalı açın: <https://portal.azure.com> sonra, arama çubuğunda veritabanı örneğinizi adıyla arayın:

Genel Bakış sayfasında CPU kullanımı ' nı görüntüleyebilir veya daha ayrıntılı ölçümler istiyorsanız sol Gezinti panelinden ölçümler ' e tıklayabilirsiniz.

Ölçüm ad alanı olarak SQL Server 'ı seçin:

Görselleştirmek istediğiniz ölçümü seçin (birden çok öğesini de seçebilirsiniz):

Sıklığı son 30 dakika olarak değiştirin:

> [!NOTE]
> Yalnızca son 30 dakikalık ölçümleri karşıya yükleyebilirsiniz. Azure Izleyici, 30 dakikadan daha eski ölçümleri reddeder.

## <a name="upload-logs-to-azure-monitor"></a>Günlükleri Azure İzleyici'ye yükleyin

 Azure SQL yönetilen örneklerinizin ve PostgreSQL için Azure veritabanı hiper ölçek sunucu gruplarının günlüklerini karşıya yüklemek için aşağıdaki CLı komutlarını çalıştırın.

Bu komut, tüm günlükleri belirtilen dosyaya aktarır:

```console
azdata arc dc export -t logs --path logs.json
```

Bu işlem günlükleri bir Azure izleyici Log Analytics çalışma alanına yükler:

```console
azdata arc dc upload --path logs.json
```

## <a name="view-your-logs-in-azure-portal"></a>Günlüklerinizi Azure portal görüntüleyin

Günlükleriniz karşıya yüklendikten sonra aşağıda gösterildiği gibi günlük sorgusu gezginini kullanarak sorgulayabilmeniz gerekir:

1. Azure portal açın ve ardından çalışma alanınızı üstteki arama çubuğunda ada göre arayın ve ardından seçin
2. Soldaki panelde Günlükler'e tıklayın
3. Kullanmaya başlayın ' a tıklayın (veya yeni başladıysanız Log Analytics hakkında daha fazla bilgi edinmek için Başlarken sayfasında bağlantıları tıklatın)
4. İlk kez kullanıyorsanız Log Analytics hakkında daha fazla bilgi edinmek için öğreticiyi izleyin
5. Tablo listesinin en altında bulunan Özel Günlükler'i genişlettiğinizde "sql_instance_logs_CL" adlı bir tablo göreceksiniz.
6. Tablo adının yanındaki göz simgesine tıklayın
7. "Sorgu düzenleyicisinde görüntüle" düğmesine tıklayın
8. Günlükteki en yeni 10 etkinliği gösterecek olan sorgu, sorgu düzenleyicisinde açılır
9. Buradan sorgu düzenleyicisini kullanarak günlükleri sorgulayabilir, uyarılar ayarlayabilir ve daha birçok deneme yapabilirsiniz.

## <a name="automating-metrics-and-logs-uploads-optional"></a>Ölçümleri ve günlükleri karşıya yüklemeyi otomatikleştirme (isteğe bağlı)

Ölçümleri ve günlükleri sürekli karşıya yüklemek isterseniz, bir betik oluşturup birkaç dakikada bir Zamanlayıcı üzerinde çalıştırabilirsiniz.  Bir Linux kabuğu betiği kullanarak karşıya yüklemeyi otomatikleştirme örneği aşağıda verilmiştir.

En sevdiğiniz metin/kod düzenleyicide, komut dosyası içeriğine aşağıdakileri ekleyin ve. sh (Linux/Mac) veya. cmd,. bat,. ps1 gibi bir betik yürütülebilir dosyası olarak kaydedin.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Betik dosyasını çalıştırılabilir hale getirme

```console
chmod +x myuploadscript.sh
```

Betiği her 2 dakikada bir çalıştırın:

```console
watch -n 120 ./myuploadscript.sh
```

Ayrıca, cron veya Windows Görev Zamanlayıcı gibi bir iş zamanlayıcısını ya da ansız, Pupevcil hayvan veya Chef gibi bir Orchestrator 'ı kullanabilirsiniz.
