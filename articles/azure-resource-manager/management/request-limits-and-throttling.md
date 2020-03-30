---
title: İstek sınırları ve azaltma
description: Abonelik sınırlarına ulaşıldığında Azure Kaynak Yöneticisi istekleriyle azaltmanın nasıl kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 03/24/2020
ms.custom: seodec18
ms.openlocfilehash: 4d387749261747eb9ea1ea26629ade4fe8729856
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239356"
---
# <a name="throttling-resource-manager-requests"></a>Resource Manager isteklerini azaltma

Bu makalede, Azure Kaynak Yöneticisi'nin istekleri nasıl azalttıaçıklanmıştır. Sınıra ulaşmadan önce kalan istek sayısını nasıl izleyeceğinizi ve sınıra ulaştığınızda nasıl yanıt verilebildiğinizi gösterir.

Azaltma iki düzeyde olur. Azure Kaynak Yöneticisi, abonelik ve kiracı isteklerini daraltır. İstek, abonelik ve kiracı için azaltma sınırları altındaysa, Kaynak Yöneticisi isteği kaynak sağlayıcısına yönlendirir. Kaynak sağlayıcısı, işlemlerine uygun olarak uyarlanan azaltma sınırlarını uygular. Aşağıdaki resim, istek kullanıcıdan Azure Kaynak Yöneticisi'ne ve kaynak sağlayıcısına giderken azaltmanın nasıl uygulandığını gösterir.

![Azaltma isteği](./media/request-limits-and-throttling/request-throttling.svg)

## <a name="subscription-and-tenant-limits"></a>Abonelik ve kiracı limitleri

Her abonelik düzeyi ve kiracı düzeyindeki işlem azaltma sınırlarına tabidir. Abonelik istekleri, aboneliğinizdeki kaynak gruplarını almak gibi abonelik kimliğinizi geçirmeyi içeren istekleriiçerir. Kiracı istekleri, geçerli Azure konumlarını alma gibi abonelik kimliğinizi içermez.

Saat başına varsayılan azaltma sınırları aşağıdaki tabloda gösterilir.

| Kapsam | İşlemler | Sınır |
| ----- | ---------- | ------- |
| Abonelik | Okur | 12000 |
| Abonelik | Silme | 15000 |
| Abonelik | Yazar | 1200 |
| Kiracı | Okur | 12000 |
| Kiracı | Yazar | 1200 |

Bu sınırların kapsamı, istekleri yapan güvenlik sorumlusu ve abonelik kimliği veya kiracı kimliğine göre belirlenir. İstekleriniz birden fazla güvenlik sorumlusundan geliyorsa, abonelik veya kiracıdaki sınırınız saatte 12.000 ve 1.200’den büyüktür.

Bu sınırlar her Azure Kaynak Yöneticisi örneği için geçerlidir. Her Azure bölgesinde birden çok örnek vardır ve Azure Kaynak Yöneticisi tüm Azure bölgelerine dağıtılır.  Yani, uygulamada, sınırları bu limitler daha yüksektir. Bir kullanıcıdan gelen istekler genellikle Azure Kaynak Yöneticisi'nin farklı örnekleri tarafından işlenir.

## <a name="resource-provider-limits"></a>Kaynak sağlayıcı sınırları

Kaynak sağlayıcıları kendi azaltma sınırlarını uygular. Kaynak Yöneticisi ana kimlik ve Kaynak Yöneticisi örneğine göre daraldığından, kaynak sağlayıcısı önceki bölümdeki varsayılan sınırlardan daha fazla istek alabilir.

Bu bölümde, yaygın olarak kullanılan bazı kaynak sağlayıcıların azaltma sınırları açıklanabilir.

### <a name="storage-throttling"></a>Depolama azaltma

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="network-throttling"></a>Ağ kapasitesi azaltma

Microsoft.Network kaynak sağlayıcısı aşağıdaki azaltma sınırlarını uygular:

| İşlem | Sınır |
| --------- | ----- |
| yazmak / silme (PUT) | 5 dakikada 1000 |
| okuyun (GET) | 5 dakikada 10000 |

### <a name="compute-throttling"></a>İşlem azaltma

