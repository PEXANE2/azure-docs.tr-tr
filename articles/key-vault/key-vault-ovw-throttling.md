---
title: Azure Anahtar Kasası azaltma kılavuzu
description: Anahtar Vault azaltma, kaynakların aşırı kullanımını önlemek için eşzamanlı arama ların sayısını sınırlar.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: 6c4923e86f8678458d6301503043413fb8a5629b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197377"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Anahtar Kasası azaltma kılavuzu

Azaltma, kaynakların aşırı kullanımını önlemek için Azure hizmetine yapılan eşzamanlı çağrı sayısını sınırlayan bir işlemdir. Azure Anahtar Kasası (AKV), yüksek sayıdaki istekleri işlemek üzere tasarlanmıştır. Çok sayıda istek oluşursa, müşterinizin isteklerini daraltma, AKV hizmetinin en iyi performansını ve güvenilirliğini korumaya yardımcı olur.

Azaltma sınırları senaryoya göre değişir. Örneğin, büyük miktarda yazma yapıyorsanız, azaltma olasılığı yalnızca okuma ları gerçekleştirmenize göre daha yüksektir.

## <a name="how-does-key-vault-handle-its-limits"></a>Key Vault sınırlarını nasıl karşılar?

Key Vault'taki hizmet limitleri kaynakların kötüye kullanılmasını önler ve Key Vault'un tüm müşterileri için hizmet kalitesini sağlar. Bir hizmet eşiği aşıldığında, Key Vault bu istemciden gelen diğer istekleri bir süre için sınırlar, HTTP durum kodu 429 'u döndürür (Çok fazla istek) ve istek başarısız olur. Key Vault tarafından izlenen gaz limitlerine doğru 429 sayısı döndüren başarısız istekler. 

Key Vault aslında dağıtım zamanında sırlarınızı depolamak ve almak için kullanılmak üzere tasarlanmıştır.  Dünya gelişti ve Key Vault sırları depolamak ve almak için çalışma zamanında kullanılıyor ve genellikle uygulamalar ve hizmetler Key Vault'u veritabanı gibi kullanmak istiyor.  Geçerli limitler yüksek iş yapma oranlarını desteklemez.

