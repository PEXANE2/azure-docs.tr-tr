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
ms.date: 03/04/2021
ms.author: memildin
ms.openlocfilehash: 6bec9f0a1c22691d818566cec3f59c1ec0f3d3bb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051625"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

- [AWS 'nin önerileri genel kullanıma sunulacaktır (GA)](#recommendations-from-aws-will-be-released-for-general-availability-ga)
- [İki eski öneri artık doğrudan Azure etkinlik günlüğüne veri yazmayacaktır](#two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log)
- ["Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [SQL veri sınıflandırması önerisine yönelik geliştirmeler](#enhancements-to-sql-data-classification-recommendation)
- [11 Azure Defender uyarılarının kullanımdan kaldırılması](#deprecation-of-11-azure-defender-alerts)


### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>AWS 'nin önerileri genel kullanıma sunulacaktır (GA)

**Değişikliğin tahmini tarihi:** 2021 Nisan

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS güvenlik hub 'ından gelen öneriler, bulut bağlayıcıları tanıtıldığından önizleme aşamasındadır. **Önizleme** olarak işaretlenen öneriler, güvenli puanınızın hesaplamalarına dahil değildir, ancak yine de düzeltilmelidir, böylece önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunur.

Bu değişiklik ile, AWS önerilerinden oluşan iki küme GA 'ye taşınır:

- [Güvenlik Merkezi 'nin PCI DSS denetimleri](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [Güvenlik Merkezi 'nin CIS AWS temelleri kıyaslama denetimleri](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Bunlar GA ve değerlendirmeler AWS kaynaklarınız üzerinde çalıştırıldığında, sonuçlar tüm çok ve karma bulut kaynaklarınız için Birleşik güvenli puanınızı etkiler. 



### <a name="two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log"></a>İki eski öneri artık doğrudan Azure etkinlik günlüğüne veri yazmayacaktır 

**Değişikliğin tahmini tarihi:** Mart 2021

Güvenlik Merkezi, Azure Advisor 'a neredeyse tüm güvenlik önerileri için verileri geçirir, bu da [Azure etkinlik günlüğüne](../azure-monitor/essentials/activity-log.md)yazar.

İki öneri için, veriler eşzamanlı olarak doğrudan Azure etkinlik günlüğüne yazılır. Bu değişiklik ile, güvenlik merkezi bu eski güvenlik önerileri için verileri doğrudan etkinlik günlüğüne yazmayı durdurur. Bunun yerine, diğer tüm önerilerin yaptığımız gibi verileri Azure Advisor 'a dışarı aktaracağız. 

Bu iki eski öneri şunlardır:
- Endpoint Protection sistem durumu sorunları makinelerinizde çözümlenmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

Bu iki öneriye yönelik bilgilere, Etkinlik günlüğünün "TaskDiscovery türü önerisi" kategorisinin bilgilerine erişiyorsanız, artık kullanılamaz.

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>"Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi 

**Değişikliğin tahmini tarihi:** Mart 2021

Aşağıdaki iki öneri Şubat 2021 ' de kullanımdan kalkmak üzere zamanlandı:

- **Sistem güncelleştirmelerinin uygulanabilmesi için makinelerinizin yeniden başlatılması gerekiyor**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.
- **İzleme Aracısı makinelerinizde yüklü olmalıdır**. Bu öneri yalnızca şirket içi makinelerle ilgilidir ve mantığının bir kısmı başka bir öneriye aktarılacaktır. **Log Analytics aracı sistem durumu sorunları makinelerinizde çözümlenmelidir**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.

Bu önerilerin bunlara dahil edilip edilmeyeceğini görmek için sürekli dışarı aktarma ve iş akışı Otomasyonu yapılandırmalarının denetlenmesini öneririz. Ayrıca, bunları kullanıyor olabilecek panolar veya diğer izleme araçları da buna uygun şekilde güncelleştirilmeleri gerekir.

[Güvenlik önerileri başvuru sayfasında](recommendations-reference.md)bu öneriler hakkında daha fazla bilgi edinin.


### <a name="enhancements-to-sql-data-classification-recommendation"></a>SQL veri sınıflandırması önerisine yönelik geliştirmeler

**Değişikliğin tahmini tarihi:** S2 2021

**SQL veritabanlarınızdaki önerinin önemli verileri** , **veri sınıflandırması Uygula** güvenlik denetiminde sınıflandırılmalıdır ve Microsoft 'un veri sınıflandırması stratejisiyle daha iyi hizalanmış yeni bir sürümle değiştirilmelidir. Sonuç olarak, önerinin KIMLIĞI de değişecektir (Şu anda, b0df6f56-862D-4730-8597-38c0fd4ebd59).


### <a name="deprecation-of-11-azure-defender-alerts"></a>11 Azure Defender uyarılarının kullanımdan kaldırılması

**Değişikliğin tahmini tarihi:** Mart 2021

Sonraki ay, aşağıda listelenen on bir Azure Defender uyarısı kullanım dışı olacaktır.

- Yeni uyarılar, bu iki uyarıyı değiştirecek ve daha iyi kapsam sağlayacak:

    | AlertType                | AlertDisplayName                                                         |
    |--------------------------|--------------------------------------------------------------------------|
    | ARM_MicroBurstDomainInfo | PREVIEW-mikro patlama araç seti "Get-AzureDomainInfo" işlev çalıştırması algılandı |
    | ARM_MicroBurstRunbook    | PREVIEW-mikro patlama araç seti "Get-AzurePasswords" işlev çalıştırması algılandı  |
    |                          |                                                                          |

- Bu dokuz uyarı, zaten kullanım dışı bırakılmış bir Azure Active Directory Kimlik Koruması Bağlayıcısı ile ilgilidir:

    | AlertType           | AlertDisplayName              |
    |---------------------|-------------------------------|
    | UnfamiliarLocation  | Bilinmeyen oturum açma özellikleri |
    | AnonymousLogin      | Anonim IP adresi          |
    | Bulaşıp Teddevicelogin | Kötü amaçlı yazılım bağlı IP adresi     |
    | Impossıbleseyahat    | Olağandışı yolculuk               |
    | MaliciousIP         | Kötü amaçlı IP adresi          |
    | LeakedCredentials   | Sızdırılan kimlik bilgileri            |
    | Passwordpüskürtme       | Parola spreyi                |
    | LeakedCredentials   | Azure AD tehdit bilgileri  |
    | AADAı               | Azure AD AI                   |
    |                     |                               |
 



## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).
