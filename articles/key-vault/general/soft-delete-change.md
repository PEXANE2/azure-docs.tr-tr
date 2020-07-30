---
title: Geçici silme, tüm Azure Anahtar Kasası 'nda etkinleştirilecek | Microsoft Docs
description: Tüm Anahtar kasaları için geçici silme benimsemek üzere bu belgeyi kullanın.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/27/2020
ms.author: sudbalas
ms.openlocfilehash: 91068bacee0bde9e11cc23816bd8e2a101854fb2
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87388229"
---
# <a name="soft-delete-will-be-enabled-on-all-key-vaults"></a>Geçici silme, tüm anahtar kasaları üzerinde etkinleştirilecek

> [!WARNING]
> **Son değişiklik**: geçici silme işlemini devre dışı bırakma özelliği, yılın sonuna kadar kullanım dışı olacaktır ve tüm anahtar kasaları için otomatik olarak geçici silme koruması açılır.  Azure Key Vault kullanıcılar ve Yöneticiler, anahtar kasalarında doğrudan geçici silme olanağı sağlamalıdır.

Gizli dizi koruması olmayan bir anahtar kasasından gizli dizi silindiğinde, gizli dizi kalıcı olarak silinir. Kullanıcılar şu anda Anahtar Kasası oluşturma sırasında geçici silme işlemini kapatabilir, ancak gizli dizileri bir kullanıcı tarafından yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için, Microsoft yakında **Tüm** Anahtar kasaları üzerinde geçici silme korumasını etkinleştirir ve kullanıcılar artık devre dışı bırakma veya etkinleştirme seçeneğine sahip olmayacaktır.

:::image type="content" source="../media/softdeletediagram.png" alt-text="<alt metin>":::

Geçici silme işleviyle ilgili tüm ayrıntılar için bkz. [Azure Key Vault geçici görünüm](soft-delete-overview.md).

## <a name="how-do-i-respond-to-breaking-changes"></a>Nasıl yaparım? son değişikliklere yanıt verme

Bir Anahtar Kasası nesnesi, geçici olarak silinen durumda bir Anahtar Kasası nesnesi ile aynı ada sahip oluşturulamaz.  Örneğin, `test key` Anahtar Kasası a 'da adlı bir anahtarı silerseniz, `test key` geçici olarak silinen nesne temizlenene kadar, Anahtar Kasası içinde adlı yeni bir anahtar oluşturabileceksiniz `test key` .

### <a name="application-changes"></a>Uygulama değişiklikleri

Uygulamanız, geçici silme özelliğinin etkinleştirilmediğini ve silinen gizli dizi ya da Anahtar Kasası adlarının hemen yeniden kullanım için kullanılabilir olduğunu varsaydığında, bu değişikliği benimsemek için uygulama mantığınızın aşağıdaki değişiklikleri yapması gerekir.

1. Özgün anahtar kasasını veya parolayı silme
2. Geçici silme durumunda anahtar kasasını veya gizli anahtarı temizleyin.
3. Bekle – hemen yeniden oluşturma bir çakışmaya neden olabilir.
4. Anahtar kasasını aynı adla yeniden oluşturun.
5. Yeniden deneyin oluşturma işlemi, ad çakışma hatası ile sonuçlanmasına devam ederse, DNS kayıtlarının en kötü durum senaryosunda güncelleştirilmesi 10 dakikaya kadar sürebilir.

### <a name="administration-changes"></a>Yönetim değişiklikleri

Gizli dizileri kalıcı olarak silme erişimi gerektiren güvenlik ilkelerine, bu gizli dizileri ve anahtar kasasını temizlemek için ek erişim ilkesi izinlerinin verilmesi gerekir.

Anahtar kasalarınızda, geçici silme özelliğinin kapalı olduğunu belirten bir Azure Ilkeniz varsa, bu ilkenin devre dışı bırakılması gerekir.  Bu sorunu, ortamınıza uygulanan Azure Ilkelerini denetleyen bir yöneticiye iletmek zorunda kalabilirsiniz. Bu ilke devre dışı bırakılsa, uygulanan ilkenin kapsamında yeni anahtar kasaları oluşturma özelliğini kaybedebilirsiniz.

Kuruluşunuzun geçerli uyumluluk gereksinimlerine sahip olması ve Silinen anahtar kasalarının ve parolaların kurtarılabilir bir durumda kalmasına izin vermediyse, uzun bir süre boyunca, kuruluşunuzun standartlarını karşılamak için 7 – 90 gün arasında yapılandırılabilir olan geçici silme bekletme süresini ayarlamanız gerekir.

## <a name="procedures"></a>Yordamlar

### <a name="audit-your-key-vaults-to-check-if-soft-delete-is-enabled"></a>Geçici silme özelliğinin etkin olup olmadığını denetlemek için anahtar kasalarınızı denetleme

1. Azure portalında oturum açın.
2. "Azure Ilkesi" araması yapın.
3. "Tanımlar" ı seçin.
4. Kategori altında, filtrede "Key Vault" öğesini seçin.
5. "Key Vault nesneleri kurtarılabilir olmalıdır" ilkesini seçin.
6. "Ata" ya tıklayın.
7. Kapsamını aboneliğiniz olarak ayarlayın.
8. "Gözden geçir + oluştur" u seçin.
9. Ortamında, ortamınızda tam taramanın tamamlanması 24 saate kadar sürebilir.
10. Azure Ilkesi dikey penceresinde "Uyumluluk" e tıklayın.
11. Uyguladığınız ilkeyi seçin.

