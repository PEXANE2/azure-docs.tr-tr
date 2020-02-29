---
title: Azure Key Vault azaltma kılavuzu
description: Key Vault azaltma, kaynakların aşırı kullanımını önlemek için eş zamanlı çağrılar sayısını sınırlar.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: 6c4923e86f8678458d6301503043413fb8a5629b
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197377"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault azaltma kılavuzu

Azaltma, kaynakların aşırı kullanımını önlemek için bir Azure hizmetine eş zamanlı çağrı sayısını sınırlayan başlatma bir işlemdir. Azure Key Vault (AKV) büyük hacimde istekleri işlemek için tasarlanmıştır. Büyük bir istek sayısı ortaya çıkarsa, istemcinin istekleri azaltma en iyi performans ve güvenilirlik AKV hizmetinin korumasına yardımcı olur.

Azaltma sınırları senaryoya bağlı olarak değişiklik gösterir. Örneğin, büyük hacimli yazma gerçekleştiriyorsanız için azaltma olanağı, yalnızca okuma işlemi yapıyorsanız daha yüksek olur.

## <a name="how-does-key-vault-handle-its-limits"></a>Key Vault, kendi sınırlarına nasıl işliyor?

Key Vault hizmet limitleri, kaynakların kötüye kullanımını engeller ve tüm Key Vault istemcileri için hizmet kalitesini güvence altına aldığınızdan emin olun. Bir hizmet eşiği aşıldığında, bu istemciden bir süre için diğer istekleri sınırlar Key Vault, HTTP durum kodu 429 (çok fazla istek) döndürür ve istek başarısız olur. Key Vault tarafından izlenen kısıtlama sınırlarına doğru 429 sayısı döndüren başarısız istekler. 

Key Vault ilk olarak, dağıtım zamanında sırlarınızı depolamak ve almak için kullanılmak üzere tasarlanmıştır.  Dünya gelişmiştir ve gizli dizileri depolamak ve almak için çalışma zamanında kullanılıyor Key Vault ve genellikle uygulama ve hizmetler bir veritabanı gibi Key Vault kullanmak ister.  Geçerli sınırlar yüksek verimlilik hızlarını desteklemez.

Key Vault ilk olarak [Azure Key Vault hizmet sınırlarında](key-vault-service-limits.md)belirtilen limitlerle oluşturulmuştur.  Key Vault, koyma ücretleri üzerinden en üst düzeye çıkarmak için, aktarım hızını en üst düzeye çıkarmak için önerilen bazı yönergeler/en iyi uygulamalar şunlardır
1. Azaltma yapıldığından emin olun.  İstemci, 429 ' un üstel geri dönüş ilkelerini kabul etmelidir ve aşağıdaki kılavuza göre yeniden denemeler yapmakta olduğunuzdan emin olmalıdır.
1. Key Vault trafiğinizi birden çok kasa ve farklı bölgeler arasında bölün.   Her güvenlik/kullanılabilirlik etki alanı için ayrı bir kasa kullanın.   Her biri iki bölgede beş uygulamanız varsa, her biri uygulama ve bölgeye özgü gizli dizileri içeren 10 kasalar öneririz.  Tüm işlem türleri için abonelik genelinde sınır, tek bir Anahtar Kasası sınırının beş katından fazla olur. Örneğin, HSM-abonelik başına diğer işlemler, abonelik başına 10 saniye içinde 5.000 işlem ile sınırlıdır. Ayrıca, RPS 'yi doğrudan Anahtar Kasası 'na düşürmek ve/veya veri bloğu tabanlı trafiği işlemek için hizmet veya uygulamanızdaki gizli anahtarı önbelleğe almayı düşünün.  Ayrıca, gecikme süresini en aza indirmek ve farklı bir abonelik/kasa kullanmak için trafiğinizi farklı bölgeler arasında ayırabilirsiniz.  Tek bir Azure bölgesindeki Key Vault hizmetine daha fazla abonelik sınırı göndermeyin.
1. Bellekte Azure Key Vault aldığınız gizli dizileri önbelleğe alın ve mümkün olan her durumda bellekten yeniden kullanın.  Yalnızca önbelleğe alınmış kopya çalışmayı durdurduğu zaman Azure Key Vault yeniden oku (örneğin, kaynakta döndürülmüştür). 
1. Key Vault kendi hizmet sırlarınız için tasarlanmıştır.   Müşterilerinizin gizli dizilerini depoluyorsanız (özellikle yüksek işlem hacmi olan anahtar depolama senaryoları için), anahtarları bir veritabanına veya depolama hesabına şifrelemeye koymak ve Azure Key Vault yalnızca ana anahtarı depolamak için göz önünde bulundurun.
1. Genel anahtar işlemlerini şifreleme, sarın ve doğrulama, azaltma riskini azalmayan, ancak aynı zamanda güvenilirliği artıran (ortak anahtar malzemesini doğru şekilde önbelleğe aldığınız sürece) Key Vault erişim olmadan gerçekleştirilebilir.
1. Bir hizmet için kimlik bilgilerini depolamak üzere Key Vault kullanıyorsanız, bu hizmetin doğrudan kimlik doğrulamak için AAD kimlik doğrulamasını destekleyip desteklemediğini denetleyin. Bu, Key Vault yükünü azaltır, güvenilirliği geliştirir ve Key Vault artık AAD belirtecini kullanabilmesi için kodunuzu basitleştirir.  Birçok hizmet AAD kimlik doğrulaması kullanılarak taşınmıştır.  [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources)kısmındaki geçerli listeye bakın.
1. Geçerli RPS limitlerinin altında kalmak için yük/dağıtımınızı daha uzun bir süre boyunca kademelendirme değerlendirin.
1. Uygulamanız aynı gizli dizi (ler) i okuması gereken birden çok düğüm içeriyorsa, bir varlığın Key Vault gizli dizi ve tüm düğümlere giden fanları okuduğu bir fan çıkış deseninin kullanılmasını düşünün.   Alınan gizli dizileri yalnızca bellekte önbelleğe al.
Yukarıdakilerden hala gereksinimlerinizi karşılamadığını fark ediyorsanız, lütfen aşağıdaki tabloyu doldurun ve hangi ek kapasitenin eklenebileceklerini öğrenmek için bizimle iletişim kurun (yalnızca tanım amaçları için aşağıda verilmiştir).