Key Vault başlangıçta [Azure Key Vault hizmet limitlerinde](key-vault-service-limits.md)belirtilen limitler ile oluşturuldu.  Anahtar Kasanızı koyma oranları yla en üst düzeye çıkarmak için, iş lerinizi en üst düzeye çıkarmak için önerilen bazı kılavuz ilkeler/en iyi uygulamalar aşağıda verilmiştir:
1. Yerinde azaltma olduğundan emin olun.  Müşteri 429's için üstel geri dönüş politikalarına uymalı ve aşağıdaki kılavuza göre yeniden deneme yaptığınızdan emin olmalıdır.
1. Key Vault trafiğinizi birden fazla kasa ve farklı bölgeler arasında bölün.   Her güvenlik/kullanılabilirlik etki alanı için ayrı bir kasa kullanın.   Her biri iki bölgede olmak üzere beş uygulamanız varsa, her biri uygulama ve bölgeye özgü sırları içeren 10 kasa öneririz.  Tüm işlem türleri için abonelik genelindeki sınır, tek tek anahtar kasa sınırının beş katıdır. Örneğin, abonelik başına HSM-diğer işlemler abonelik başına 10 saniye içinde 5.000 işlemle sınırlıdır. RPS'yi doğrudan anahtar kasasına ve/veya işleme patlaması tabanlı trafiğe düşürmek için hizmetiniz veya uygulamanızdaki sırrı önbelleğe almayı düşünün.  Gecikmeyi en aza indirmek ve farklı bir abonelik/kasa kullanmak için trafiğinizi farklı bölgelere bölebilirsiniz.  Tek bir Azure bölgesinde Key Vault hizmetine abonelik sınırından daha fazlasını göndermeyin.
1. Azure Key Vault'tan aldığınız sırları bellekte önbelleğe alın ve mümkün olduğunca bellekten yeniden kullanın.  Azure Key Vault'tan yalnızca önbelleğe alınan kopya çalışmayı durdurduğunda (örn. kaynakta döndürüldeği için) yeniden okuyun. 
1. Key Vault kendi hizmet sırları için tasarlanmıştır.   Müşterilerinizin sırlarını saklıyorsanız (özellikle yüksek iş sahibi anahtar depolama senaryoları için), anahtarları şifreleme içeren bir veritabanına veya depolama hesabına koymayı ve azure Key Vault'ta sadece ana anahtarı depolamayı düşünün.
1. Ortak anahtar işlemlerini, yalnızca azaltma riskini azaltır, aynı zamanda güvenilirliği artırır (ortak anahtar malzemeyi düzgün bir şekilde önbelleğe aldığınız sürece) anahtar kasasına erişmeden gerçekleştirilebilir.
1. Bir hizmetin kimlik bilgilerini depolamak için Key Vault kullanıyorsanız, bu hizmetin doğrudan kimlik doğrulaması için AAD Kimlik Doğrulamasını destekleyip desteklemediğini denetleyin. Bu, Key Vault üzerindeki yükü azaltır, güvenilirliği artırır ve Key Vault artık AAD belirteci kullanabilirsiniz beri kodunuzu kolaylaştırır.  Birçok hizmet AAD Auth kullanarak taşındı.  [Azure kaynakları için yönetilen kimlikleri destekleyen Hizmetler'deki](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources)geçerli listeye bakın.
1. Mevcut RPS sınırlarının altında kalmak için yükünüzü/dağıtımınızı daha uzun bir süre boyunca sarsmayı düşünün.
1. Uygulamanız aynı gizli (ler) okumak için gereken birden fazla düğümden oluşuyorsa, bir varlığın Key Vault'tan sırrı okuduğu ve tüm düğümlere hayran olduğu bir fan çıkış deseni kullanmayı düşünün.   Alınan sırları yalnızca bellekte önbelleğe alın.
Yukarıdakilerin hala ihtiyaçlarınızı karşılamadığını fark ederseniz, lütfen aşağıdaki tabloyu doldurun ve ek kapasitenin eklenebilir olduğunu belirlemek için bizimle iletişime geçin (yalnızca açıklayıcı amaçlar için aşağıda belirtilen örnek).

| Kasa adı | Vault Bölgesi | Nesne türü (Gizli, Anahtar veya Cert) | İşlem(ler)* | Anahtar Türü | Anahtar Uzunluğu veya Eğrisi | HSM anahtarı mı?| Sabit durum RPS gerekli | Tepe RPS gerekli |
|--|--|--|--|--|--|--|--|--|
| https://mykeyvault.vault.azure.net/ | | Anahtar | İşaret | EC | P-256 | Hayır | 200 | 1000 |

\*Olası değerlerin tam listesi için [Azure Anahtar Kasası işlemlerine](/rest/api/keyvault/key-operations)bakın.

Ek kapasite onaylanırsa, kapasite artışları sonucunda aşağıdakileri dikkate alın:
1. Veri tutarlılığı modeli değişir. Bir kasaya ek iş hacmi kapasitesiyle izin verilenden, Key Vault hizmeti veri tutarlılığı değişiklikleri garanti eder (temel Azure Depolama hizmeti devam edemediği için daha yüksek hacimli RPS'yi karşılamak için gereklidir).  Kısaca:
  1. **İzin siz giriş**: Key Vault hizmeti bir yazma işleminin sonuçlarını yansıtacaktır (örneğin. SecretSet, CreateKey) hemen sonraki aramalarda (örneğin. SecretGet, KeySign).
  1. **İzin girişi ile**: Key Vault hizmeti bir yazma işleminin sonuçlarını yansıtacaktır (örneğin. SecretSet, CreateKey) sonraki aramalarda 60 saniye içinde (örneğin. SecretGet, KeySign).
1. İstemci kodu, 429 yeniden deneme için geri tepme ilkesine uymalıdır. Key Vault hizmetini çağıran istemci kodu, 429 yanıt kodu aldığında Key Vault isteklerini hemen yeniden denememelidir.  Burada yayınlanan Azure Key Vault azaltma kılavuzu, 429 Http yanıt kodu alırken üstel geri leme uygulamanızı önerir.

Daha yüksek gaz limitleri için geçerli bir iş durumunuz varsa, lütfen bize ulaşın.

## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Hizmet limitlerine yanıt olarak uygulamanızı azaltma

Hizmetiniz daraltıldığında uygulamanız gereken **en iyi uygulamalar** şunlardır:
- İsteme başına işlem sayısını azaltın.
- İsteklerin sıklığını azaltın.
- Hemen yeniden denemelerden kaçının. 
    - Tüm istekler kullanım sınırlarınıza göre tahakkuk ettirilen.

Uygulamanızın hata işlemesini uyguladığınızda, istemci tarafı azaltma gereksinimini algılamak için HTTP hata kodu 429'u kullanın. İstek BIR HTTP 429 hata koduyla yine başarısız olursa, yine bir Azure hizmet sınırıyla karşılaşabilirsiniz. İstemi başarılı olana kadar yeniden deneyerek önerilen istemci tarafı azaltma yöntemini kullanmaya devam edin.

Üstel geri tepme uygulayan kod aşağıda gösterilmiştir. 
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


Bu kodu bir istemci C# uygulamasında kullanmak kolaydır. 

### <a name="recommended-client-side-throttling-method"></a>Önerilen istemci tarafı azaltma yöntemi

HTTP hata kodu 429'da, üstel bir geri dönüş yaklaşımı kullanarak istemcinizi daraltmaya başlayın:

1. 1 saniye bekleyin, isteği yeniden deneyin
2. Hala daraltılmışsa bekleme 2 saniye, yeniden deneme isteği
3. Hala daraltılmışsa bekleme 4 saniye, yeniden deneme isteği
4. Hala daraltılmışsa 8 saniye bekleyin, isteği yeniden deneyin
5. 16 saniye bekleyin, hala daraltılmışsa, isteği yeniden deneyin

Bu noktada, HTTP 429 yanıt kodları almamalısınız.

## <a name="see-also"></a>Ayrıca bkz.

Microsoft Cloud'da azaltmanın daha derin bir yönü için [Azaltma Deseni'ne](https://docs.microsoft.com/azure/architecture/patterns/throttling)bakın.

