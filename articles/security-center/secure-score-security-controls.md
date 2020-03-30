---
title: Azure Güvenlik Merkezi'nde güvenli puan
description: Azure Güvenlik Merkezi'nin güvenli puanının ve güvenlik denetimlerinin açıklaması
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2020
ms.author: memildin
ms.openlocfilehash: d7eea9cd83e72b6ffeaae319a8e87c065015e6b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415818"
---
# <a name="enhanced-secure-score-preview-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde geliştirilmiş güvenli puan (önizleme)

## <a name="introduction-to-secure-score"></a>Güvenli skora giriş

Azure Güvenlik Merkezi'nin iki ana amacı vardır: Mevcut güvenlik durumunuzu anlamanıza yardımcı olmak ve güvenliğinizi verimli ve etkili bir şekilde geliştirmenize yardımcı olmak. Güvenlik Merkezi'nin bu hedeflere ulaşmanızı sağlayan merkezi yönü güvenli puandır.

Güvenlik Merkezi, kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu güvenlik sorunları için sürekli olarak değerlendirir. Daha sonra tüm bulguları tek bir skora toplar, böylece mevcut güvenlik durumunuzu bir bakışta anlayabilirsiniz: skor ne kadar yüksekse, tanımlanan risk düzeyi de o kadar düşük olur. Kuruluşunuzdaki güvenlik çabalarını ve projelerini izlemek için puanı kullanın. 

Güvenlik Merkezi'nin güvenli puan sayfası şunları içerir:

