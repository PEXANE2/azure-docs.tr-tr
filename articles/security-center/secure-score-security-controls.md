---
title: Azure Güvenlik Merkezi 'nde gelişmiş güvenli skor (Önizleme)
description: Azure Güvenlik Merkezi 'nde gelişmiş güvenli skor (Önizleme) ve güvenlik denetimlerinin açıklaması
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
ms.date: 12/04/2019
ms.author: memildin
ms.openlocfilehash: f4f6cf01502070ea63eaf0083aba33ff213534a4
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500241"
---
# <a name="the-enhanced-secure-score-preview"></a>Gelişmiş güvenli puan (Önizleme) 

Bu makalede gelişmiş güvenli puan (Şu anda önizleme aşamasında), ilgili güvenlik denetimleri ve getirdikleri avantajlar tanıtılmaktadır. Puanınızın nasıl hesaplanacağını da açıklar.

## <a name="introduction-to-secure-score"></a>Güvenli puana giriş

Azure Güvenlik Merkezi 'nin iki ana hedefi vardır: geçerli güvenlik durumunuzu anlamanıza ve güvenliği verimli ve etkili bir şekilde iyileştirmenize yardımcı olmak için. Güvenlik Merkezi 'nin bu hedeflere ulaşmanızı sağlayan merkezi bir yönü güvenli puanı.

Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır. Kuruluşunuzdaki güvenlik çalışmalarını ve projelerini izlemek için puanı kullanın. 

*Gelişmiş* güvenli puan (Şu anda önizleme aşamasında) **saldırı yüzeyi odaklı** ve üç avantaj sunar:

