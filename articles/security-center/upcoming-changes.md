---
title: Azure Güvenlik Merkezi 'ne gelen önemli değişiklikler
description: Azure Güvenlik Merkezi 'Nde, planlamanız gerekebilecek ve planlamanız gerekebilen değişiklikler
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2021
ms.author: memildin
ms.openlocfilehash: 83c1d8ac316703d9d22d0716c86c5731c3db7133
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99226480"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

- [Kubernetes iş yükü koruma önerileri yakında genel kullanılabilirlik (GA) için kullanıma sunulacaktır](#kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga)
- ["Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [SQL veri sınıflandırması önerisine yönelik geliştirmeler](#enhancements-to-sql-data-classification-recommendation)


### <a name="kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga"></a>Kubernetes iş yükü koruma önerileri yakında genel kullanılabilirlik (GA) için kullanıma sunulacaktır

**Değişikliğin tahmini tarihi:** Ocak 2021

[Kubernetes iş yüklerinizi koruma](kubernetes-workload-protections.md) bölümünde açıklanan Kubernetes iş yükü koruma önerileri şu anda önizleme aşamasındadır. Bir öneri önizlemede olduğunda, bir kaynağı sağlıksız olarak işlemez ve güvenli puanınızın hesaplamalarına dahil değildir.

Bu öneriler yakında genel kullanıma sunulacaktır (GA) ve *Bu nedenle puan hesaplamasına dahil edilir.* Zaten düzeltilmediyse, bu durum güvenli puanınızda küçük bir etkiye neden olur.

Mümkün olan yerlerde düzeltin ( [Azure Güvenlik Merkezi 'nde önerileri](security-center-remediate-recommendations.md)düzeltme hakkında bilgi edinin).

Kubernetes iş yükü koruma önerileri şunlardır:

- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır
- Kapsayıcı CPU ve bellek sınırları zorunlu kılınmalıdır
- Ayrıcalıklı kapsayıcılar kaçınılmalıdır
- Kapsayıcılar için sabit (salt okunurdur) kök dosya sistemi zorunlu kılınmalıdır
- Ayrıcalık yükseltme ile kapsayıcının önlenebilir olması gerekir
- Kök kullanıcı olarak çalışan kapsayıcılar önlenebilir olmalıdır
- Gizli ana bilgisayar ad alanlarını paylaşan kapsayıcıların önlenebilir olması gerekir
- Kapsayıcılar için en az ayrıcalıklı Linux özellikleri zorlanmalıdır
- Pod HostPath birimi takmaları kullanımı bilinen bir listeyle sınırlandırılmalıdır
- Kapsayıcılar yalnızca izin verilen bağlantı noktalarında dinleme yapılmalıdır
- Hizmetlerin yalnızca izin verilen bağlantı noktalarını dinlemesi gerekir
- Konak ağ ve bağlantı noktalarının kullanımı sınırlandırılmalıdır
- Kapsayıcılar AppArmor profilinin geçersiz kılınması veya devre dışı bırakılması kısıtlanıyor
- Kapsayıcı görüntüleri yalnızca güvenilen kayıt defterlerinden dağıtılmalıdır             

[Kubernetes iş yüklerinizi korumak](kubernetes-workload-protections.md)için bu öneriler hakkında daha fazla bilgi edinin.

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>"Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi 

**Değişikliğin tahmini tarihi:** Şubat 2021

Aşağıdaki iki öneri Şubat 2021 ' de kullanımdan kalkmak üzere zamanlandı:

- **Sistem güncelleştirmelerinin uygulanabilmesi için makinelerinizin yeniden başlatılması gerekiyor**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.
- **İzleme Aracısı makinelerinizde yüklü olmalıdır**. Bu öneri yalnızca şirket içi makinelerle ilgilidir ve mantığının bir kısmı başka bir öneriye aktarılacaktır. **Log Analytics aracı sistem durumu sorunları makinelerinizde çözümlenmelidir**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.

Bu önerilerin bunlara dahil edilip edilmeyeceğini görmek için sürekli dışarı aktarma ve iş akışı Otomasyonu yapılandırmalarının denetlenmesini öneririz. Ayrıca, bunları kullanıyor olabilecek panolar veya diğer izleme araçları da buna uygun şekilde güncelleştirilmeleri gerekir.

[Güvenlik önerileri başvuru sayfasında](recommendations-reference.md)bu öneriler hakkında daha fazla bilgi edinin.


### <a name="enhancements-to-sql-data-classification-recommendation"></a>SQL veri sınıflandırması önerisine yönelik geliştirmeler

**Değişikliğin tahmini tarihi:** S2 2021

**SQL veritabanlarınızdaki önerinin hassas verilerinin** geçerli sürümü, **veri sınıflandırması Uygula** güvenlik denetimi ' nde, Microsoft 'un veri sınıflandırması stratejisiyle daha iyi hizalanmış yeni bir sürümle değiştirilmelidir. Sonuç olarak:

- Öneri artık güvenli puanınızı etkilemeyecek
- Güvenlik denetimi ("veri sınıflandırmasını Uygula") artık güvenli puanınızı etkilemeyecek
- Önerinin KIMLIĞI de değişecektir (Şu anda b0df6f56-862D-4730-8597-38c0fd4ebd59)



## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).
