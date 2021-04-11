---
title: Azure Güvenlik Merkezi 'ne gelen önemli değişiklikler
description: Azure Güvenlik Merkezi 'Nde, planlamanız gerekebilecek ve planlamanız gerekebilen değişiklikler
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 04/04/2021
ms.author: memildin
ms.openlocfilehash: 78332e95d400348c2878748a92fd818451183ffb
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385879"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

| Planlı değişiklik                                                                                                                                                        | Değişikliğin tahmini tarihi |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| ["Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi](#two-recommendations-from-apply-system-updates-security-control-being-deprecated) | 2021 Nisan                |
| [21 güvenlik denetimleri arasında hareket eden öneriler](#21-recommendations-moving-between-security-controls)                                                           | 2021 Nisan                |
| ["Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı bırakılmakta olan iki ek öneri](#two-further-recommendations-from-apply-system-updates-security-control-being-deprecated)                                                                                         | 2021 Nisan                |
| [AWS 'nin önerileri genel kullanıma sunulacaktır (GA)](#recommendations-from-aws-will-be-released-for-general-availability-ga)                     | 2021 Nisan                |
| [SQL veri sınıflandırması önerisine yönelik geliştirmeler](#enhancements-to-sql-data-classification-recommendation)                                                     | S2 2021                   |
|                                                                                                                                                                       |                           |


### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>"Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı olmasının iki önerisi 

**Değişikliğin tahmini tarihi:** 2021 Nisan

Aşağıdaki iki öneri 2021 Nisan 'da kullanımdan kalkmak üzere zamanlandı:

- **Sistem güncelleştirmelerinin uygulanabilmesi için makinelerinizin yeniden başlatılması gerekiyor**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.
- **İzleme Aracısı makinelerinizde yüklü olmalıdır**. Bu öneri yalnızca şirket içi makinelerle ilgilidir ve mantığının bir kısmı başka bir öneriye aktarılacaktır. **Log Analytics aracı sistem durumu sorunları makinelerinizde çözümlenmelidir**. Bu, güvenli puanınızın küçük bir etkisi oluşmasına neden olur.

Bu önerilerin bunlara dahil edilip edilmeyeceğini görmek için sürekli dışarı aktarma ve iş akışı Otomasyonu yapılandırmalarının denetlenmesini öneririz. Ayrıca, bunları kullanıyor olabilecek panolar veya diğer izleme araçları da buna uygun şekilde güncelleştirilmeleri gerekir.

[Güvenlik önerileri başvuru sayfasında](recommendations-reference.md)bu öneriler hakkında daha fazla bilgi edinin.

### <a name="21-recommendations-moving-between-security-controls"></a>21 güvenlik denetimleri arasında hareket eden öneriler 

**Değişikliğin tahmini tarihi:** 2021 Nisan

Aşağıdaki öneriler farklı bir güvenlik denetimine taşınıyor. Güvenlik denetimleri, ilgili güvenlik önerilerinin mantıksal gruplarıdır ve savunmasız saldırı yüzeylerinizi yansıtır. Bu taşıma, Bu önerilerin her birinin amacını karşılamak için en uygun denetimde olmasını sağlar. 

Güvenlik denetimlerinde ve bunların önerilerinde her güvenlik denetiminde hangi önerilerin olduğunu öğrenin.

|Öneri |Değişiklik ve etki  |
|---------|---------|
|Güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir<br>SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir<br>SQL veritabanlarınızdaki güvenlik açıkları yeni bir kez düzeltilmelidir<br>VM 'lerdeki SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir     |Düzeltme açıklarına geçme (6 noktaya kadar)<br>Güvenlik yapılandırmalarının düzeltilmesi için (4 puntoya).<br>Ortamınıza bağlı olarak, Bu önerilerin puanınız üzerinde daha az etkisi olacaktır.|
|Aboneliğinize birden fazla sahip atanmalıdır<br>Otomasyon hesabı değişkenleri şifrelenmelidir<br>IoT cihazları-Auditd işlemi olayları göndermeyi durdurdu<br>IoT cihazları-Işletim sistemi taban çizgisi doğrulama hatası<br>IoT cihazları-TLS şifre paketi yükseltmesi gerekli<br>IoT cihazları-cihazda bağlantı noktalarını açma<br>IoT cihazları-zincirden birindeki Izin veren güvenlik duvarı ilkesi bulundu<br>IoT cihazları-giriş zincirindeki Izin veren güvenlik duvarı kuralı bulundu<br>IoT cihazları-çıkış zincirindeki Izin veren güvenlik duvarı kuralı bulundu<br>IoT Hub tanılama günlükleri etkinleştirilmelidir<br>IoT cihazları-aracı az kullanılan iletiler gönderiyor<br>IoT cihazları-varsayılan IP filtresi Ilkesi reddetme olmalıdır<br>IoT cihazları-IP filtre kuralı büyük IP aralığı<br>IoT cihazları-aracı ileti aralıkları ve boyutu ayarlanmalıdır<br>IoT cihazları-aynı kimlik doğrulama kimlik bilgileri<br>IoT cihazları-denetlenen işlem olay göndermeyi durdurdu<br>IoT cihazları-Işletim sistemi (OS) taban çizgisi yapılandırması düzeltilmelidir|**En iyi güvenlik uygulamalarını uygulamak** için taşınıyor.<br>Bir öneri en iyi güvenlik uygulamaları uygulama güvenlik denetimine geçerse, hiçbir puan değer vermez, öneri artık güvenli puanınızı etkilemeyecek.|
|||


### <a name="two-further-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>"Sistem güncelleştirmelerini Uygula" güvenlik denetiminin kullanım dışı bırakılmakta olan iki ek öneri

**Değişikliğin tahmini tarihi:** 2021 Nisan

Aşağıdaki iki öneri kullanım dışı bırakılmıştır:

- **Bulut hizmeti rolleriniz Için Işletim sistemi sürümü güncellenmelidir** . varsayılan olarak, Azure Konuk işletim sistemini, Windows Server 2016 gibi hizmet yapılandırmanızda (. cscfg) belirttiğiniz işletim sistemi ailesi içinde en son desteklenen görüntüye düzenli olarak güncelleştirir.
- **Kubernetes Hizmetleri, güvenlik açığı olmayan bir Kubernetes sürümüne yükseltilmelidir** . bu önerinin değerlendirmeleri, aralarında olmasını istediğimizde geniş kapsamlı değildir. Bu önerinin geçerli sürümü, sonunda müşterinin güvenlik gereksinimleriyle daha iyi hizalanan gelişmiş bir sürümle değiştirilmiştir.


### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>AWS 'nin önerileri genel kullanıma sunulacaktır (GA)

**Değişikliğin tahmini tarihi:** 2021 Nisan

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS güvenlik hub 'ından gelen öneriler, bulut bağlayıcıları tanıtıldığından önizleme aşamasındadır. **Önizleme** olarak işaretlenen öneriler, güvenli puanınızın hesaplamalarına dahil değildir, ancak yine de düzeltilmelidir, böylece önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunur.

Bu değişiklik ile, AWS önerilerinden oluşan iki küme GA 'ye taşınır:

- [Güvenlik Merkezi 'nin PCI DSS denetimleri](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [Güvenlik Merkezi 'nin CIS AWS temelleri kıyaslama denetimleri](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Bunlar GA ve değerlendirmeler AWS kaynaklarınız üzerinde çalıştırıldığında, sonuçlar tüm çok ve karma bulut kaynaklarınız için Birleşik güvenli puanınızı etkiler. 



### <a name="enhancements-to-sql-data-classification-recommendation"></a>SQL veri sınıflandırması önerisine yönelik geliştirmeler

**Değişikliğin tahmini tarihi:** S2 2021

**SQL veritabanlarınızdaki önerinin önemli verileri** , **veri sınıflandırması Uygula** güvenlik denetiminde sınıflandırılmalıdır ve Microsoft 'un veri sınıflandırması stratejisiyle daha iyi hizalanmış yeni bir sürümle değiştirilmelidir. Sonuç olarak, önerinin KIMLIĞI de değişecektir (Şu anda, b0df6f56-862D-4730-8597-38c0fd4ebd59).



## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).