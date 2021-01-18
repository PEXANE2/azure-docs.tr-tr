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
ms.date: 01/18/2021
ms.author: memildin
ms.openlocfilehash: ba9a640c2231c7098e58ad6e29bbfa196436a7f9
ms.sourcegitcommit: 61d2b2211f3cc18f1be203c1bc12068fc678b584
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98562327"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

- ["Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [SQL veri sınıflandırması önerisine yönelik geliştirmeler](#enhancements-to-sql-data-classification-recommendation)
- [Azure Ilke değerlendirmelerinde "uygun değil" kaynakları "uyumlu" olarak bildirilecek kaynaklar](#not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments)
- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklendi](#35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark)


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



### <a name="not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments"></a>Azure Ilke değerlendirmelerinde "uygun değil" kaynakları "uyumlu" olarak bildirilecek kaynaklar

**Değişikliğin tahmini tarihi:** Ocak 2021

Şu anda, bir öneri için değerlendirilen ve **uygulanamaz** olarak bulunan kaynaklar Azure ilkesinde "uyumsuz" olarak görünür. Kullanıcı eylemi, durumunu "uyumlu" olarak değiştirebilir. Bu planlı değişiklikten sonra, gelişmiş açıklık için "uyumlu" olarak bildirilir.

Azure Ilkesinde, uyumlu kaynakların sayısının arttıracağı tek etkisi görünür. Azure Güvenlik Merkezi 'nde güvenli puanınızın hiçbir etkisi olmayacaktır.

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