| Kasa adı | Kasa bölgesi | Nesne türü (gizli, anahtar veya sertifika) | İşlemler * | Anahtar türü | Anahtar uzunluğu veya eğrisi | HSM anahtarı?| Sabit durum RPS gerekli | Gerekli en yüksek RPS |
|--|--|--|--|--|--|--|--|--|
| https://mykeyvault.vault.azure.net/ | | Anahtar | İmzalayabilirsiniz | EC | P-256 | Hayır | 200 | 1000 |

olası değerlerin tam listesi Için \* bkz. [Azure Key Vault işlemler](/rest/api/keyvault/key-operations).

Ek kapasite onaylanırsa, kapasitenin sonucu arttıkça lütfen aşağıdakileri unutmayın:
1. Veri tutarlılığı modeli değişiklikleri. Kasa, ek aktarım hızı kapasitesine izin vertikten sonra, Key Vault Service veri tutarlılığı garantisi (temeldeki Azure depolama hizmeti devam edemediğinden daha yüksek hacimli RPS 'yi karşılamak için gereklidir).  Bir Nutshell 'de:
  1. **Listeye izin verme olmadan**: Key Vault hizmeti bir yazma işleminin sonuçlarını yansıtır (örn. SecretSet, CreateKey) sonraki çağrılarda hemen (ör. SecretGet, KeySign).
  1. **İzin verilenler listesi ile**: Key Vault hizmeti bir yazma işleminin sonuçlarını yansıtır (örn. SecretSet, CreateKey) sonraki çağrılarda 60 saniye içinde (örn. SecretGet, KeySign).
1. İstemci kodu, 429 yeniden deneme için geri dönüş ilkesini kabul etmelidir. Key Vault hizmetini çağıran istemci kodu, 429 yanıt kodu aldığında istekleri Key Vault anında yeniden denememelidir.  Burada yayımlanan Azure Key Vault daraltma Kılavuzu, 429 http yanıt kodu alınırken üstel geri alma uygulanmasını önerir.

İşle ilgili geçerli durum azaltma sınırları için varsa, lütfen bizimle iletişime geçin.

## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Nasıl kısıtlanacağını uygulamanızın yanıt olarak hizmet sınırları

Aşağıda, hizmetiniz kısıtlandığınızda uygulamanız gereken **en iyi yöntemler** verilmiştir:
- İstek başına işlemlerin sayısını azaltın.
- İstekleri sıklığını azaltın.
- Hemen yeniden deneme kaçının. 
    - Tüm istekler, kullanım sınırlarını karşı tahakkuk eder.

Uygulamanızın hata işleme uygularken, istemci tarafı azaltma ihtiyacına algılamak için HTTP hata kodu 429 kullanın. İstek yine bir HTTP 429 hata koduyla başarısız olursa, bir Azure hizmeti sınırını hala karşılaşıyoruz. Önerilen yöntem azaltma, başarılı olana kadar istek yeniden deneniyor taraflı kullanmaya devam edin.

Üstel geri alma uygulayan kodu aşağıda gösterilmiştir. 
```
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
    var client = new SecretClient(new Uri(https://keyVaultName.vault.azure.net"), new DefaultAzureCredential(),options);
                                 
    //Retrieve Secret
    secret = client.GetSecret(secretName);
```


Bu kodun bir istemci C# uygulamasında kullanılması basittir. 

### <a name="recommended-client-side-throttling-method"></a>Önerilen istemci-tarafı azaltma yöntemi

HTTP hata kodu 429 üzerinde bir üstel geri alma yaklaşımı kullanarak istemci azaltma başlayın:

1. 1 saniye, yeniden deneme isteği bekleyin
2. Yine de 2 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
3. Yine de 4 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
4. Yine de 8 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
5. Hala 16 saniye bekleyin kısıtlanan, isteği yeniden deneyin.

Bu noktada, HTTP 429 yanıtı kodları alamıyorsanız.

## <a name="see-also"></a>Ayrıca bkz.

Microsoft Bulut azaltma daha derin bir yönü için bkz. [daraltma kriteri](https://docs.microsoft.com/azure/architecture/patterns/throttling).