Artık anahtar kasalarınızdan hangilerinin geçici silme etkin (uyumlu kaynaklar) ve hangi anahtar kasalarının yazılım için (uyumlu olmayan kaynak) etkin olmadığını filtreleyebilmeli.

### <a name="turn-on-soft-delete-for-an-existing-key-vault"></a>Mevcut bir Anahtar Kasası için geçici silmeyi aç

1. Azure portalında oturum açın.
2. Key Vault arayın.
3. Ayarlar altında "Özellikler" i seçin.
4. Geçici silme altında "kurtarmayı etkinleştir" öğesine karşılık gelen radyo düğmesini seçin. Bu kasanın ve nesnelerinin ".
5. Geçici silme için saklama süresini ayarlayın.
6. "Kaydet" i seçin.

### <a name="grant-purge-access-policy-permissions-to-a-security-principal"></a>Güvenlik sorumlusu için Temizleme erişim ilkesi izinleri verme

1. Azure portalında oturum açın.
2. Key Vault arayın.
3. Ayarlar altında "erişim Ilkeleri" ni seçin.
4. Erişim vermek istediğiniz hizmet sorumlusu ' nı seçin.
5. Anahtar, gizli dizi ve sertifika izinleri altındaki her bir açılan menü için "ayrıcalıklı Işlemler" i aşağı kaydırın ve "Temizle" iznini seçin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-this-change-affect-me"></a>Bu değişiklik beni etkiler mi?

Zaten geçici silme özelliği açıksa veya aynı ada sahip Anahtar Kasası nesnelerini silip yeniden oluşturmadıysanız, büyük olasılıkla anahtar kasasının davranışında herhangi bir değişiklik fark vermeyecektir.

Aynı adlandırma kurallarına göre Anahtar Kasası nesnelerini silen ve yeniden oluşturan bir uygulamanız varsa, beklenen davranışı korumak için uygulama mantığınızdaki değişiklikleri yapmanız gerekir. Lütfen "değişiklikleri kıranın kesilmesi Nasıl yaparım? mi?" bölümüne bakın. Yukarıdaki bölüm.

### <a name="how-do-i-benefit-from-this-change"></a>Bu değişiklikten faydalanmak Nasıl yaparım? misiniz?

Geçici silme koruması, kuruluşunuza yanlışlıkla veya kötü amaçlı silmeye karşı ek bir koruma katmanı sağlar. Bir Anahtar Kasası Yöneticisi olarak, erişimi hem kurtarma izinleri hem de Temizleme izinleri ile kısıtlayabilirsiniz.

Kullanıcı yanlışlıkla bir Anahtar Kasası veya gizli dizi silerse, gizli dizi veya anahtar kasasını kalıcı olarak sildikleri bir risk oluşturmadan gizli dizi erişimini kurtarmak için onlara erişim izinleri verebilirsiniz. Bu self servis işlemi ortamınızdaki bir süre en aza indirir ve sırlarınızın kullanılabilirliğini garanti eder.

### <a name="how-do-i-find-out-if-i-need-to-take-action"></a>Nasıl yaparım?, eyleme ihtiyacım olup olmadığını öğrenmek mi istiyorsunuz?

Lütfen "geçici silme işlemi açık olup olmadığını denetlemek için anahtar kasalarınızı denetleme yordamı" başlıklı bölümde yer alan yukarıdaki adımları izleyin. Geçici silme özelliği açık olmayan herhangi bir Anahtar Kasası, bu değişiklikten etkilenir. Denetlemeye yardımcı olacak ek araçlar yakında sunulacaktır ve bu belge güncelleştirilir.

### <a name="what-action-do-i-need-to-take"></a>Ne yapmam gerekir?

Uygulama mantığınızdaki değişiklikleri yapmak zorunda olmadığından emin olun. Bunu onayladıktan sonra tüm anahtar kasalarınızda geçici silme özelliğini açın. Bu, yılın sonundaki tüm anahtar kasaları için geçici silme açık olduğunda bir son değişiklikten etkilenmemesini sağlayacaktır.

### <a name="by-when-do-i-need-to-take-action"></a>Ne zaman işlem yapmam gerekir?

Geçici silme, yıl sonuna kadar tüm anahtar kasaları için açık olacaktır. Uygulamalarınızın etkilenmediğinden emin olmak için, anahtar kasalarınızda mümkün olan en kısa sürede geçici silme özelliğini açın.

## <a name="what-will-happen-if-i-dont-take-any-action"></a>Hiçbir eylemde yoksa ne olur?

Herhangi bir işlem yapmazsanız, yıl sonunda tüm anahtar kasaları için geçici silme otomatik olarak açılır. Bu, bir Anahtar Kasası nesnesini silmeye ve bunu önce geçici olarak silinen durumdan kaldırmadan aynı adla yeniden oluşturmaya çalıştığınızda çakışma hatalarına neden olabilir. Bu durum uygulamalarınızın veya otomasyonunun başarısız olmasına neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bu değişiklik ile ilgili tüm sorularınızı bizimle iletişime geçin [akvsoftdelete@microsoft.com](mailto:akvsoftdelete@microsoft.com) .
- [Geçici silme genel bakışını](soft-delete-overview.md) okuyun