- **Güvenlik denetimleri** -güvenlik önerileri artık, savunmasız saldırı yüzeylerinizi daha iyi yansıtan mantıksal kümeler halinde gruplandırılır. Daha fazla bilgi için bkz. [güvenli Puanlama aşağıda nasıl hesaplanır](secure-score-security-controls.md#how-the-secure-score-is-calculated) .

- **Genel puan, genel** geri yükleme noktalarının öneri düzeyinde daha iyi olduğunu yansıtır. Bu geliştirme sayesinde puanınız yalnızca bir denetimdeki tek bir kaynak için *Tüm* önerileri düzelttireceğiniz zaman iyileştirecektir. Bu, puanınızın yalnızca bir kaynağın güvenliği artdığı zaman gelişmediği anlamına gelir. 

- **Tek tek saldırı yüzeylerinin güvenlik durumu daha görünür** -güvenlik denetimi başına puanı göstererek, güvenli puan sayfası, kuruluşunuzun her bir saldırı yüzeyini ne kadar iyi güvenlik altına alarak ayrıntılı bir görünüm elde edebilirsiniz.

Gelişmiş güvenli puan, aşağıdaki ekran görüntüsünde gösterildiği gibi yüzde olarak gösterilir:

[Gelişmiş güvenli skor (Önizleme) ![artık bir yüzde içerir](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)


## <a name="locating-your-secure-score"></a>Güvenli puanınızı bulma

Güvenlik Merkezi puanınızı göze çarpacak şekilde görüntüler: Genel Bakış sayfasında gösterilen ilk şey. Adanmış güvenli puan sayfasına tıkladığınızda, puanı aboneliğe göre aşağı doğru görürsünüz. Önceliklendirilmiş önerilerin ayrıntılı listesini görmek için tek bir aboneliğe tıklayın ve bu bilgilerin, aboneliğin puanına ait olduğunu belirten olası etkileri görebilirsiniz.

## <a name="how-the-secure-score-is-calculated"></a>Güvenli puan nasıl hesaplanır 

Bu önizleme öncesinde, güvenlik merkezi her öneriyi tek tek kabul edilir ve önem derecesine göre bu değere atanır. Güvenlik duruşunu geliştirmek için çalışan güvenlik ekipleri, bulgularının tam listesini temel alarak Güvenlik Merkezi önerilerine verilen yanıtların önceliğini belirleme gerekiyordu. Tek bir kaynak için bir öneriyi her başlattığınızda, güvenli puanınız geliştirilmiştir.

Güvenli puanın geliştirmelerinin bir parçası olarak, öneriler artık **güvenlik denetimleriyle**gruplandırılır. Denetim, bu önerileri uygulamanıza yardımcı olan güvenlik önerileri ve yönergeleri kümesidir. Denetimler, ilgili önerilerin mantıksal gruplandırmalarıdır. Noktalara artık öneri düzeyi verilmez. Bunun yerine, puanınız yalnızca bir denetim içindeki tek bir kaynak için *Tüm* önerileri düzelttireceğiniz zaman geliştirecek.

Her güvenlik denetiminin genel güvenli puana doğru katkısı, öneriler sayfasında net bir şekilde gösterilir.

[Gelişmiş güvenli puanı (Önizleme) ![güvenlik denetimlerini tanıtır](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Bir güvenlik denetimine yönelik tüm olası noktaları almak için, tüm kaynaklarınızın güvenlik denetimindeki tüm güvenlik önerilerine uyması gerekir. Örneğin, güvenlik merkezi 'nin Yönetim bağlantı noktalarınızı güvenli hale getirmeye yönelik birden çok önerisi vardır. Geçmişte, diğer sorunları çözbırakırken ve güvenli puanınızın iyileştirecağından ilgili ve birbirine bağlı önerilerin bazılarını düzeltebilirsiniz. Objecas 'e baktığı zaman, güvenlerinizin tümünü çözümlene kadar iyileştirildiğinden emin olmak çok kolay. Şimdi, güvenli puanınız için fark yapmak üzere tümünü düzeltmeniz gerekir.

Örneğin, "sistem güncelleştirmelerini Uygula" adlı güvenlik denetiminde en fazla altı puan bulunur ve bu, denetimin olası artış değerindeki araç ipucunda görebileceğiniz şekilde:

["sistem güncelleştirmelerini Uygula" güvenlik denetimini ![](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Yukarıdaki ekran görüntüsünde "sistem güncelleştirmelerini Uygula" güvenlik denetimi potansiyeli "2% (1 Point)" gösterir. Bu, bu denetimdeki tüm önerileri düzeltmeniz durumunda puanınızın %2 oranında (Bu durumda bir nokta) artıralacağını gösterir. Basitlik için, öneriler listesinin "olası artışı" sütunundaki değerler, tam sayılara yuvarlanır. Araç ipuçları kesin değerleri gösterir:

* **Maksimum puan** -denetim içindeki tüm önerileri tamamlayarak elde edebilmeniz gereken en yüksek puan sayısıdır. Bir denetim için maksimum puan, bu denetimin göreli anlam olduğunu gösterir. İlk olarak hangi sorunların çalıştığını önceliklendirme için maksimum puan değerlerini kullanın. 
* **Olası artış** -denetim içinde sizin için kullanılabilir olan diğer noktaları. Bu noktaları güvenli puanlarınıza eklemek için tüm denetim önerilerini düzeltin. Yukarıdaki örnekte, denetim için gösterilen bir nokta aslında 0,96 puntodur.
* **Geçerli puan** -bu denetimin geçerli puanı. Her denetim toplam puanta katkıda bulunur. Bu örnekte denetim, toplamda 5,04 noktaya katkıda bulunur. 

### <a name="calculations---understanding-your-score"></a>Hesaplamalar-puanınızı anlama

|Ölçüm|Formül ve örnek|
|-|-|
|**Güvenlik denetiminin geçerli puanı**|<br>Güvenlik denetiminin geçerli Puanını hesaplamak için ![denklemi](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>Her bireysel güvenlik denetimi güvenlik puanına katkıda bulunur. Denetim içindeki bir öneriden etkilenen her kaynak, denetimin geçerli puanına doğru katkıda bulunur. Her denetim için geçerli *puan, denetimdeki kaynakların durumunun* ölçüsüdür.<br>Güvenlik denetiminin geçerli puanı hesaplanırken kullanılan değerleri gösteren araç Ipuçları ![](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>Bu örnekte, sağlıklı ve sağlıksız kaynakların toplamı olduğundan, 6 ' nın en fazla puanı 78 olarak bölünür.<br>6/78 = 0,0769<br>Sağlıklı kaynak sayısına (4) göre çarpılması geçerli puanın sonucunu elde ediyor:<br>0,0769 * 4 = **0,31**<br><br>|
|**Güvenlik Puanı**<br>Tek abonelik|<br>![Geçerli güvenli puanı hesaplama denklemi](media/secure-score-security-controls/secure-score-equation.png)<br><br>![Tüm denetimler etkin olan tek abonelik güvenli puanı](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>Bu örnekte, kullanılabilen tüm güvenlik denetimlerine sahip tek bir abonelik vardır (en fazla 60 punto puanı). Puan, olası bir 60 28 noktayı gösterir ve kalan 32 noktaları, güvenlik denetimlerinin "potansiyel puan artışı" biçiminde yansıtılır.<br>![Denetimlerin listesi ve olası puan artışı](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Güvenlik Puanı**<br>Birden çok abonelik|<br>Tüm aboneliklerdeki tüm kaynakların geçerli puanı eklenir ve hesaplama daha sonra tek bir abonelikle aynı olur<br><br>Birden çok abonelik görüntülenirken, güvenli puan tüm etkin ilkelerin içindeki tüm kaynakları değerlendirir ve her bir güvenlik denetiminin en fazla puanı üzerinde Birleşik etkileri gruplandırır.<br>Tüm denetimler etkin olan birden çok abonelik için güvenli Puanlama ![](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Birleşik puan bir ortalama **değil** ; Bunun yerine, tüm aboneliklerdeki tüm kaynakların durumunun değerlendirilmiş bir şekilde olduğunu gösterir.<br>Burada, öneriler sayfasına giderseniz ve kullanılabilir olası noktaları eklerseniz, geçerli puan (24) ile kullanılabilir maksimum puan (60) arasındaki fark olduğunu göreceksiniz.|
||||

## <a name="improving-your-secure-score"></a>Güvenli puanınızı iyileştirme

Güvenli puanınızı iyileştirmek için, öneriler listenizden güvenlik önerilerini düzeltin. Her bir kaynak için veya **hızlı düzeltme** kullanarak her öneriyi el ile düzeltebilirsiniz! bir kaynak grubuna hızlıca bir öneri için düzeltme uygulamak üzere (kullanılabilir olduğunda) seçeneği. Daha fazla bilgi için bkz. [önerileri](security-center-remediate-recommendations.md)düzeltme.

Yalnızca yerleşik önerilerin güvenli puanı üzerinde etkisi vardır.

## <a name="security-controls-and-their-recommendations"></a>Güvenlik denetimleri ve önerileri

Aşağıdaki tabloda, Azure Güvenlik Merkezi 'ndeki güvenlik denetimleri listelenmiştir. Her denetim için, denetimde listelenen *Tüm* önerileri, *Tüm* kaynaklarınız Için düzeltmeniz durumunda, güvenli puanlarınıza ekleyebileceğiniz en fazla puan sayısını görebilirsiniz. 

> [!TIP]
> Bu listeyi farklı filtrelemek veya sıralamak isterseniz Excel 'e kopyalayıp yapıştırın.

|Güvenlik denetimi|En fazla güvenli puan noktası|Öneriler|
|----------------|:-------------------:|---------------|
|**MFA’yı etkinleştirme**|10|-MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir<br>-, Aboneliğinizde okuma izinleri olan hesaplarda MFA etkinleştirilmelidir<br>-MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir|
|**Güvenli yönetim bağlantı noktaları**|8|-Tam zamanında ağ erişim denetimi, sanal makinelere uygulanmalıdır<br>-Sanal makinelerin bir ağ güvenlik grubuyla ilişkilendirilmesi gerekir<br>-Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır|
|**Sistem güncelleştirmelerini uygulayın**|6|-İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir<br>-İzleme Aracısı, sanal makine ölçek kümelerine yüklenmelidir<br>-İzleme Aracısı makinelerinizde yüklü olmalıdır<br>-İşletim sistemi sürümü, bulut hizmeti rolleriniz için güncelleştirilmeleri gerekir<br>-Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir<br>-Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir<br>-Sistem güncelleştirmelerinin uygulanabilmesi için makineleriniz yeniden başlatılmalıdır<br>-Kubernetes Hizmetleri, güvenlik açığı olmayan bir Kubernetes sürümüne yükseltilmelidir<br>-İzleme Aracısı sanal makinelerinizde yüklü olmalıdır|
|**Güvenlik açıklarını düzeltin**|6|-SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir<br>-Azure Container Registry görüntülerde güvenlik açıkları düzeltilmelidir (Önizleme)<br>-SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir<br>-Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir<br>-SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir<br>-SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir<br>-Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir|
|**Bekleyen şifrelemeyi etkinleştir**|4|-Disk şifrelemesi sanal makinelere uygulanmalıdır<br>-SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir<br>-Otomasyon hesabı değişkenleri şifrelenmelidir<br>-Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır<br>-SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir|
|**Yoldaki verileri şifreleme**|4|-API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>-İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>-Yalnızca Redis Cache güvenli bağlantı etkinleştirilmelidir<br>-Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir<br>-Web uygulamasına yalnızca HTTPS üzerinden erişilebilir olması gerekir|
|**Erişimi ve izinleri yönetme**|4|-Aboneliğiniz için en fazla 3 sahip belirtilmelidir<br>-Kullanımdan kaldırılan hesaplar aboneliğinizden kaldırılmalı (Önizleme)<br>-Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalı (Önizleme)<br>-Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalı (Önizleme)<br>-Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir<br>-Yazma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir (Önizleme)<br>-Aboneliğinize birden fazla sahip atanmalıdır<br>-Rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır (Önizleme)<br>-Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır|
|**Güvenlik yapılandırmalarını düzeltme**|4|-Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır (Önizleme)<br>-Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir<br>-Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>-Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>-İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>-İzleme Aracısı makinelerinizde yüklü olmalıdır<br>-İzleme Aracısı, sanal makine ölçek kümelerine yüklenmelidir<br>-İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir|
|**Yetkisiz ağ erişimini kısıtla**|4|-Sanal makinenizde IP iletimi devre dışı bırakılmalıdır<br>-Yetkilendirilmiş IP aralıkları Kubernetes hizmetlerinde tanımlanmalıdır (Önizleme)<br>-(Kullanım DıŞı) uygulama hizmetlerine erişim kısıtlı olmalıdır (Önizleme)<br>-(Kullanım DıŞı) IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır<br>-Sanal makinelerin bir ağ güvenlik grubuyla ilişkilendirilmesi gerekir<br>-CORS, her kaynağın API uygulamanıza erişmesine izin vermemelidir<br>-CORS her kaynağın İşlev Uygulaması erişmesine izin vermemelidir<br>-CORS, her kaynağın Web uygulamanıza erişmesine izin vermemelidir<br>-API uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>-İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>-Web uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>-Internet 'e yönelik VM 'lerle izin veren ağ güvenlik grupları için erişim kısıtlanması gerekir<br>-Internet 'e yönelik sanal makineler için ağ güvenlik grubu kuralları sağlamlaştırılmış olmalıdır|
|**Uyarlamalı uygulama denetimi uygulama**|3|-Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir<br>-İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>-İzleme Aracısı makinelerinizde yüklü olmalıdır<br>-İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir|
|**Veri sınıflandırması Uygula**|2|-SQL veritabanlarınızdaki hassas verilerin sınıflandırılmalı (Önizleme)|
|**DDoS saldırılarına karşı uygulamaları koruma**|2|-DDoS koruma standardı etkinleştirilmelidir|
|**Endpoint Protection 'ı etkinleştir**|2|-Endpoint Protection sistem durumu hatalarının sanal makine ölçek kümelerinde düzeltilmelidir<br>-Endpoint Protection sistem durumu sorunları makinelerinizde çözümlenmelidir<br>-Endpoint Protection çözümünün sanal makine ölçek kümelerine yüklenmesi gerekir<br>-Endpoint Protection çözümünü sanal makinelere yükler<br>-İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir<br>-İzleme Aracısı, sanal makine ölçek kümelerine yüklenmelidir<br>-İzleme Aracısı makinelerinizde yüklü olmalıdır<br>-İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>-Makinelerinize Endpoint Protection çözümünü yükler|
|**Denetim ve günlüğe kaydetmeyi etkinleştirme**|1|-SQL Server üzerinde denetim etkinleştirilmelidir<br>-Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir<br>-Azure Data Lake Store tanılama günlükleri etkinleştirilmelidir<br>-Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir<br>-Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir<br>-Data Lake Analytics tanılama günlükleri etkinleştirilmelidir<br>-Olay Hub 'ında tanılama günlükleri etkinleştirilmelidir<br>-IoT Hub tanılama günlükleri etkinleştirilmelidir<br>-Key Vault tanılama günlükleri etkinleştirilmelidir<br>-Logic Apps tanılama günlükleri etkinleştirilmelidir<br>-Arama hizmetindeki tanılama günlükleri etkinleştirilmelidir<br>-Service Bus tanılama günlükleri etkinleştirilmelidir<br>-Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir<br>-Ölçüm uyarı kuralları Batch hesaplarında yapılandırılmalıdır<br>-SQL denetim ayarları, kritik etkinlikleri yakalamak için yapılandırılmış eylem grupları olmalıdır<br>-SQL Server 'lar 90 günden daha uzun denetim bekletme günlerindeki şekilde yapılandırılmalıdır.|
|**En iyi güvenlik uygulamalarını uygulayın**|0|-Güvenlik Duvarı ve sanal ağ yapılandırmalarına sahip depolama hesaplarına erişim kısıtlı olmalıdır<br>-RootManageSharedAccessKey dışındaki tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır<br>-SQL sunucuları için bir Azure Active Directory yöneticisinin sağlanması gerekir<br>-Olay Hub 'ı örneğindeki yetkilendirme kuralları tanımlanmalıdır<br>-Depolama hesaplarının yeni Azure Resource Manager kaynaklara geçirilmesi gerekir<br>-Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir<br>-SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir<br>-Yönetilen örneklerinizin üzerinde gelişmiş veri güvenliği etkinleştirilmelidir<br>-SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda tüm gelişmiş tehdit koruması türleri etkinleştirilmelidir<br>-SQL Server Gelişmiş veri güvenliği ayarlarında yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir<br>-Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır<br>-Alt ağların bir ağ güvenlik grubuyla ilişkilendirilmesi gerekir<br>-SQL Server Gelişmiş veri güvenliği ayarlarında tüm gelişmiş tehdit koruması türleri etkinleştirilmelidir|
||||

## <a name="secure-score-faq"></a>Güvenli puan SSS

### <a name="why-has-my-secure-score-gone-down"></a>Neden güvenli puanım çalışmıyor?
Bu gelişmiş güvenli puanda tanıtılan değişikliklerle, bir kaynağın noktaları almasını sağlamak için tüm önerisi çözmeniz gerekir. Puanlar Ayrıca 0-10 ölçeğine de dönüştürülür.

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Bir güvenlik denetimindeki dört önerimin yalnızca üç tanesi ele alıyorsa, güvenli puanımı değişmem gerekir mi?
Eşleşen tek bir kaynağın tüm önerilerini düzeltene kadar değişmez. Bir denetim için en fazla puanı almak üzere tüm kaynaklar için tüm önerileri düzeltmeniz gerekir.

### <a name="will-this-enhanced-secure-score-replace-the-existing-secure-score"></a>Bu gelişmiş güvenli puan, var olan güvenli puanın yerini alacak mı? 
Evet, ancak bir süre boyunca geçişi kolaylaştırmak için yan yana çalışır.

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Öneri bana uygulanabilir değilse ve ilkede devre dışı bıraktıktan sonra güvenlik denetimin yerine getirilmesi ve güvenli puanım güncellenmem gerekir mi?
Evet. Ortamınızda uygulandıklarında önerilerin devre dışı bırakılmasını öneririz. Belirli bir öneriyi devre dışı bırakma hakkında yönergeler için bkz. [güvenlik Ilkelerini devre dışı bırakma](https://docs.microsoft.com/azure/security-center/tutorial-security-policy#disable-security-policies).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Güvenlik denetimi, güvenli puanma doğru bir şekilde sıfır puan sunduğunda, bunu yoksaymalıdır mi?
Bazı durumlarda, en yüksek puanı sıfırdan büyük bir denetim görürsünüz, ancak etki sıfırdır. Kaynakları düzeltmeye yönelik artımlı puan göz ardı edildiğinde sıfıra yuvarlanır. Bu önerileri, hala güvenlik geliştirmeleri getirdiklerinde yok saymayın. Tek özel durum, "ek En Iyi Yöntem" denetimidir. Bu önerilerin yapılması puanınızı artırmaz, ancak genel güvenliğinizi geliştirir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, gelişmiş güvenli puan ve sunulan yeni güvenlik denetimleri açıklanmaktadır. İlgili malzemeler için aşağıdaki makalelere bakın:

- [Önerinin farklı öğeleri hakkında bilgi edinin](security-center-recommendations.md)
- [Önerileri nasıl düzelteceğinizi öğrenin](security-center-remediate-recommendations.md)