---
title: Azure Anahtar Kasası erişim ilkesi sorunlarını giderme
description: Azure Anahtar Kasası erişim ilkesi sorunlarını giderme
author: sebansal
ms.author: sebansal
ms.date: 08/10/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: 270e5ba1879b229fbe9f5e6c8692bd8b4e9eebc7
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88688649"
---
# <a name="troubleshooting-azure-key-vault-access-policy-issues"></a>Azure Anahtar Kasası erişim ilkesi sorunlarını giderme

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-identify-how-and-when-key-vaults-are-accessed"></a>Anahtar kasalarına nasıl ve ne zaman erişildiğini nasıl tanımlayabilirim?
Bir veya daha fazla Anahtar Kasası oluşturduktan sonra muhtemelen anahtar kasalarınızın nasıl ve ne zaman erişildiğini ve kim tarafından yapılacağını izlemek isteyeceksiniz. Bunu, Azure Key Vault için günlüğe kaydetmeyi etkinleştirerek yapabilirsiniz, günlük kaydını etkinleştirmek için adım adım kılavuz için [daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/general/logging).

### <a name="how-can-i-monitor-vault-availability-service-latency-periods-or-other-performance-metrics-for-key-vault"></a>Kasa kullanılabilirliğini, hizmet gecikme sürelerini veya Anahtar Kasası için diğer performans ölçümlerini nasıl izleyebilirim?
Hizmetinizi ölçeklendirmeye başladığınızda, anahtar kasanıza gönderilen isteklerin sayısı artacaktır. Bu, isteklerinizin gecikme süresini artırmak ve olağanüstü durumlarda, hizmetinizin performansını etkileyecek olan isteklerinizin kısıtlanmasına neden olur. Anahtar Kasası performans ölçümlerini izleyebilir ve belirli eşiklere göre uyarı alabilir, izlemeyi yapılandırmaya yönelik adım adım kılavuz için [daha fazla](https://docs.microsoft.com/azure/key-vault/general/alert)bilgi edinebilirsiniz.

### <a name="how-can-i-assign-access-control-per-key-vault-object"></a>Anahtar Kasası nesnesine göre erişim denetimini nasıl atayabilirim? 
Gizli/anahtar/sertifika erişim denetimi özelliğinin kullanılabilirliğine buradan bildirim verilecektir, [daha fazla bilgi edinin](https://feedback.azure.com/forums/906355-azure-key-vault/suggestions/32213176-per-secret-key-certificate-access-control)

### <a name="how-can-i-provide-key-vault-authenticate-using-access-control-policy"></a>Erişim denetimi ilkesini kullanarak Anahtar Kasası kimlik doğrulamasını nasıl sağlayabilirim?
Key Vault için bulut tabanlı bir uygulamanın kimlik doğrulamasının en kolay yolu, yönetilen bir kimlikle; Ayrıntılar için [Azure Key Vault erişmek üzere App Service yönetilen bir kimlik kullanma]( https://docs.microsoft.com/azure/key-vault/general/managed-identity) konusuna bakın.
Şirket içi bir uygulama oluşturuyorsanız, yerel geliştirme yapıyor veya yönetilen bir kimlik kullanmıyorsanız, bir hizmet sorumlusunu el ile kaydedebilir ve bir erişim denetim ilkesi kullanarak anahtar kasanıza erişim sağlayabilirsiniz, [daha fazla](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps)bilgi edinebilirsiniz.


### <a name="how-can-i-give-the-ad-group-access-to-the-key-vault"></a>AD grubuna Anahtar Kasası erişimine nasıl izin veririm?
Azure CLı az keykasa Set-Policy komutunu veya Azure PowerShell set-AzKeyVaultAccessPolicy cmdlet 'ini kullanarak anahtar kasanıza AD grubu izinleri verin. Örnekler için bkz. [uygulama, Azure AD grubu veya anahtar kasanıza Kullanıcı erişimi verin](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps#give-the-principal-access-to-your-key-vault).

Uygulamanın Ayrıca anahtar kasasına atanmış en az bir kimlik ve erişim yönetimi (ıAM) rolü olması gerekir. Aksi takdirde, oturum açamayacak ve aboneliğe erişmek için yetersiz haklarla başarısız olacak. Yönetilen kimlikleri olan Azure AD grupları, belirteci yenilemek ve etkili hale gelmesi için 8 saat kadar sürebilir.

### <a name="how-can-i-redeploy-key-vault-with-arm-template-without-deleting-existing-access-policies"></a>Mevcut erişim ilkelerini silmeden ARM şablonuyla Key Vault nasıl yeniden dağırım?
Şu anda Key Vault ARM redopleyment, Key Vault tüm erişim ilkelerini silecek ve bunları ARM şablonundaki erişim ilkesiyle değiştirecek. Key Vault erişim ilkeleri için artımlı bir seçenek yoktur. Key Vault erişim ilkelerini korumak için, Key Vault ' de var olan erişim ilkelerini okumanız ve ARM şablonunu bu ilkelerle doldurmanız gerekir.

### <a name="recommended-troubleshooting-steps-for-following-error-types"></a>Aşağıdaki hata türleri için önerilen sorun giderme adımları
* HTTP 401: kimliği doğrulanmamış Istek- [sorun giderme adımları](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-401-unauthenticated-request)
* HTTP 403: yetersiz Izinler- [sorun giderme adımları](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-403-insufficient-permissions)
* HTTP 429: çok fazla Istek- [sorun giderme adımları](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-429-too-many-requests)
* Anahtar kasası için silme erişim iznine sahip olup olmadığınızı denetleyin: [Key Vault Erişim İlkeleri](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps)
* Kodda anahtar kasasının kimliğini doğrulama sorununuz varsa [Kimlik Doğrulaması SDK’sını](https://azure.github.io/azure-sdk/posts/2020-02-25/defaultazurecredentials.html) kullanın

### <a name="what-are-the-best-practices-i-should-implement-when-key-vault-is-getting-throttled"></a>Anahtar Kasası kısıtlanarak uygulamam gereken en iyi uygulamalar nelerdir?
[Burada](https://docs.microsoft.com/azure/key-vault/general/overview-throttling#how-to-throttle-your-app-in-response-to-service-limits) belgelenen en iyi uygulamaları izleyin

## <a name="next-steps"></a>Sonraki Adımlar

Anahtar Kasası kimlik doğrulama hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin. [Key Vault sorun giderme kılavuzu](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes)
