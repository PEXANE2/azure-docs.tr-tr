---
title: Depolama hesabı için gerekli minimum Aktarım Katmanı Güvenliği (TLS) sürümünü yapılandırın
titleSuffix: Azure Storage
description: Azure depolama 'ya karşı istek yapan istemciler için Aktarım Katmanı Güvenliği (TLS) için en düşük sürümü gerektirecek bir depolama hesabı yapılandırın.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/08/2020
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: ab83f0ee656dfc717284c1e26d10dcb814fe1c9e
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209861"
---
# <a name="configure-minimum-required-version-of-transport-layer-security-tls-for-a-storage-account"></a>Depolama hesabı için gerekli minimum Aktarım Katmanı Güvenliği (TLS) sürümünü yapılandırın

İstemci uygulaması ve bir Azure depolama hesabı arasındaki iletişim, Aktarım Katmanı Güvenliği (TLS) kullanılarak şifrelenir. TLS, Internet üzerinden istemciler ve hizmetler arasında gizlilik ve veri bütünlüğünü sağlayan standart bir şifreleme protokolüdür. TLS hakkında daha fazla bilgi için bkz. [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security).

Azure Storage Şu anda TLS protokolünün üç sürümünü desteklemektedir: 1,0, 1,1 ve 1,2. TLS 1,2, en güvenli TLS sürümüdür. Azure depolama, genel HTTPs uç noktalarında TLS 1,2 kullanır, ancak geriye dönük uyumluluk için TLS 1,0 ve TLS 1,1 desteklenmeye devam etmektedir.

Varsayılan olarak, Azure depolama hesapları istemcilerin en eski TLS, TLS 1,0 ve üzeri sürümü ile veri göndermesini ve almasına izin verir. Daha sıkı güvenlik önlemleri zorlamak için depolama hesabınızı, istemcilerin daha yeni bir TLS sürümüyle veri göndermesini ve almasını gerektirecek şekilde yapılandırabilirsiniz. Depolama hesabı en az TLS sürümü gerektiriyorsa, önceki bir sürümle yapılan tüm istekler başarısız olur.

Bu makalede, istemcilerin en düşük TLS sürümüne sahip istekleri göndermesini gerektirecek şekilde bir depolama hesabının nasıl yapılandırılacağı açıklanır. Bir istemci uygulamasından istek gönderirken belirli bir TLS sürümünü belirtme hakkında bilgi için, bkz. [bir istemci uygulaması Için Aktarım Katmanı Güvenliği (TLS) yapılandırma](transport-layer-security-configure-client-version.md).

## <a name="detect-the-tls-version-used-by-client-applications"></a>İstemci uygulamaları tarafından kullanılan TLS sürümünü Algıla

Depolama hesabınız için en düşük TLS sürümünü zorlayarak, TLS 'nin önceki bir sürümüyle veri gönderen istemcilerden gelen istekleri reddetmeniz önerilir. En düşük TLS sürümünü yapılandırmanın istemci uygulamalarını nasıl etkileyebileceğini anlamak için, Microsoft, Azure depolama hesabınız için günlük kaydını etkinleştirmenizi ve TLS istemci uygulamalarının hangi sürümlerinin kullandığını belirlemek için günlükleri bir zaman aralığından sonra analiz etmenize olanak önerir.

Azure depolama hesabınıza yönelik istekleri günlüğe kaydetmek ve istemci tarafından kullanılan TLS sürümünü öğrenmek için Azure Izleyici 'de (Önizleme) Azure depolama günlüğü 'nü kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Storage 'ı izleme](monitor-storage.md).

Azure Izleyici 'de Azure depolama günlüğü, günlük verilerini çözümlemek için günlük sorgularının kullanılmasını destekler. Günlükleri sorgulamak için bir Azure Log Analytics çalışma alanı kullanabilirsiniz. Günlük sorguları hakkında daha fazla bilgi edinmek için bkz. [öğretici: Log Analytics sorguları kullanmaya başlama](../../azure-monitor/log-query/get-started-portal.md).

Azure depolama verilerini Azure Izleyici ile günlüğe kaydetmek ve Azure Log Analytics ile analiz etmek için, önce hangi istek türlerinin ve hangi depolama hizmetlerini günlüğe kaydetmek istediğinizi belirten bir tanılama ayarı oluşturmanız gerekir. Azure portal bir tanılama ayarı oluşturmak için aşağıdaki adımları izleyin:

1. Azure [izleyici önizlemesinde Azure Storage günlük](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)kaydına kaydolun.
1. Azure depolama hesabınızı içeren abonelikte yeni bir Log Analytics çalışma alanı oluşturun. Depolama hesabınız için günlüğe kaydetmeyi yapılandırdıktan sonra, Günlükler Log Analytics çalışma alanında kullanılabilir. Daha fazla bilgi için [Azure portal Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md)bölümüne bakın.
1. Azure portalda depolama hesabınıza gidin.
1. Izleme bölümünde **Tanılama ayarları (Önizleme)** öğesini seçin.
1. İsteklerini günlüğe kaydetmek istediğiniz Azure Storage hizmetini seçin. Örneğin, istekleri blob depolamaya kaydetmek için **BLOB** ' u seçin.
1. **Tanılama ayarı Ekle**' yi seçin.
1. Tanılama ayarı için bir ad girin.
1. **Kategori ayrıntıları**' nın altında, **günlük** bölümünde günlüğe kaydedilecek istek türlerini seçin. Okuma, yazma ve silme isteklerini günlüğe kaydedebilirsiniz. Örneğin, **Storageread** ve **storagewrite** seçildiğinde, okuma ve yazma istekleri seçili hizmete kaydedilir.
1. **Hedef ayrıntıları**altında **Log Analytics gönder**' i seçin. Aşağıdaki görüntüde gösterildiği gibi aboneliğinizi ve daha önce oluşturduğunuz Log Analytics çalışma alanını seçin.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/create-diagnostic-setting-logs.png" alt-text="İstekleri günlüğe kaydetmek için bir tanılama ayarı oluşturmayı gösteren ekran görüntüsü":::

Tanılama ayarını oluşturduktan sonra depolama hesabına yönelik istekler daha sonra bu ayara göre günlüğe kaydedilir. Daha fazla bilgi için bkz. [Azure 'da kaynak günlüklerini ve ölçümleri toplamak için tanılama ayarı oluşturma](../../azure-monitor/platform/diagnostic-settings.md).

Azure Izleyici 'de Azure depolama günlüklerinde bulunan alanların bir başvurusu için bkz. [kaynak günlükleri (Önizleme)](monitor-storage-reference.md#resource-logs-preview).

### <a name="query-logged-requests-by-tls-version"></a>Günlüğe kaydedilen istekleri TLS sürümüne göre sorgula

Azure Izleyici 'de Azure depolama günlükleri bir depolama hesabına istek göndermek için kullanılan TLS sürümünü içerir. Günlüğe kaydedilen bir isteğin TLS sürümünü denetlemek için **TlsVersion** özelliğini kullanın.

Son 7 güne ait günlükleri almak ve her TLS sürümü ile blob depolamaya göre kaç istek yapıldığını öğrenmek için Log Analytics çalışma alanınızı açın. Sonra, aşağıdaki sorguyu yeni bir günlük sorgusuna yapıştırın ve çalıştırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>"
| summarize count() by TlsVersion
```

Sonuçlar, her TLS sürümünde yapılan isteklerin sayısını gösterir:

:::image type="content" source="media/transport-layer-security-configure-minimum-version/log-analytics-query-version.png" alt-text="TLS sürümünü döndürmek için Log Analytics sorgusunun sonuçlarını gösteren ekran görüntüsü":::

### <a name="query-logged-requests-by-caller-ip-address-and-user-agent-header"></a>Çağıran IP adresine ve Kullanıcı Aracısı üstbilgisine göre günlüğe kaydedilmiş istekleri sorgula

Azure Izleyici 'de Azure depolama günlükleri, depolama hesabına hangi istemci uygulamalarının eriştiğini değerlendirmenize yardımcı olmak üzere arayan IP adresini ve Kullanıcı Aracısı üst bilgisini de içerir. Bu değerleri, istemci uygulamalarının daha yeni bir TLS sürümünü kullanacak şekilde güncelleştirilip güncelleştirilmediğini veya en düşük TLS sürümü ile gönderilmemişse istemcinin isteği başarısız olarak kabul edilip edilmeyeceğini belirlemek için çözümleyebilirsiniz.

Son 7 güne ait günlükleri almak ve TLS 1,2 ' den önceki bir TLS sürümü ile istek yaptığını anlamak için, aşağıdaki sorguyu yeni bir günlük sorgusuna yapıştırın ve çalıştırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>" and TlsVersion != "TLS 1.2"
| project TlsVersion, CallerIpAddress, UserAgentHeader
```

## <a name="configure-the-minimum-tls-version-for-an-account"></a>Bir hesabın en düşük TLS sürümünü yapılandırın

Bir depolama hesabı için en düşük TLS sürümünü yapılandırmak üzere, hesap için **Minimumtlsversion** sürümünü ayarlamak üzere Azure Portal veya Azure CLI 'yi kullanın. Bu özellik, Azure Resource Manager dağıtım modeliyle oluşturulan tüm depolama hesapları için kullanılabilir. Daha fazla bilgi için bkz. [depolama hesabına genel bakış](storage-account-overview.md).

# <a name="portal"></a>[Portal](#tab/portal)

Bir depolama hesabı için en düşük TLS sürümünü Azure portal yapılandırmak için aşağıdaki adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Yapılandırma** ayarını seçin.
1. **En düşük TLS sürümü**altında, aşağıdaki görüntüde gösterildiği gibi, bu depolama hesabındaki verilere erişmek için gereken en düşük TLS sürümünü seçmek üzere açılan listesini kullanın.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/configure-minimum-version-portal.png" alt-text="Azure portal en düşük TLS sürümünün nasıl yapılandırılacağını gösteren ekran görüntüsü":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile bir depolama hesabı için en düşük TLS sürümünü yapılandırmak üzere [az Resource Show](/cli/azure/resource#az-resource-show) komutunu çağırarak depolama HESABıNıZıN kaynak kimliğini alın. Daha sonra, depolama hesabı için **Minimumtlsversion** özelliğini ayarlamak üzere [az Resource Update](/cli/azure/resource#az-resource-update) komutunu çağırın. **Minimumtlsversion** için geçerli değerler `TLS1_0` `TLS1_1` ve `TLS1_2` .

Aşağıdaki örnek, en düşük TLS sürümünü 1,2 olarak ayarlar. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
storage_account_id=$(az resource show \
  --name <storage-account> \
  --resource-group <resource-group> \
  --resource-type Microsoft.Storage/storageAccounts \
  --query id \
  --output tsv)

az resource update \
  --ids $storage_account_id \
  --set properties.minimumTlsVersion="TLS1_2"
```

---

> [!NOTE]
> Depolama hesabı için en düşük TLS sürümünü güncelleştirdikten sonra, değişiklik tamamen yayılmadan önce 30 saniye kadar sürebilir.

## <a name="check-the-minimum-required-tls-version-for-an-account"></a>Bir hesap için gereken en düşük TLS sürümünü denetleyin

Bir depolama hesabı için yapılandırılmış gereken en düşük TLS sürümünü öğrenmek için Azure Resource Manager **Minimumtlsversion** özelliğini denetleyin. Tek seferde büyük bir depolama hesabı için bu özelliği denetlemek üzere Azure Kaynak Grafiği gezginini kullanın.

**Minimumtlsversion** özelliği varsayılan olarak ayarlanmadı ve açıkça ayarlanana kadar bir değer döndürmez. Depolama hesabı, özellik değeri null ise TLS sürüm 1,0 veya üzeri ile gönderilen isteklere izin vermek için varsayılan olarak kullanılır.

### <a name="check-the-minimum-required-tls-version-for-a-single-storage-account"></a>Tek bir depolama hesabı için gerekli en düşük TLS sürümünü denetleyin

Azure CLı kullanarak tek bir depolama hesabı için gerekli en düşük TLS sürümünü denetlemek için, **Minimumtlsversion** özelliği için [az Resource Show](/cli/azure/resource#az-resource-show) komutunu ve sorguyu çağırın:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.minimumTlsVersion \
    --output tsv
```

### <a name="check-the-minimum-required-tls-version-for-a-set-of-storage-accounts"></a>Bir depolama hesabı kümesi için gereken en düşük TLS sürümünü denetleyin

En iyi performansa sahip bir dizi depolama hesabı genelinde gereken en düşük TLS sürümünü denetlemek için Azure portal Azure Kaynak Grafiği Gezginini kullanabilirsiniz. Kaynak Grafiği Gezginini kullanma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak Ilk kaynak grafik sorgunuzu çalıştırma](/azure/governance/resource-graph/first-query-portal).