- **Skor** - Güvenli skor yüzde değeri olarak gösterilir, ancak temel değerler de açıktır:

    [![Temel sayılar da net bir yüzde değeri olarak gösterilen güvenli puan](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)

- **Güvenlik denetimleri** - Her denetim, ilgili güvenlik önerilerinden oluşan mantıksal bir gruptur ve savunmasız saldırı yüzeylerinizi yansıtır. Denetim, bu önerileri uygulamanıza yardımcı olan yönergeleri içeren bir güvenlik önerileri kümesidir. Puanınız yalnızca bir denetim içinde tek bir kaynak için *tüm* önerileri düzeltdiğinizde artar.

    Kuruluşunuzun her bir saldırı yüzeyini ne kadar iyi emniyete alabildiğini hemen görmek için, her güvenlik denetiminin puanlarını gözden geçirin.

    Daha fazla bilgi [için, güvenli skorun nasıl hesaplandığına](secure-score-security-controls.md#how-the-secure-score-is-calculated) bakın. 


>[!TIP]
> Güvenlik Merkezi'nin önceki sürümlerinde tavsiye düzeyinde puan lar verilir: tek bir kaynak için bir öneri düzeltdiğinizde, güvenli puanınız düzeldi. Bugün, puanınız yalnızca bir denetim içinde tek bir kaynak için *tüm* önerileri düzeltirseniz artar. Böylece puanınız yalnızca bir kaynağın güvenliğini artırdığınızda artar.
> Bu gelişmiş sürüm hala önizlemede yken, önceki güvenli puan deneyimi Azure Portalı'ndan bir seçenek olarak kullanılabilir. 


## <a name="locating-your-secure-score"></a>Güvenli puanınızı bulma

Güvenlik Merkezi puanınızı belirgin bir şekilde görüntüler: Genel Bakış sayfasında gösterilen ilk şeydir. Özel güvenli puan sayfasına tıklarsanız, aboneliğin bölündüğünü görürsünüz. Öncelik verilen önerilerin ayrıntılı listesini ve bunları düzelten önerilerin aboneliğin puanı üzerindeki olası etkisini görmek için tek bir aboneliği tıklatın.

## <a name="how-the-secure-score-is-calculated"></a>Güvenli puan nasıl hesaplanır? 

Her güvenlik denetiminin genel güvenli skora katkısı öneriler sayfasında açıkça gösterilir.

[![Geliştirilmiş güvenli skor güvenlik denetimleri sunar](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Bir güvenlik denetimi için mümkün olan tüm noktaları elde etmek için tüm kaynaklarınız güvenlik denetimindeki tüm güvenlik önerilerine uymalıdır. Örneğin, Güvenlik Merkezi'nin yönetim bağlantı noktalarınızın güvenliğini sağlama yla ilgili birden çok önerisi vardır. Geçmişte, diğerlerini çözülmemiş bırakarak bu ilgili ve birbirine bağlı önerilerden bazılarını düzeltebilirsiniz ve güvenli puanınız düzelebilir. Objektif olarak bakıldığında, tüm bunları çözene kadar güvenliğinizin gelişmediğini iddia etmek kolaydır. Şimdi, güvenli puanı için bir fark yaratmak için hepsini düzeltmek gerekir.

Örneğin, "Sistem güncelleştirmelerini uygulayın" adlı güvenlik denetimi, denetimin olası artış değeri üzerinde araç ipucunda görebileceğiniz en fazla altı puana sahiptir:

[![Güvenlik denetimi "Sistem güncelleştirmelerini uygulayın"](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Yukarıdaki ekran görüntüsünde güvenlik denetimi "Sistem güncelleştirmelerini uygulayın" potansiyeli "%2 (1 Puan)" gösterir. Bu, bu denetimdeki tüm önerileri düzeltirseniz, puanınızın %2 artacağı anlamına gelir (bu durumda, bir puan). Basitlik için, öneriler listesinin "Potansiyel artış" sütunundaki değerler tam sayılara yuvarlanır. Araç uçları kesin değerleri gösterir:

* **Maksimum puan** - Bir kontrol içinde tüm önerileri tamamlayarak kazanabileceğiniz maksimum puan sayısı. Bir denetim için maksimum puan, bu denetimin göreli önemini gösterir. Önce hangi konularda çalışmak için triyaj için maksimum puan değerlerini kullanın. 
* **Potansiyel artış** - Kontrol içinde kullanabileceğiniz kalan noktalar. Bu puanların güvenli puanınıza eklenmesini sağlamak için, denetimin tüm önerilerini düzeltin. Yukarıdaki örnekte, denetim için gösterilen bir nokta aslında 0,96 puandır.
* **Geçerli puan** - Bu denetim için geçerli puan. Her kontrol toplam puana katkıda bulunur. Bu örnekte, denetim toplama 5,04 puan katkıda bulunuyor. 

### <a name="calculations---understanding-your-score"></a>Hesaplamalar - puanınızı anlamak

|Ölçüm|Formül ve örnek|
|-|-|
|**Güvenlik denetiminin geçerli puanı**|<br>![Bir güvenlik denetiminin geçerli puanını hesaplamak için denklem](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>Her bir güvenlik denetimi Güvenlik Puanı'na katkıda bulunur. Denetim içinde bir öneri etkilenen her kaynak, denetimin geçerli puanı doğru katkıda bulunur. Her denetim için geçerli puan, denetim *içindeki* kaynakların durumunun bir ölçüsüdür.<br>![Güvenlik denetiminin geçerli puanı hesaplanırken kullanılan değerleri gösteren araç ipuçları](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>Bu örnekte, 6'nın en yüksek puanı 78'e bölünür, çünkü bu sağlıklı ve sağlıksız kaynakların toplamıdır.<br>6 / 78 = 0,0769<br>Bunu, geçerli puandaki sağlıklı kaynak sayısı (4) ile çarpmak:<br>0,0769 * 4 = **0,31**<br><br>|
|**Güvenli skor**<br>Tek abonelik|<br>![Geçerli güvenli puanı hesaplamak için denklem](media/secure-score-security-controls/secure-score-equation.png)<br><br>![Tüm denetimler etkinleştirilmiş tek abonelik güvenli puanı](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>Bu örnekte, tüm güvenlik denetimleri (60 puan potansiyel maksimum puan) ile tek bir abonelik vardır. Puan, olası 60 üzerinden 28 puan gösterirken, kalan 32 puan güvenlik kontrollerinin "Potansiyel puan artışı" rakamlarına yansıtılır.<br>![Kontroller listesi ve potansiyel puan artışı](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Güvenli skor**<br>Birden çok abonelik|<br>Tüm abonelikler genelindeki tüm kaynakların geçerli puanı eklenir ve hesaplama daha sonra tek bir abonelik için aynıdır<br><br>Birden çok aboneliği görüntülerken, güvenli puan tüm etkin ilkelerdeki tüm kaynakları değerlendirir ve bunların her güvenlik denetiminin maksimum puanı üzerindeki birleşik etkilerini gruplatır.<br>![Tüm denetimler etkinken birden çok abonelik için güvenli puan](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Kombine puan ortalama **değildir;** daha ziyade tüm abonelikler arasında tüm kaynakların durumunun değerlendirilmiş duruşu.<br>Burada da, öneriler sayfasına gider ve mevcut potansiyel puanları toplarsanız, bunun geçerli puan (24) ile mevcut maksimum puan (60) arasındaki fark olduğunu göreceksiniz.|
||||

## <a name="improving-your-secure-score"></a>Güvenli puanınızı artırma

Güvenli puanınızı artırmak için, öneriler listenizdeki güvenlik önerilerini düzeltin. Her bir kaynağı el ile düzeltebilirsiniz, ya da **Hızlı Düzeltme kullanarak!** bir öneri için bir düzeltmeyi hızlı bir şekilde bir kaynak grubuna uygulamak için (varsa) seçeneğini belirleyin. Daha fazla bilgi için [bkz.](security-center-remediate-recommendations.md)

>[!IMPORTANT]
> Yalnızca yerleşik önerilerin güvenli skor üzerinde etkisi olur.

## <a name="security-controls-and-their-recommendations"></a>Güvenlik denetimleri ve önerileri

Aşağıdaki tabloda Azure Güvenlik Merkezi'ndeki güvenlik denetimleri listelanmaktadır. Her denetim için, *tüm* kaynaklarınız için denetimde listelenen *tüm* önerileri düzeltirseniz, güvenli puanınıza ekleyebileceğiniz maksimum puan sayısını görebilirsiniz. 

> [!TIP]
> Bu listeyi farklı filtrelemek veya sıralamak istiyorsanız, kopyalayıp Excel'e yapıştırın.

|Güvenlik kontrolü|Maksimum güvenli puan|Öneriler|
|----------------|:-------------------:|---------------|
|**MFA’yı etkinleştirme**|10|- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir<br>- MFA, aboneliğinizde okuma izni olan hesaplarda etkinleştirilmelidir<br>- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmeli|
|**Güvenli yönetim bağlantı noktaları**|8|- Sanal makinelerde Tam Zamanında ağ erişim kontrolü uygulanmalıdır<br>- Sanal makineler bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir<br>- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır|
|**Sistem güncelleştirmelerini uygulama**|6|- İzleme aracısı sağlık sorunları makinelerinizde çözülmelidir<br>- İzleme aracısı sanal makine ölçek kümeleri üzerine kurulmalıdır<br>- İzleme aracı makinelerinize monte edilmelidir<br>- Bulut hizmeti rolleriniz için işletim sistemi sürümü güncellenmelidir<br>- Sanal makine ölçek kümelerinde sistem güncellemeleri kurulmalıdır<br>- Sistem güncellemeleri makinelerinize yüklenmeli<br>- Sistem güncellemelerini uygulamak için makineleriniz yeniden başlatılmalıdır<br>- Kubernetes Hizmetleri savunmasız olmayan bir Kubernetes sürümüne yükseltilmelidir<br>- İzleme aracısanal makinelerinize kurulmalıdır|
|**Güvenlik açıklarını düzeltin**|6|- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir<br>- Azure Konteyner Kayıt Defteri görüntülerindeki güvenlik açıkları düzeltilmelidir<br>- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir<br>- Güvenlik açıkları bir Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir<br>- SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir<br>- SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir<br>- Güvenlik açığı değerlendirme çözümü sanal makinelerinize yüklenmelidir|
|**Şifrelemeyi istirahatte etkinleştirme**|4|- Disk şifreleme sanal makinelere uygulanmalıdır<br>- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir<br>- Otomasyon hesabı değişkenleri şifrelenmelidir<br>- Service Fabric kümeleri ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlamalıdır<br>- SQL server TDE koruyucusu kendi anahtarınızla şifrelenmelidir|
|**Aktarımsırasında verileri şifreleme**|4|- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilmelidir<br>- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir<br>- Depolama hesaplarına güvenli transfer etkinleştirilmelidir<br>- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilmelidir|
|**Erişimi ve izinleri yönetme**|4|- Aboneliğiniz için en fazla 3 tesis belirlenmelidir<br>- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır (Önizleme)<br>- Sahibi izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır (Önizleme)<br>- Sahibi izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır (Önizleme)<br>- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır<br>- Yazma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır (Önizleme)<br>- Aboneliğinize birden fazla sahip atanmalıdır<br>- Kubernetes Hizmetlerinde Rol Tabanlı Erişim Kontrolü (RBAC) kullanılmalıdır (Önizleme)<br>- Service Fabric kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır|
|**Güvenlik yapılandırmalarını düzeltme**|4|- Pod Güvenlik Politikaları Kubernetes Hizmetlerinde Tanımlanmalıdır<br>- Konteyner güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir<br>- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>- İzleme aracısanal makinelerinize kurulmalıdır<br>- İzleme aracı makinelerinize monte edilmelidir<br>- İzleme aracısı sanal makine ölçek kümeleri üzerine kurulmalıdır<br>- İzleme aracısı sağlık sorunları makinelerinizde çözülmelidir|
|**Yetkisiz ağ erişimini kısıtlama**|4|- Sanal makinenizdeki IP yönlendirmedevre dışı bırakılmalı<br>- Yetkili IP aralıkları Kubernetes Hizmetlerinde tanımlanmalıdır (Önizleme)<br>- (DEPRECATED) Uygulama Hizmetlerine erişim kısıtlanmalıdır (Önizleme)<br>- (DEPRECATED) IaaS NSG'lerde web uygulamaları için kurallar sertleştirilmiş olmalıdır<br>- Sanal makineler bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir<br>- CORS her kaynağın API Uygulamanıza erişmesine izin vermemelidir<br>- CORS her kaynağın İşlev Uygulamanıza erişmesine izin vermemelidir<br>- CORS her kaynağın Web Uygulamanıza erişmesine izin vermemelidir<br>- API Uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- Fonksiyon Uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- Web Uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- İnternete bakan VM'lere sahip izin verilen Ağ Güvenlik Grupları için erişim kısıtlanmalıdır<br>- İnternete bakan sanal makineler için Ağ Güvenliği Grup Kuralları sertleştirilmiş olmalıdır|
|**Uyarlanabilir uygulama denetimi uygulayın**|3|- Sanal makinelerde Adaptif Uygulama Kontrolleri etkinleştirilmelidir<br>- İzleme aracısanal makinelerinize kurulmalıdır<br>- İzleme aracı makinelerinize monte edilmelidir<br>- İzleme aracısı sağlık sorunları makinelerinizde çözülmelidir|
|**Veri sınıflandırması uygulama**|2|- SQL veritabanlarınızdaki hassas veriler sınıflandırılmalıdır (Önizleme)|
|**Uygulamaları DDoS saldırılarına karşı koruyun**|2|- DDoS Koruma Standardı etkinleştirilmelidir|
|**Uç nokta korumayı etkinleştirme**|2|- Uç nokta koruma sistem durumu hataları sanal makine ölçeği kümelerinde düzeltilmelidir<br>- Makinelerinizde uç nokta koruma sağlık sorunları çözülmelidir<br>- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır<br>- Sanal makinelere uç nokta koruma çözümü yükleyin<br>- İzleme aracısı sağlık sorunları makinelerinizde çözülmelidir<br>- İzleme aracısı sanal makine ölçek kümeleri üzerine kurulmalıdır<br>- İzleme aracı makinelerinize monte edilmelidir<br>- İzleme aracısanal makinelerinize kurulmalıdır<br>- Makinelerinize uç nokta koruma çözümü yükleyin|
|**Denetleme ve günlüğe kaydetmeyi etkinleştirme**|1|- SQL sunucusunda denetim etkinleştirilmelidir<br>- Uygulama Hizmetlerinde tanılama günlükleri etkinleştirilmelidir<br>- Azure Veri Gölü Deposu'ndaki tanılama günlükleri etkinleştirilmelidir<br>- Azure Akış Analizi'ndeki tanılama günlükleri etkinleştirilmelidir<br>- Toplu hesaplardaki tanılama günlükleri etkinleştirilmelidir<br>- Data Lake Analytics'teki tanılama günlükleri etkinleştirilmelidir<br>- Event Hub'daki tanılama günlükleri etkinleştirilmeli<br>- IoT Hub'daki tanılama günlükleri etkinleştirilmelidir<br>- Key Vault'taki tanılama günlükleri etkinleştirilmelidir<br>- Mantık Uygulamaları'ndaki tanılama günlükleri etkinleştirilmelidir<br>- Arama hizmetindeki tanılama günlükleri etkinleştirilmelidir<br>- Servis Veri Tos'undaki tanı günlükleri etkinleştirilmeli<br>- Sanal Makine Ölçek Kümelerinde tanılama günlükleri etkinleştirilmelidir<br>- Toplu hesaplarda metrik uyarı kuralları yapılandırılmalıdır<br>- SQL Denetim ayarları, kritik etkinlikleri yakalamak için Eylem Grupları yapılandırıldı<br>- SQL sunucuları 90 günden uzun denetim bekletme günleriyle yapılandırılmalıdır.|
|**Güvenlik en iyi uygulamalarını uygulayın**|0|- Güvenlik duvarı ve sanal ağ yapılandırmaları içeren depolama hesaplarına erişim kısıtlanmalıdır<br>- RootManageSharedAccessKey dışındaki tüm yetkilendirme kuralları Event Hub ad alanından kaldırılmalıdır<br>- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır<br>- Olay Hub örneğindeki yetkilendirme kuralları tanımlanmalıdır<br>- Depolama hesapları yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir<br>- Sanal makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir<br>- SQL sunucusu için gelişmiş veri güvenliği ayarları güvenlik uyarıları almak için bir e-posta adresi içermelidir<br>- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir<br>- Tüm gelişmiş tehdit koruma türleri SQL yönetilen örnek gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir<br>- Yöneticilere ve abonelik sahiplerine e-posta bildirimleri SQL sunucusu gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir<br>- Gelişmiş Tehdit Koruması türleri SQL sunucusu Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır<br>- Alt ağlar bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir<br>- Tüm gelişmiş tehdit koruma türleri SQL sunucusu gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir|
||||

## <a name="secure-score-faq"></a>Güvenli puan SSS

### <a name="why-has-my-secure-score-gone-down"></a>Güvenli puanım neden düştü?
Güvenlik Merkezi, skorun hesaplanma şeklindeki değişiklikleri içeren gelişmiş bir güvenli puana (şu anda önizleme durumunda) geçti. Şimdi, puan almak için bir kaynak için tüm önerileri çözmek gerekir. Skorlar da 0-10 ölçeğine değiştirildi.

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Güvenlik denetiminde dört öneriden yalnızca üçünü ele alırsam, güvenli puanım değişir mi?
Hayır. Tek bir kaynak için tüm önerileri düzeltene kadar bu durum değişmez. Bir denetim için maksimum puanı almak için, tüm kaynaklar için tüm önerileri düzeltmelisiniz.

### <a name="is-the-previous-experience-of-the-secure-score-still-available"></a>Güvenli skorun önceki deneyimi hala mevcut mu? 
Evet. Bir süre geçişi kolaylaştırmak için yan yana koşacaklar. Önceki modelin zaman içinde aşamalı olarak durdurulmasını bekleyin. 

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Bir tavsiye benim için geçerli değilse ve ben politikada devre dışı kalırsa, güvenlik denetimim yerine getirilecek ve güvenli puanım güncellenecek mi?
Evet. Çevrenizde uygun olmadığında önerileri devre dışı bırakmanızı öneririz. Belirli bir öneriyi nasıl devre dışı dışı kakacağınız hakkındaki [yönergeler](https://docs.microsoft.com/azure/security-center/tutorial-security-policy#disable-security-policies)için bkz.

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Bir güvenlik kontrolü bana güvenli puanıma karşı sıfır puan teklif ederse, bunu görmezden mi gelmeliyim?
Bazı durumlarda, denetim maksimum puanısıfırdan büyük görürsünüz, ancak etkisi sıfırdır. Kaynakları sabitleme için artımlı puan önemsiz olduğunda, sıfıra yuvarlanır. Güvenlik iyileştirmeleri getirdiği için bu önerileri göz ardı etmeyin. Tek istisna "Ek En İyi Uygulama" denetimidir. Bu önerileri düzeltmek puanınızı artırmaz, ancak genel güvenliğinizi artırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenli puan ve tanıttı güvenlik denetimleri açıklanmıştır. İlgili materyaller için aşağıdaki makalelere bakın:

- [Bir önerinin farklı öğeleri hakkında bilgi edinin](security-center-recommendations.md)
- [Önerileri nasıl düzeltiriz öğrenin](security-center-remediate-recommendations.md)