Bilgi işlem işlemleri için azaltma sınırları hakkında bilgi için Bkz. [Sorun Giderme API azaltma hataları - Bilgi İşlem](../../virtual-machines/troubleshooting/troubleshooting-throttling-errors.md).

Sanal makine ölçek kümesi içindeki sanal makine örneklerini denetlemek için [Sanal Makine Ölçeği Kümeleri işlemlerini](/rest/api/compute/virtualmachinescalesetvms)kullanın. Örneğin, Sanal [Makine Ölçeği Kümesi VM'leri](/rest/api/compute/virtualmachinescalesetvms/list) kullanın - Sanal makine örneklerinin güç durumunu kontrol etmek için parametreleri içeren listeyi kullanın. Bu API istek sayısını azaltır.

### <a name="azure-resource-graph-throttling"></a>Azure Kaynak Grafiği azaltma

[Azure Kaynak Grafiği,](../../governance/resource-graph/overview.md) işlemleriiçin istek sayısını sınırlar. Bu makalede, kalan istekleri belirlemek ve sınıra ulaşıldığında nasıl yanıt verilebildiğini belirlemek için atılan adımlar Kaynak Grafiği için de geçerlidir. Ancak, Kaynak Grafiği kendi sınırını ve sıfırlama hızını ayarlar. Daha fazla bilgi için kaynak [grafiği azaltma üstbilgilerine](../../governance/resource-graph/concepts/guidance-for-throttled-requests.md#understand-throttling-headers)bakın.

## <a name="error-code"></a>Hata kodu

Sınıra ulaştığınızda, HTTP durum kodu **429 Çok fazla istek**alırsınız. Yanıt, uygulamanızın bir sonraki isteği göndermeden önce beklemesi gereken (veya uyuması gereken) saniye sayısını belirten bir **Yeniden Deneme-Sonra** değeri içerir. Yeniden deneme değeri dolmadan önce bir istek gönderirseniz, isteğiniz işleme edilmez ve yeni bir yeniden deneme değeri döndürülür.

Belirtilen süreyi bekledikten sonra Azure bağlantınızı kapatıp yeniden açabilirsiniz. Bağlantıyı sıfırlayarak, Azure Kaynak Yöneticisi'nin farklı bir örneğine bağlanabilirsiniz.

Azure SDK kullanıyorsanız, SDK'nın otomatik yeniden deneme yapılandırması olabilir. Daha fazla bilgi [için Azure hizmetleri için Yeniden Deneme kılavuzuna](/azure/architecture/best-practices/retry-service-specific)bakın.

Bazı kaynak sağlayıcıları geçici bir sorunu bildirmek için 429 döndürün. Sorun, doğrudan isteğinizden kaynaklanamayan aşırı yükleme durumu olabilir. Veya, hedef kaynağın veya bağımlı kaynağın durumu hakkında geçici bir hata temsil edebilir. Örneğin, hedef kaynak başka bir işlem tarafından kilitlendiğinde ağ kaynak sağlayıcısı 429'u **Yeniden DeneebilirErrorDueToAnotherOperation** hata koduyla birlikte döndürür. Hatanın azaltmadan mı yoksa geçici bir durumdan mı kaynaklandığını belirlemek için yanıttaki hata ayrıntılarını görüntüleyin.

## <a name="remaining-requests"></a>Kalan istekler

Yanıt üstbilgilerini inceleyerek kalan istek sayısını belirleyebilirsiniz. Okuma istekleri, kalan okuma isteği sayısı için üstbilgide bir değer döndürün. Yazma istekleri, kalan yazma istekleri sayısı için bir değer içerir. Aşağıdaki tabloda, bu değerler için inceleyebileceğiniz yanıt üstbilgiaçıklanmaktadır:

| Yanıt üstbilgi | Açıklama |
| --- | --- |
| x-ms-ratelimit-kalan-abonelik-okuma |Abonelik kapsamı kalan okumalar. Bu değer okundu işlemlerinde döndürülür. |
| x-ms-ratelimit-kalan-abonelik-yazma |Abonelik kapsamı kalan yazıyor. Bu değer yazma işlemlerinde döndürülür. |
| x-ms-ratelimit-kalan-kiracı-okuma |Kiracı kapsamlı kalan okumaları |
| x-ms-ratelimit-kalan-kiracı-yazıyor |Kiracı kapsamlı kalan yazıyor |
| x-ms-ratelimit-kalan-abonelik-kaynak-istekleri |Abonelik kapsamı namına kaynak türü istekleri kaldı.<br /><br />Bu üstbilgi değeri yalnızca bir hizmet varsayılan sınırı geçersiz kılırsa döndürülür. Kaynak Yöneticisi, aboneliğin okuması veya yazması yerine bu değeri ekler. |
| x-ms-ratelimit-kalan-abonelik-kaynak-varlıklar-okuyun |Abonelik kapsamı namına kaynak türü toplama istekleri kaldı.<br /><br />Bu üstbilgi değeri yalnızca bir hizmet varsayılan sınırı geçersiz kılırsa döndürülür. Bu değer, kalan toplama isteklerinin (liste kaynakları) sayısını sağlar. |
| x-ms-ratelimit-kalan-kiracı-kaynak istekleri |Kiracı kapsamlı kaynak türü istekleri kaldı.<br /><br />Bu üstbilgi yalnızca kiracı düzeyindeki istekler için ve yalnızca bir hizmet varsayılan sınırı geçersiz kılırsa eklenir. Kaynak Yöneticisi, kiracının okuması veya yazması yerine bu değeri ekler. |
| x-ms-ratelimit-kalan-kiracı-kaynak-varlıklar-okuma |Kiracı kapsamlı kaynak türü toplama istekleri kaldı.<br /><br />Bu üstbilgi yalnızca kiracı düzeyindeki istekler için ve yalnızca bir hizmet varsayılan sınırı geçersiz kılırsa eklenir. |

Kaynak sağlayıcısı, kalan istekler hakkında bilgi içeren yanıt üstbilgilerini de döndürebilir. Bilgi İşlem kaynak sağlayıcısı tarafından döndürülen yanıt üstbilgileri hakkında bilgi için [bkz.](../../virtual-machines/troubleshooting/troubleshooting-throttling-errors.md#call-rate-informational-response-headers)

## <a name="retrieving-the-header-values"></a>Üstbilgi değerlerini alma

Bu üstbilgi değerlerini kodunuzda veya komut dosyanızda almak, herhangi bir üstbilgi değerini almaktan farklı değildir. 

Örneğin, **C#**'da, aşağıdaki kodla **yanıt** adlı bir **HttpWebResponse** nesnesinden üstbilgi değerini alırsınız:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

**PowerShell'de**üstbilgi değerini invoke-WebRequest işleminden alırsınız.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Tam bir PowerShell örneği [için, Abonelik için Kaynak Yöneticisi Sınırlarını Denetle'ye](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI)bakın.

Hata ayıklama için kalan istekleri görmek istiyorsanız, **PowerShell** cmdlet'inizin **-Hata Ayıklama** parametresini sağlayabilirsiniz.

```powershell
Get-AzResourceGroup -Debug
```

Aşağıdaki yanıt değeri de dahil olmak üzere birçok değer döndürür:

```output
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 11999
```

Yazma sınırlarını almak için bir yazma işlemi kullanın: 

```powershell
New-AzResourceGroup -Name myresourcegroup -Location westus -Debug
```

Aşağıdaki değerler de dahil olmak üzere birçok değer döndürür:

```output
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

**Azure CLI'de,** daha ayrıntılı seçeneği kullanarak üstbilgi değerini alırsınız.

```azurecli
az group list --verbose --debug
```

Aşağıdaki değerler de dahil olmak üzere birçok değer döndürür:

```output
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '11998'
```

Yazma sınırlarını almak için bir yazma işlemi kullanın: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Aşağıdaki değerler de dahil olmak üzere birçok değer döndürür:

```output
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="next-steps"></a>Sonraki adımlar

* Tam bir PowerShell örneği [için, Abonelik için Kaynak Yöneticisi Sınırlarını Denetle'ye](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI)bakın.
* Sınırlar ve kotalar hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın.
* Eşzamanlı REST isteklerini işleme hakkında bilgi edinmek için [eşsenkronize Azure işlemlerini izleyin'e](async-operations.md)bakın.