Kaynak Graph Explorer 'da aşağıdaki sorguyu çalıştırmak, depolama hesaplarının bir listesini döndürür ve her hesap için en düşük TLS sürümünü görüntüler:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend minimumTlsVersion = parse_json(properties).minimumTlsVersion
| project subscriptionId, resourceGroup, name, minimumTlsVersion
```

---

## <a name="test-the-minimum-tls-version-from-a-client"></a>Bir istemciden en düşük TLS sürümünü test etme

Önceki bir sürümle yapılan bir depolama hesabı yasaklıyor çağrıları için gereken en düşük TLS sürümünü sınamak için, bir istemciyi TLS 'nin önceki bir sürümünü kullanacak şekilde yapılandırabilirsiniz. Bir istemciyi belirli bir TLS sürümünü kullanacak şekilde yapılandırma hakkında daha fazla bilgi için, bkz. [bir istemci uygulaması Için Aktarım Katmanı Güvenliği 'ni (TLS) yapılandırma](transport-layer-security-configure-client-version.md).

Bir istemci, hesap için yapılandırılmış en düşük TLS sürümünü karşılamayan bir TLS sürümünü kullanarak bir depolama hesabına eriştiğinde, Azure Storage hata kodu 400 hatası (Hatalı Istek) ve kullanılan TLS sürümünün bu depolama hesabında istek yapmasına izin verilmediğini belirten bir ileti döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- [İstemci uygulaması için Aktarım Katmanı Güvenliği 'ni (TLS) yapılandırma](transport-layer-security-configure-client-version.md)
- [BLOB depolama için güvenlik önerileri](../blobs/security-recommendations.md)
