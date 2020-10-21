---
title: Kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme
description: Kaynak envanterini, kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 869bfcb87aa4846674db233c4268e9269929cd04
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92320173"
---
# <a name="upload-usage-data-metrics-and-logs-to-azure-monitor"></a>Kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme

Düzenli olarak, faturalama amaçları, ölçümleri izlemek ve günlükleri Azure 'a yüklemek için kullanım bilgilerini dışarı aktarabilirsiniz.  Bu üç türden verilerin herhangi birini dışarı ve karşıya yükleme işlemi, Azure 'daki veri denetleyicisi, SQL yönetilen örnek ve PostgreSQL hiper ölçek sunucu grubu kaynaklarını da oluşturur ve güncelleştirir.

> [!NOTE] 
> Önizleme dönemi boyunca, Azure Arc etkin veri hizmetlerini kullanma maliyeti yoktur.

## <a name="prerequisites"></a>Ön koşullar

Azure CLı (az) ve yüklü olması gerekir [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] .  [Araçları yükler](./install-client-tools.md).

Verileri Azure 'a yüklemeden önce, Azure aboneliğinizin kayıtlı Microsoft. AzureData kaynak sağlayıcısına sahip olduğundan emin olmanız gerekir.

Bunu, aşağıdaki komutu çalıştırarak doğrulayabilirsiniz:

```console
az provider show -n Microsoft.AzureData -o table
```

Kaynak sağlayıcısı şu anda aboneliğinizde kayıtlı değilse, aşağıdaki komutu çalıştırarak kaydı yapabilirsiniz.  Bu komutun tamamlanması için bir dakika veya iki işlem yapın.

```console
az provider register -n Microsoft.AzureData --wait
```

## <a name="upload-usage-data"></a>Kullanım verilerini karşıya yükleme

Envanter ve kaynak kullanımı gibi kullanım bilgileri aşağıdaki iki adımlı şekilde Azure 'a yüklenebilir:

1. Aşağıdaki gibi kullanım verilerini komutunu kullanarak dışarı aktarın `azdata export` :

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the export command
   azdata arc dc export --type usage --path usage.json
   ```
   Bu komut `usage.json` , veri denetleyicisinde oluşturulan SQL yönetilen örnekler ve PostgreSQL hiper ölçek örnekleri vb. gibi tüm Azure Arc etkin veri kaynaklarını içeren bir dosya oluşturur.

2. Komutu kullanarak kullanım verilerini karşıya yükleme `azdata upload`

   > [!NOTE]
   > Karşıya yüklemeyi çalıştırmadan önce lütfen Azure Arc veri denetleyicisini oluşturduktan sonra en az 24 saat bekleyin

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the upload command
   azdata arc dc upload --path usage.json
   ```

## <a name="upload-metrics-and-logs"></a>Ölçümleri ve günlükleri karşıya yükleme

Azure Arc veri Hizmetleri sayesinde ölçümleri ve günlükleri Azure Izleyici 'ye yükleyerek ölçümleri, günlükleri toplayabilir ve analiz edebilir, uyarılar oluşturabilir, bildirimler gönderebilir veya otomatik eylemleri tetikleyebilirsiniz. 

Verilerinizi Azure Izleyici 'ye gönderdiğinizde, izleme ve günlük verilerini yerinde depolar ve gelişmiş analizler için verilerin uzun süreli depolanmasını olanaklı kılarak çok büyük ölçekli verileri saklayabilirsiniz.

Azure Arc veri Hizmetleri 'ne sahip birden fazla siteniz varsa, Azure Izleyici 'yi siteler genelinde tüm günlüklerinizi ve ölçümlerinizi toplamak için merkezi bir konum olarak kullanabilirsiniz.

### <a name="before-you-begin"></a>Başlamadan önce

Günlükleri ve ölçümleri karşıya yükleme senaryolarını etkinleştirmek için birkaç tek seferlik kurulum adımı gereklidir:

1. İstemci erişim parolası oluşturma ve hizmet sorumlusunu veritabanı örnek kaynaklarınızın bulunduğu abonelikler üzerinde ' Izleme ölçümleri yayımcısı ' rolüne atama dahil olmak üzere bir hizmet sorumlusu/Azure Active Directory uygulaması oluşturun.
2. Bir Log Analytics çalışma alanı oluşturun ve anahtarları alın ve ortam değişkenlerinde bilgileri ayarlayın.

İlk öğe, ölçümleri karşıya yüklemek için gereklidir ve günlüğü karşıya yüklemek için ikinci bir değer gereklidir.

Ölçüm karşıya yükleme hizmeti sorumlusunu oluşturmak için bu komutları izleyin ve hizmet sorumlusu ölçümleri karşıya yükleyebilir ve oluşturma ve karşıya yükleme işlemleri gerçekleştirebilmeleri için ' Izleme ölçümleri yayımcısı ' ve ' katkıda bulunan ' rollerine atayın.

## <a name="create-service-principal-and-assign-roles"></a>Hizmet sorumlusu oluşturma ve rol atama

Ölçümlerinizi karşıya yükleme hizmet sorumlusunu oluşturmak ve ' Izleme ölçümleri yayımcısı ' rolüne atamak için şu komutları izleyin:

Hizmet sorumlusu oluşturmak için şu komutu çalıştırın:

