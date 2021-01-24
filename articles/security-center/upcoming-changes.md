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
ms.date: 01/24/2021
ms.author: memildin
ms.openlocfilehash: 1b034c0f1c62eecf8139ed908a5a242060f3e886
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746569"
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
- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklendi](#35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark)


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

**SQL veritabanlarınızdaki önerinin hassas verilerinin** geçerli sürümü, **veri sınıflandırması Uygula** güvenlik denetimi kullanım dışı kalacak ve Microsoft 'un veri sınıflandırması stratejisiyle daha iyi hizalanmış yeni bir sürümle değiştirilmelidir. Sonuç olarak:

- Öneri artık güvenli puanınızı etkilemeyecek
- Güvenlik denetimi ("veri sınıflandırmasını Uygula") artık güvenli puanınızı etkilemeyecek
- Önerinin KIMLIĞI de değişecektir (Şu anda b0df6f56-862D-4730-8597-38c0fd4ebd59)


### <a name="35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark"></a>Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklenmiştir

**Değişikliğin tahmini tarihi:** Ocak 2021

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. [Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Bu kıyaslama kapsamını artırmak için aşağıdaki 35 önizleme önerileri güvenlik merkezi 'ne eklenecektir.

Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

| Güvenlik denetimi                     | Yeni öneriler                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bekleyen şifrelemeyi etkinleştir            | -Azure Cosmos DB hesapları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Azure Machine Learning çalışma alanları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-MySQL sunucuları için kendi anahtar veri korumanın etkinleştirilmesi gerekir<br>-PostgreSQL sunucuları için kendi anahtar veri korumanızı getir özelliği etkinleştirilmelidir<br>Bilişsel hizmetler hesapları, müşteri tarafından yönetilen bir anahtarla (CMK) veri şifrelemeyi etkinleştirmelidir<br>-Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-SQL yönetilen örnekler, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-SQL sunucuları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Depolama hesapları, şifreleme için müşteri tarafından yönetilen anahtar (CMK) kullanmalıdır                                                                                                                                                              |
| En iyi güvenlik uygulamalarını uygulayın    | -Abonelikler güvenlik sorunları için bir iletişim e-posta adresine sahip olmalıdır<br> -Log Analytics aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir<br> -Yüksek önem derecesine sahip uyarılar için e-posta bildirimi etkinleştirilmelidir<br> -Yüksek önem derecesi uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir<br> -Anahtar kasaları Temizleme koruması etkinleştirilmelidir<br> -Anahtar kasaları geçici silme etkin olmalıdır |
| Erişimi ve izinleri yönetme        | -Function uygulamalarında ' Istemci sertifikaları (gelen istemci sertifikaları) ' etkin olmalıdır |
| DDoS saldırılarına karşı uygulamaları koruma | -Web uygulaması güvenlik duvarı (WAF) Application Gateway için etkinleştirilmelidir<br> -Azure ön kapı hizmeti hizmeti için Web uygulaması güvenlik duvarı (WAF) etkinleştirilmelidir |
| Yetkisiz ağ erişimini kısıtla | -Güvenlik duvarının Key Vault üzerinde etkinleştirilmesi gerekir<br> -Özel uç nokta Key Vault için yapılandırılmalıdır<br> -Uygulama yapılandırması özel bağlantı kullanmalıdır<br> -Redsıs için Azure önbelleği bir sanal ağ içinde yer almalıdır<br> -Azure Event Grid etki alanları özel bağlantı kullanmalıdır<br> -Azure Event Grid konular özel bağlantı kullanmalıdır<br> -Azure Machine Learning çalışma alanları özel bağlantı kullanmalıdır<br> -Azure SignalR hizmeti özel bağlantı kullanmalıdır<br> -Azure yay bulutu, ağ ekleme kullanmalıdır<br> -Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor<br> -Kapsayıcı kayıt defterleri özel bağlantı kullanmalıdır<br> -MariaDB sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -MySQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -PostgreSQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -Depolama hesabı bir özel bağlantı bağlantısı kullanmalıdır<br> -Depolama hesapları, sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı<br> -VM görüntü Oluşturucu şablonlarının özel bağlantı kullanması gerekir|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)





## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).