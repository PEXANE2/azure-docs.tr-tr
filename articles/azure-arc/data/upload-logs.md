---
title: Günlükleri Azure İzleyici'ye yükleyin
description: Azure Arc etkin veri Hizmetleri için günlükleri Azure Izleyici 'ye yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
zone_pivot_groups: client-operating-system-macos-and-linux-windows-powershell
ms.openlocfilehash: f3f29ae1ab868a96e6f70ed964f79c47bc591c4d
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92378143"
---
# <a name="upload-logs-to-azure-monitor"></a>Günlükleri Azure İzleyici'ye yükleyin

Düzenli olarak, günlükleri dışa aktarabilir ve ardından Azure 'a yükleyebilirsiniz. Günlükleri dışarı ve karşıya yüklemek, Azure 'da veri denetleyicisi, SQL yönetilen örnek ve PostgreSQL hiper ölçek sunucu grubu kaynaklarını oluşturur ve güncelleştirir.

> [!NOTE] 
> Önizleme dönemi boyunca, Azure Arc etkin veri hizmetlerini kullanma maliyeti yoktur.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Günlükleri karşıya yüklemeden önce şunları yapmanız gerekir: 

1. [Log Analytics çalışma alanı oluşturma](#create-a-log-analytics-workspace)
1. [Ortam değişkenlerine KIMLIK ve paylaşılan anahtar ata](#assign-id-and-shared-key-to-environment-variables)

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Bir Log Analytics çalışma alanı oluşturmak için, bir Log Analytics çalışma alanı oluşturmak ve erişim bilgilerini ortam değişkenlerine ayarlamak için bu komutları yürütün.

> [!NOTE]
> Zaten bir çalışma alanınız varsa bu adımı atlayın.

```azurecli
az monitor log-analytics workspace create --resource-group <resource group name> --workspace-name <some name you choose>
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

Günlük çalışma alanı analizlerini `customerId` daha sonra kullanılmak üzere bir ortam değişkeni olarak kaydedin:

::: zone pivot="client-operating-system-windows-command"

```console
SET WORKSPACE_ID=<customerId>
```

::: zone-end

::: zone pivot="client-operating-system-powershell"

```PowerShell
$Env:WORKSPACE_ID='<customerId>'
```
::: zone-end

::: zone pivot="client-operating-system-macos-and-linux"

```console
export WORKSPACE_ID='<customerId>'
```
::: zone-end

Bu komut, Log Analytics çalışma alanınıza bağlanmak için gereken erişim anahtarlarını döndürür:

```azurecli
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
```

Örnek çıktı:

```output
{
  "primarySharedKey": "JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123==",
  "secondarySharedKey": "p2XHSxLJ4o9IAqm2zINcEmx0UWU5Z5EZz8PQC0OHpFjdpuVaI0zsPbTv5VyPFgaCUlCZb2yEbkiR4eTuTSF123=="
}
```

Birincil anahtarı daha sonra kullanılacak bir ortam değişkenine kaydedin:

::: zone pivot="client-operating-system-windows-command"

```console
SET WORKSPACE_SHARED_KEY=<primarySharedKey>
```

::: zone-end

::: zone pivot="client-operating-system-powershell"

```console
$Env:WORKSPACE_SHARED_KEY='<primarySharedKey>'
```
```
::: zone-end


::: zone pivot="client-operating-system-macos-and-linux"

```console
export WORKSPACE_SHARED_KEY='<primarySharedKey>'
```

::: zone-end

## <a name="set-final-environment-variables-and-confirm"></a>Son ortam değişkenlerini ayarlama ve onaylama

Bir ortam değişkeninde SPN yetkilisi URL 'sini ayarlayın:

::: zone pivot="client-operating-system-windows-command"

```console
SET SPN_AUTHORITY=https://login.microsoftonline.com
```

::: zone-end

::: zone pivot="client-operating-system-powershell"

```console
$Env:SPN_AUTHORITY='https://login.microsoftonline.com'
```

::: zone-end

::: zone pivot="client-operating-system-macos-and-linux"

```console
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

::: zone-end


## <a name="verify-environment-variables"></a>Ortam değişkenlerini doğrulama

İsterseniz gerekli tüm ortam değişkenlerinin ayarlandığından emin olmak için işaretleyin:


::: zone pivot="client-operating-system-windows-command"

```console
echo %WORKSPACE_ID%
echo %WORKSPACE_SHARED_KEY%
echo %SPN_TENANT_ID%
echo %SPN_CLIENT_ID%
echo %SPN_CLIENT_SECRET%
echo %SPN_AUTHORITY%
```

::: zone-end

::: zone pivot="client-operating-system-powershell"

```PowerShell
$Env:WORKSPACE_ID
$Env:WORKSPACE_SHARED_KEY
$Env:SPN_TENANT_ID
$Env:SPN_CLIENT_ID
$Env:SPN_CLIENT_SECRET
$Env:SPN_AUTHORITY
```


::: zone-end

::: zone pivot="client-operating-system-macos-and-linux"

```console
echo $WORKSPACE_ID
echo $WORKSPACE_SHARED_KEY
echo $SPN_TENANT_ID
echo $SPN_CLIENT_ID
echo $SPN_CLIENT_SECRET
echo $SPN_AUTHORITY
```

::: zone-end

Ortam değişkenleri ayarlanmış olarak günlükleri günlük çalışma alanına yükleyebilirsiniz. 

## <a name="upload-logs-to-azure-monitor"></a>Günlükleri Azure İzleyici'ye yükleyin

 Azure Arc etkin SQL yönetilen örneklerinizin ve AzureArc etkin PostgreSQL hiper ölçek sunucu gruplarının günlüklerini karşıya yüklemek için aşağıdaki CLı komutlarını çalıştırın.

1. İle Azure Arc veri denetleyicisinde oturum açın [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] .

   ```console
   azdata login
   ```

   Ad alanını, yönetici kullanıcı adını ve parolayı ayarlamak için istemleri izleyin. 

1. Tüm günlükleri belirtilen dosyaya aktar:

   ```console
   azdata arc dc export --type logs --path logs.json
   ```

2. Günlükleri Azure izleyici Log Analytics çalışma alanına yükleme:

   ```console
   azdata arc dc upload --path logs.json
   ```

## <a name="view-your-logs-in-azure-portal"></a>Günlüklerinizi Azure portal görüntüleyin

Günlükleriniz karşıya yüklendikten sonra aşağıda gösterildiği gibi günlük sorgusu gezginini kullanarak sorgulayabilmeniz gerekir:

1. Azure portal açın ve ardından çalışma alanınızı üstteki arama çubuğunda ada göre arayın ve ardından seçin.
2. Sol panelde Günlükler ' i seçin.
3. Kullanmaya başlayın ' ı seçin (veya yeni başladıysanız Log Analytics hakkında daha fazla bilgi edinmek için Başlarken sayfasında bağlantıları seçin).
4. Log Analytics ilk kez Log Analytics hakkında daha fazla bilgi edinmek için öğreticiyi izleyin.
5. Tablo listesinin en altında bulunan Özel Günlükler'i genişlettiğinizde "sql_instance_logs_CL" adlı bir tablo göreceksiniz.
6. Tablo adının yanındaki ' göz ' simgesini seçin.
7. ' Sorgu Düzenleyicisi 'nde görüntüle ' düğmesini seçin.
8. Artık sorgu Düzenleyicisi 'nde, günlükteki en son 10 olayı gösterecek bir sorgunuz olacak.
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

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümleri ve günlükleri Azure Izleyici 'ye yükleme](upload-metrics.md)

[Kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme](upload-usage-data.md)

[Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin](view-billing-data-in-azure.md)

[Azure Arc Data Controller kaynağını Azure portal görüntüle](view-data-controller-in-azure-portal.md)