> [!NOTE]
> Hizmet sorumlusu oluşturmak için [Azure 'da belirli izinler](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)gerekir.

```console
az ad sp create-for-rbac --name <a name you choose>

#Example:
#az ad sp create-for-rbac --name azure-arc-metrics
```

Örnek çıktı:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

AppID ve kiracı değerlerini daha sonra kullanmak üzere bir ortam değişkenine kaydedin. 

AppID ve kiracı değerlerini PowerShell ile kaydetmek için şu örneği izleyin:

```powershell
$Env:SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'
```

Alternatif olarak, Linux veya macOS üzerinde, AppID ve kiracı değerlerini şu örnekle kaydedebilirsiniz:

   ```console
   export SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

   #Example (using Linux):
   export SPN_CLIENT_ID='2e72adbf-de57-4c25-b90d-2f73f126e123'
   export SPN_CLIENT_SECRET='5039d676-23f9-416c-9534-3bd6afc78123'
   export SPN_TENANT_ID='72f988bf-85f1-41af-91ab-2d7cd01ad1234'
   ```

Hizmet sorumlusunu, veritabanı örneği kaynaklarınızın bulunduğu abonelikte bulunan ' Izleme ölçümleri yayımcısı ' rolüne atamak için bu komutu çalıştırın:


> [!NOTE]
> Windows ortamından çalışırken rol adları için çift tırnak kullanmanız gerekir.


```console
az role assignment create --assignee <appId value from output above> --role "Monitoring Metrics Publisher" --scope subscriptions/<sub ID>
az role assignment create --assignee <appId value from output above> --role 'Contributor' --scope subscriptions/<sub ID>

#Example:
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Contributor' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123

#On Windows environment
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Contributor" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
```

Örnek çıktı:

```console
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Sonra, bir Log Analytics çalışma alanı oluşturmak ve erişim bilgilerini ortam değişkenlerine ayarlamak için bu komutları yürütün.

> [!NOTE]
> Zaten bir çalışma alanınız varsa bu adımı atlayın.

```console
az monitor log-analytics workspace create --resource-group <resource group name> --workspace-name <some name you choose>

#Example:
#az monitor log-analytics workspace create --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
```

Örnek çıktı:

```output
{
  "customerId": "d6abb435-2626-4df1-b887-445fe44a4123",
  "eTag": null,
  "id": "/subscriptions/<Subscription ID>/resourcegroups/user-arc-demo/providers/microsoft.operationalinsights/workspaces/user-logworkspace",
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
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
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

Azure Arc etkin SQL yönetilen örnekleriniz ve Azure Arc etkin PostgreSQL hiper ölçek sunucu grupları için ölçümleri karşıya yüklemek için aşağıdaki CLı komutlarını çalıştırın:

1. Tüm ölçümleri belirtilen dosyaya aktar:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the metrics
   azdata arc dc export --type metrics --path metrics.json
   ```

2. Ölçümleri Azure izleyici 'ye yükleyin:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #upload the metrics
   azdata arc dc upload --path metrics.json
   ```

   >[!NOTE]
   >İlk karşıya yükleme için Azure Arc etkin veri örnekleri oluşturulduktan en az 30 dakika bekleyin
   >
   >`upload` `export` Azure izleyici, yalnızca son 30 dakikalık ölçümleri kabul ettiğinden, ölçümlerin doğru bir şekilde ayrıltığından emin olun. [Daha fazla bilgi edinin](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)


Dışarı aktarma sırasında "ölçümleri almak için hata" belirten bir hata görürseniz, aşağıdaki komutu çalıştırarak veri toplamanın olarak ayarlanmış olup olmadığını denetleyin ```true``` :

```console
azdata arc dc config show
```

ve "güvenlik bölümü" nün altına bakın

```output
 "security": {
      "allowDumps": true,
      "allowNodeMetricsCollection": true,
      "allowPodMetricsCollection": true,
      "allowRunAsRoot": false
    },
```

`allowNodeMetricsCollection`Ve `allowPodMetricsCollection` özelliklerinin olarak ayarlandığını doğrulayın `true` .

## <a name="view-the-metrics-in-the-portal"></a>Ölçümleri portalda görüntüleme

Ölçümleriniz karşıya yüklendikten sonra Azure portal görüntüleyebilirsiniz.
> [!NOTE]
> Portalda ölçümleri görüntüleyebilmeniz için karşıya yüklenen verilerin işlenmesi birkaç dakika sürebileceğini lütfen unutmayın.


Portalda ölçümlerinizi görüntülemek için bu bağlantıyı kullanarak portalı açın: <https://portal.azure.com> sonra, arama çubuğunda veritabanı örneğinizi adıyla arayın:

Genel Bakış sayfasında CPU kullanımı ' nı görüntüleyebilir veya daha ayrıntılı ölçümler istiyorsanız sol Gezinti panelinden ölçümler ' e tıklayabilirsiniz.

Ölçüm ad alanı olarak SQL Server 'ı seçin:

Görselleştirmek istediğiniz ölçümü seçin (birden çok öğesini de seçebilirsiniz):

Sıklığı son 30 dakika olarak değiştirin:

> [!NOTE]
> Yalnızca son 30 dakikalık ölçümleri karşıya yükleyebilirsiniz. Azure Izleyici, 30 dakikadan daha eski ölçümleri reddeder.

## <a name="upload-logs-to-azure-monitor"></a>Günlükleri Azure İzleyici'ye yükleyin

 Azure Arc etkin SQL yönetilen örneklerinizin ve AzureArc etkin PostgreSQL hiper ölçek sunucu gruplarının günlüklerini karşıya yüklemek için aşağıdaki CLı komutlarını çalıştırın.

1. Tüm günlükleri belirtilen dosyaya aktar:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the logs
   azdata arc dc export --type logs --path logs.json
   ```

2. Günlükleri Azure izleyici Log Analytics çalışma alanına yükleme:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #Upload the logs
   azdata arc dc upload --path logs.json
   ```

## <a name="view-your-logs-in-azure-portal"></a>Günlüklerinizi Azure portal görüntüleyin

Günlükleriniz karşıya yüklendikten sonra aşağıda gösterildiği gibi günlük sorgusu gezginini kullanarak sorgulayabilmeniz gerekir:

1. Azure portal açın ve ardından çalışma alanınızı üstteki arama çubuğunda ada göre arayın ve ardından seçin
2. Soldaki panelde Günlükler'e tıklayın
3. Kullanmaya başlayın ' a tıklayın (veya yeni başladıysanız Log Analytics hakkında daha fazla bilgi edinmek için Başlarken sayfasında bağlantıları tıklatın)
4. İlk kez kullanıyorsanız Log Analytics hakkında daha fazla bilgi edinmek için öğreticiyi izleyin Log Analytics
5. Tablo listesinin en altında bulunan Özel Günlükler'i genişlettiğinizde "sql_instance_logs_CL" adlı bir tablo göreceksiniz.
6. Tablo adının yanındaki göz simgesine tıklayın
7. "Sorgu düzenleyicisinde görüntüle" düğmesine tıklayın
8. Artık sorgu düzenleyicisinde, günlükteki en son 10 olayı gösteren bir sorgunuz olacak
9. Buradan sorgu düzenleyicisini kullanarak günlükleri sorgulayabilir, uyarılar ayarlayabilir ve daha birçok deneme yapabilirsiniz.

## <a name="automating-uploads-optional"></a>Karşıya Yüklemeleri Otomatikleştirme (isteğe bağlı)

Ölçümleri ve günlükleri Zamanlanmış olarak yüklemek isterseniz, bir komut dosyası oluşturup birkaç dakikada bir Zamanlayıcı üzerinde çalıştırabilirsiniz. Bir Linux kabuğu betiği kullanarak karşıya yüklemeyi otomatikleştirme örneği aşağıda verilmiştir.

En sevdiğiniz metin/kod düzenleyicide, dosyaya aşağıdaki betiği ekleyin ve. sh (Linux/Mac) veya. cmd,. bat,. ps1 gibi bir betik yürütülebilir dosyası olarak kaydedin.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Betik dosyasını çalıştırılabilir hale getirme

```console
chmod +x myuploadscript.sh
```

Betiği her 20 dakikada bir çalıştırın:

```console
watch -n 1200 ./myuploadscript.sh
```

Ayrıca, cron veya Windows Görev Zamanlayıcı gibi bir iş zamanlayıcısını ya da ansız, Pupevcil hayvan veya Chef gibi bir Orchestrator 'ı kullanabilirsiniz.

## <a name="general-guidance-on-exporting-and-uploading-usage-metrics"></a>Kullanım, ölçüm verme ve karşıya yükleme hakkında genel yönergeler

Azure Arc etkin veri Hizmetleri üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri faturalandırma ve izleme amacıyla günlüğe kaydedilir. Bu CRUD işlemlerini izleyen ve tüketimi uygun şekilde hesaplayan arka plan hizmetleri mevcuttur. Kullanım veya tüketimin gerçek hesaplaması, zamanlanan bir şekilde gerçekleşir ve arka planda yapılır. 

Önizleme süresince bu işlem gecelik olur. Genel rehberlik, kullanımı günde yalnızca bir kez karşıya yüklemek içindir. Kullanım bilgileri aktarıldığında ve aynı 24 saatlik süre içinde birden çok kez karşıya yüklendiğinde, yalnızca kaynak envanteri Azure portal ' de güncelleştirilir ancak kaynak kullanımı değildir.

Azure izleyici, ölçümleri karşıya yüklemek için yalnızca son 30 dakikalık verileri kabul eder ([daha fazla bilgi edinin](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)). Ölçüm Yükleme Kılavuzu, verileri dışa aktarma dosyası oluşturulduktan hemen sonra, Azure portal ' de tüm veri kümesini görüntüleyebilmeniz için, ölçümleri karşıya yüklemedir. Örneğin, ölçümleri 2:00 PM tarihinde ve 2:50 PM 'de karşıya yükle komutunu çalıştırdıysanız. Azure Izleyici yalnızca son 30 dakikalık verileri kabul ettiğinden portalda herhangi bir veri göremeyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin](view-billing-data-in-azure.md)

[Azure Arc Data Controller kaynağını Azure portal görüntüle](view-data-controller-in-azure-portal.md)