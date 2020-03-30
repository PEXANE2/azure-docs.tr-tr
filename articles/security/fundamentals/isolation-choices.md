---
title: Azure Genel Bulutunda Yalıtım | Microsoft Dokümanlar
description: Azure'un hem kötü amaçlı hem de kötü niyetli olmayan kullanıcılara karşı nasıl yalıtım sağladığını ve mimarlara çeşitli yalıtım seçenekleri sunduğunu öğrenin.
services: security
documentationcenter: na
author: UnifyCloud
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: TomSh
ms.openlocfilehash: c6e74e7992326d2a4b8fe24510742422b005c2e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280319"
---
# <a name="isolation-in-the-azure-public-cloud"></a>Azure Genel Bulutunda Yalıtım
Azure, paylaşılan fiziksel altyapıda uygulamaları ve sanal makineleri (VM' ler) çalıştırmanızı sağlar. Uygulamaları bulut ortamında çalıştırmanın en önemli ekonomik motivasyonlarından biri, paylaşılan kaynakların maliyetini birden çok müşteri arasında dağıtabilmektir. Çoklu kirabu uygulama düşük maliyetlerle birbirinden farklı müşteriler arasında kaynakları çokkatlı tarafından verimliliği artırır. Ne yazık ki, aynı zamanda rasgele ve potansiyel olarak kötü niyetli bir kullanıcıya ait olabilir hassas uygulamaları ve VM'ler çalıştırmak için fiziksel sunucular ve diğer altyapı kaynakları paylaşımı riskini tanıtır.

Bu makalede, Azure'un hem kötü amaçlı hem de kötü amaçlı olmayan kullanıcılara karşı nasıl yalıtım sağladığı ve mimarlara çeşitli yalıtım seçenekleri sunarak bulut çözümlerini mimaretmek için bir kılavuz görevi göstermesi özetlanmaktadır.

## <a name="tenant-level-isolation"></a>Kiracı Seviye Yalıtımı
Bulut bilgi işlemin başlıca avantajlarından biri, aynı anda çok sayıda müşteri arasında paylaşılan, ortak bir altyapı kavramıdır ve bu da ölçek ekonomilerine yol açacaktır. Bu kavram çoklu kira denir. Microsoft, Microsoft Cloud Azure'un çok kiracılı mimarisinin güvenlik, gizlilik, gizlilik, bütünlük ve kullanılabilirlik standartlarını desteklediğinden emin olmak için sürekli olarak çalışır.

Bulutun etkin olduğu çalışma alanında kiracı, bulut hizmetinin belirli bir örneğine sahip olan ve o örneği yöneten bir istemci veya kuruluş olarak tanımlanabilir. Microsoft Azure tarafından sağlanan kimlik platformuyla kiracı, kuruluşunuzun microsoft bulut hizmetine kaydolurken aldığı ve sahip olduğu Azure Etkin Dizin (Azure AD) özel bir örneğidir.

Her Azure AD dizini, diğer Azure AD dizinlerinden farklı ve ayrıdır. Kurumsal ofis binasının yalnızca kuruluşunuza özel güvenilir bir varlık olması gibi, Azure AD dizini de yalnızca sizin kuruluşunuz tarafından kullanılmak üzere tasarlanan güvenilir bir varlıktır. Azure AD mimarisi, müşteri verilerini ve kimlik bilgilerini ortak karıştırma alanından yalıtır. Bu, bir Azure AD dizinindeki kullanıcıların ve yöneticilerin yanlışlıkla veya kötü amaçlı olarak başka bir dizindeki verilere erişemeyeceği anlamına gelir.

### <a name="azure-tenancy"></a>Azure Kira
Azure kira (Azure Aboneliği), "müşteri/faturalandırma" ilişkisini ve [Azure Etkin Dizini'nde](../../active-directory/fundamentals/active-directory-whatis.md)benzersiz bir [kiracıyı](../../active-directory/develop/quickstart-create-new-tenant.md) ifade eder. Microsoft Azure'da kiracı düzeyi yalıtımı, Azure Etkin Dizini ve onun tarafından sunulan [rol tabanlı denetimler](../../role-based-access-control/overview.md) kullanılarak elde edilir. Her Azure aboneliği bir Azure Etkin Dizin (AD) dizini ile ilişkilidir.

Bu dizindeki kullanıcılar, gruplar ve uygulamalar Azure aboneliğindeki kaynakları yönetebilir. Bu erişim haklarını Azure portalı, Azure komut satırı araçlarını ve Azure Yönetimi API'lerini kullanarak atayabilirsiniz. Azure AD kiracısı, hiçbir müşterinin ortak kiracılarına kötü amaçlı veya yanlışlıkla erişemesin veya bu yla güvenlik sınırlarını kullanarak mantıksal olarak yalıtılır. Azure AD, ana bilgisayar düzeyinde paket filtreleme ve Windows Güvenlik Duvarı'nın istenmeyen bağlantıları ve trafiği engellediği ayrılmış ağ segmentinde yalıtılmış "çıplak metal" sunucularda çalışır.

- Azure AD'deki verilere erişim, bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir. Bu oturumda, hedef kiracıya istenen erişimin bu kullanıcı için izin verilip verilmediğini belirlemek için yetkilendirme sistemi tarafından kullanıcının varlığı, etkin durumu ve rolü hakkındaki bilgiler kullanılır.

![Azure Kira](./media/isolation-choices/azure-isolation-fig1.png)


- Kiracılar ayrı kapsayıcılar ve bunlar arasında hiçbir ilişki yoktur.

- Kiracı yönetici, federasyon veya diğer kiracılardan kullanıcı hesapları sağlama yoluyla hibe sürece kiracı arasında erişim yok.

- Azure AD hizmetini oluşturan sunuculara fiziksel erişim ve Azure AD'nin arka uç sistemlerine doğrudan erişim kısıtlanmıştır.

- Azure AD kullanıcılarının fiziksel varlıklara veya konumlara erişimi yoktur ve bu nedenle aşağıdaki şekilde belirtilen mantıksal RBAC ilke denetimlerini atlatırlar.

Tanılama ve bakım ihtiyaçları için, tam zamanında ayrıcalık yükselti sistemi kullanan bir operasyonel model gereklidir ve kullanılır. Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM), uygun bir yönetici kavramını sunar. [Uygun yöneticiler,](../../active-directory/privileged-identity-management/pim-configure.md) her gün değil, şimdi ve sonra ayrıcalıklı erişime ihtiyaç duyan kullanıcılar olmalıdır. Bu rol, kullanıcı erişime ihtiyaç duyana kadar devre dışıdır ancak kullanıcı bir etkinleştirme işlemini tamamladıktan sonra önceden belirlenen süre boyunca etkin bir yönetici olur.

![Azure AD Privileged Identity Management](./media/isolation-choices/azure-isolation-fig2.png)

Azure Etkin Dizin, yalnızca kiracının sahip olduğu ve yönettiği kapsayıcının ilkeleri ve izinleri ile her kiracıyı kendi korumalı kapsayıcısında barındırUr.

Kiracı kapsayıcıları kavramı, portallardan kalıcı depolamaya kadar tüm katmanlarda dizin hizmetinde kökleşmiştir.

Birden çok Azure Etkin Dizin kiracısından gelen meta veriler aynı fiziksel diskte depolansa bile, kapsayıcılar arasında dizin hizmeti tarafından tanımlanan ve kiracı yönetici tarafından dikte edilen den başka bir ilişki yoktur.

### <a name="azure-role-based-access-control-rbac"></a>Azure Role Tabanlı Erişim Denetimi (RBAC)
[Azure Rolüne Dayalı Erişim Denetimi (RBAC),](../../role-based-access-control/overview.md) Azure için ince taneli erişim yönetimi sağlayarak Azure aboneliğinde bulunan çeşitli bileşenleri paylaşmanıza yardımcı olur. Azure RBAC, kuruluşunuzdaki görevleri ayırmanızı ve kullanıcıların işlerini gerçekleştirmek için gerekenlere göre erişim sağlamanızı sağlar. Azure aboneliğinde veya kaynaklarında herkese sınırsız izin vermek yerine, yalnızca belirli eylemlere izin verebilirsiniz.

Azure RBAC'ın tüm kaynak türleri için geçerli olan üç temel rolü vardır:

- **Sahibi,** başkalarına erişimi devretme hakkı da dahil olmak üzere tüm kaynaklara tam erişime sahiptir.

- **Katılımcı,** tüm Azure kaynakları oluşturabilir ve yönetebilir, ancak başkalarına erişim izni veremez.

- **Okuyucu** varolan Azure kaynaklarını görüntüleyebilir.

![Azure Rol Tabanlı Erişim Denetimi](./media/isolation-choices/azure-isolation-fig3.png)

Azure'daki RBAC rollerinin geri kalanı belirli Azure kaynaklarının yönetimine izin verir. Örneğin Sanal Makine Katılımcısı rolü, kullanıcının sanal makine oluşturmasını ve yönetmesini sağlar. Bu, onlara Azure Sanal Ağı'na veya sanal makinenin bağlandığı alt ağa erişim sağlamaz.

[RBAC yerleşik rolleri](../../role-based-access-control/built-in-roles.md) Azure'da kullanılabilen rolleri listeler. Her yerleşik rol kullanıcıya sağladığı işlemleri ve kapsamı belirtir. Daha fazla denetim için kendi rollerinizi tanımlamak istiyorsanız, [Azure RBAC'da Özel rollerin](../../role-based-access-control/custom-roles.md)nasıl oluşturabileceğinizi görün.

Azure Etkin Dizini için diğer bazı özellikler şunlardır:
- Azure AD, barındırıldıkları yerden bağımsız olarak SaaS uygulamaları için SSO özelliğini etkinleştirir. Bazı uygulamalar Azure AD federasyonu kullanırken diğerleri parola SSO hizmetinden yararlanır. Federe uygulamalar da kullanıcı sağlama ve [şifre tonozlama](https://www.techopedia.com/definition/31415/password-vault)destekleyebilir.

- [Azure Storage](https://azure.microsoft.com/services/storage/) verilerine erişim, kimlik doğrulaması ile denetlenir. Her depolama hesabının birincil anahtarı[(depolama hesabı anahtarı](../../storage/common/storage-create-storage-account.md)veya SAK) ve ikincil bir gizli anahtar (paylaşılan erişim imzası veya SAS) vardır.

- Azure AD, [Etkin Dizin Federasyonu Hizmetleri,](../../active-directory/hybrid/how-to-connect-fed-azure-adfs.md)eşitleme ve şirket içi dizinlerle çoğaltma kullanarak federasyon aracılığıyla Hizmet Olarak Kimlik sağlar.

- [Azure Çok Faktörlü Kimlik Doğrulama,](../../active-directory/authentication/multi-factor-authentication.md) kullanıcıların bir mobil uygulama, telefon görüşmesi veya kısa mesaj kullanarak oturum açma oturumlarını doğrulamalarını gerektiren çok faktörlü kimlik doğrulama hizmetidir. Azure Çok Faktörlü Kimlik Doğrulama sunucusuyla şirket içi kaynakların güvenliğini sağlamaya yardımcı olmak için Azure AD ile ve Ayrıca SDK'yı kullanan özel uygulamalar ve dizinlerle kullanılabilir.

- [Azure AD Etki Alanı Hizmetleri,](https://azure.microsoft.com/services/active-directory-ds/) etki alanı denetleyicilerini dağıtmadan Azure sanal makinelerine Etkin Dizin etki alanına katılmanızı sağlar. Bu sanal makinelerde kurumsal Active Directory kimlik bilgilerinizle oturum açabilir ve tüm Azure sanal makinelerinizde güvenlik taban çizgilerini zorlamak için Grup İlkesi'ni kullanarak etki alanına katılan sanal makineleri yönetebilirsiniz.

- [Azure Active Directory B2C,](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimliğe ölçeklendirilebilen tüketiciye yönelik uygulamalar için son derece kullanılabilir bir küresel kimlik yönetimi hizmeti sağlar. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz, mevcut sosyal hesaplarını kullanarak veya kimlik bilgileri oluşturarak özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınızda oturum açabilir.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Veri Silme & Microsoft Yöneticilerinden Yalıtım
Microsoft, verilerinizi yetkisiz kişilerin uygunsuz erişimlerine veya kullanımına karşı korumak için güçlü önlemler alır. Bu operasyonel süreçler ve denetimler, verilerinize erişimi düzenleyen sözleşmetaahhütleri sunan [Çevrimiçi Hizmet Koşulları](https://aka.ms/Online-Services-Terms)tarafından desteklenir.

-   Microsoft mühendislerinin buluttaki verilerinize varsayılan erişimi yoktur. Bunun yerine, yalnızca gerektiğinde yönetim gözetimi altında erişim hakkı verilir. Bu erişim dikkatle denetlenir ve günlüğe kaydedilir ve artık ihtiyaç duyulmadığında iptal edilir.

-   Microsoft, kendi adına sınırlı hizmetler sağlaması için başka şirketler kiralayabilir. Alt yükleniciler müşteri verilerine yalnızca, sağlamak için kiraladığımız hizmetleri sunmak için erişebilir ler ve bunları başka herhangi bir amaçla kullanmaları yasaktır. Ayrıca, müşterilerimizin bilgilerinin gizliliğini korumak için sözleşmeye bağlıdırlar.

ISO/IEC 27001 gibi denetlenen sertifikalara sahip iş hizmetleri, yalnızca yasal iş amaçları için bu erişimi kanıtlamak için örnek denetimler gerçekleştiren Microsoft ve akredite denetim firmaları tarafından düzenli olarak doğrulanır. İstediğiniz zaman ve herhangi bir nedenle kendi müşteri verilerinize her zaman erişebilirsiniz.

Herhangi bir veriyi silerseniz, Önbelleğe alınmış veya yedek kopyalar da dahil olmak üzere Microsoft Azure verileri siler. Kapsam içi hizmetler için, bu silme işlemi bekletme süresinin bitiminden sonraki 90 gün içinde gerçekleşir. (Kapsam içi [hizmetler, Çevrimiçi Hizmet Koşullarımızın](https://aka.ms/Online-Services-Terms)Veri İşleme Koşulları bölümünde tanımlanır.)

Depolama için kullanılan bir disk sürücüsünde donanım hatası varsa, Microsoft değiştirme veya onarım için üreticiye iade etmeden önce güvenli bir şekilde silinir veya [yok edilir.](https://microsoft.com/trustcenter/privacy/you-own-your-data) Sürücüdeki veriler, verilerin hiçbir şekilde kurtarılamayacağından emin olmak için üzerine yazılır.

## <a name="compute-isolation"></a>İşlem İzolasyon
Microsoft Azure, uygulamanızın veya işletmenizin gereksinimlerini karşılamak üzere otomatik olarak ölçeklendirilebilen çok çeşitli bilgi işlem örnekleri & hizmetleri içeren çeşitli bulut tabanlı bilgi işlem hizmetleri sağlar. Bu bilgi işlem örneği ve hizmeti, müşterilerin talep ettiği yapılandırma esnekliğinden ödün vermeden verileri güvenli hale getirmek için birden çok düzeyde yalıtım sunar.

### <a name="isolated-virtual-machine-sizes"></a>İzole Sanal Makine Boyutları

[!INCLUDE [virtual-machines-common-isolation](../../../includes/virtual-machines-common-isolation.md)]

### <a name="dedicated-hosts"></a>Ayrılmış konaklar
Azure, önceki bölümde açıklanan yalıtılmış ana bilgisayarların yanı sıra özel ana bilgisayarlar da sunar. Azure'daki özel ana bilgisayarlar, bir veya daha fazla sanal makine barındırabilen ve tek bir Azure aboneliğine adanmış fiziksel sunucular sağlayan bir hizmettir. Özel ana bilgisayarlar, fiziksel sunucu düzeyinde donanım yalıtımı sağlar. Ana bilgisayarlarınıza başka hiçbir VM yerleştirilmez. Özel ana bilgisayarlar aynı veri merkezlerinde dağıtılır ve aynı ağ ve altta yatan depolama altyapısını diğer, yalıtılmış olmayan ana bilgisayarlarla paylaşır. Daha fazla bilgi için [Azure'a özel ana bilgisayarların](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)ayrıntılı genel görünümüne bakın.

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Root VM & Konuk VM'ler Arasında Hyper-V & Kök İşletim İzolasyon
Azure'un bilgi işlem platformu makine sanallaştırması temel alınca tüm müşteri kodlarının bir Hyper-V sanal makinede yürütülmesi anlamına gelir. Her Azure düğümünde (veya ağ bitiş noktasında), doğrudan donanımın üzerinde çalışan ve bir düğümü değişken sayıda Konuk Sanal Makineye (VM) bölen bir Hypervisor vardır.


![Root VM & Konuk VM'ler Arasında Hyper-V & Kök İşletim İzolasyon](./media/isolation-choices/azure-isolation-fig4.jpg)


Her düğümde Ayrıca Host OS'yi çalıştıran özel bir Root VM vardır. Kritik bir sınır, hypervisor ve root OS tarafından yönetilen konuk VM'lerden ve konuk VM'lerden vm kökünün yalıtımıdır. Hypervisor/root OS eşleştirmesi, Microsoft'un on yıllardır süren işletim sistemi güvenlik deneyiminden ve konuk VM'lerin güçlü bir şekilde yalıtımını sağlamak için Microsoft'un Hyper-V'inden daha yeni bir öğrenme den yararlanır.

Azure platformu, sanallaştırılmış bir ortam kullanır. Kullanıcı örnekleri, fiziksel bir ana bilgisayar sunucusuna erişimi olmayan bağımsız sanal makineler olarak çalışır.

Azure hipervizörü mikro çekirdek gibi davranır ve konuk sanal makinelerden vmbus adı verilen paylaşılan bellek arabirimini kullanarak işlenmek üzere ana bilgisayara tüm donanım erişim isteklerini iletir. Bu, kullanıcıların sistemde ham okuma/yazma/yürütme erişimine sahip olmasını önler ve sistem kaynaklarının paylaşımıyla ilgili riskleri azaltır.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Gelişmiş VM yerleştirme algoritması yan kanal saldırılarına karşı koruma &
Herhangi bir çapraz VM saldırısı iki adım içerir: kurban VM'lerinden biriyle aynı ana bilgisayara düşman kontrolündeki bir VM yerleştirmek ve daha sonra hassas kurban bilgilerini çalmak veya açgözlülük veya vandalizm performansını etkilemek için izolasyon sınırını aşmak. Microsoft Azure, gelişmiş bir VM yerleşim algoritması kullanarak her iki adımda da koruma sağlar ve gürültülü komşu VM'ler de dahil olmak üzere bilinen tüm yan kanal saldırılarına karşı koruma sağlar.

### <a name="the-azure-fabric-controller"></a>Azure Kumaş Denetleyicisi
Azure Kumaş Denetleyicisi, altyapı kaynaklarını kiracı iş yüklerine ayırmaktan sorumludur ve ana bilgisayardan sanal makinelere tek yönlü iletişimleri yönetir. Azure kumaş denetleyicisinin VM yerleştirme algoritması son derece karmaşıktır ve fiziksel ana bilgisayar düzeyi olarak tahmin edilmesi neredeyse imkansızdır.

![Azure Kumaş Denetleyicisi](./media/isolation-choices/azure-isolation-fig5.png)

Azure hipervizörü sanal makineler arasındaki bellek ve işlem ayrımını zorlar ve ağ trafiğini konuk işletim sistemi kiracılarına güvenli bir şekilde yönlendirir. Bu, VM düzeyinde yan kanal saldırısı olasılığını ortadan kaldırır.

Azure'da VM kökü özeldir: bir kumaş aracısı (FA) barındıran kök işletim sistemi adı verilen sertleştirilmiş bir işletim sistemi çalıştırır. FA'lar, müşteri VM'lerinde konuk OS'ler içinde konuk aracıları (GA) yönetmek için kullanılır. FA'lar depolama düğümlerini de yönetir.

Azure hipervizörü, root OS/FA ve müşteri VM/G'leri koleksiyonu bir işlem düğümü nden oluşur. FA'lar, bilgi işlem ve depolama düğümleri dışında bulunan bir kumaş denetleyicisi (FC) tarafından yönetilir (bilgi işlem ve depolama kümeleri ayrı FC'ler tarafından yönetilir). Bir müşteri çalışırken uygulamanın yapılandırma dosyasını güncelleştirirse, FC Fa ile iletişim kurar ve daha sonra GAs ile bağlantı kurar ve yapılandırma değişikliğini uygulamayı bildirir. Bir donanım hatası durumunda, FC kullanılabilir donanımı otomatik olarak bulur ve VM'yi orada yeniden başlatAcaktır.

![Masmavi Kumaş Denetleyici](./media/isolation-choices/azure-isolation-fig6.jpg)

Kumaş Denetleyicisinden bir ajana iletişim tek yönlüdür. Aracı, yalnızca denetleyiciden gelen isteklere yanıt veren SSL korumalı bir hizmet uygular. Denetleyiciye veya diğer ayrıcalıklı iç düğümlere bağlantı başlatamaz. FC, tüm yanıtları güvenilmemiş gibi ele adatır.


![Kumaş Denetleyici](./media/isolation-choices/azure-isolation-fig7.png)

İzolasyon, Konuk VM'lerden Root VM'den ve konuk VM'lerden birbirinden genişler. İşlem düğümleri de daha fazla koruma için depolama düğümlerinden izole edilmiştir.


Hypervisor ve ana bilgisayar işletim sistemi ağ paketi sağlar - güvenilmeyen sanal makinelerin sahte trafik oluşturamamasını veya kendilerine hitap edilmeyen trafiği alamamasını, trafiği korumalı altyapı uç noktalarına yönlendiremeyeceğini veya uygunsuz yayın trafiği gönderemez/alamayacağından emin olmak için filtreler.


### <a name="additional-rules-configured-by-fabric-controller-agent-to-isolate-vm"></a>VM'yi Yalıtmak için Kumaş Denetleyici Aracısı tarafından Yapılandırılan Ek Kurallar
Varsayılan olarak, sanal bir makine oluşturulduğunda tüm trafik engellenir ve ardından kumaş denetleyici aracısı paket filtresini yetkili trafiğe izin vermek için kurallar ve özel durumlar eklemek üzere yapılandırır.

Programlanmış iki kural kategorisi vardır:

-   **Makine yapılandırması veya altyapı kuralları:** Varsayılan olarak, tüm iletişim engellenir. Sanal bir makinenin DHCP ve DNS trafiğini gönderip almasına izin veren istisnalar vardır. Sanal makineler aynı Azure Sanal Ağı ve işletim sistemi etkinleştirme sunucusundaki trafiği "herkese açık" internete gönderebilir ve trafiği diğer sanal makinelere gönderebilir. Sanal makinelerin izin verilen giden hedefler listesi Azure yönlendirici alt ağlarını, Azure yönetimini ve diğer Microsoft özelliklerini içermez.

-   **Rol yapılandırma dosyası:** Bu, kiracının hizmet modeline göre gelen Erişim Denetim Listeleri'ni (ALA)'ları tanımlar.

### <a name="vlan-isolation"></a>VLAN İzolasyon
Her kümede üç VUN vardır:

![VLAN İzolasyon](./media/isolation-choices/azure-isolation-fig8.jpg)


-   Ana VLAN – güvenilmeyen müşteri düğümlerini birbirine bağlar

-   FC VLAN – güvenilir FC'ler ve destek sistemleri içerir

-   VLAN cihazı güvenilir ağ ve diğer altyapı aygıtları içerir

FC VLAN'dan ana VLAN'a iletişim izin verilir, ancak ana VLAN'dan FC VLAN'a başlatılamaz. Ana VLAN'dan VLAN cihazına iletişim de engellenir. Bu, çalışan bir düğüm müşteri kodu tehlikeye olsa bile, fc veya aygıt VLANs düğümlerine saldıramayacağını garanti eder.

## <a name="storage-isolation"></a>Depolama Yalıtımı
### <a name="logical-isolation-between-compute-and-storage"></a>İşlem ve Depolama Arasında Mantıksal Yalıtım
Temel tasarımının bir parçası olarak Microsoft Azure, VM tabanlı hesaplamayı depolamadan ayırır. Bu ayırma, hesaplama ve depolamanın bağımsız olarak ölçeklendirmesini sağlayarak çoklu kira ve yalıtım sağlamayı kolaylaştırır.

Bu nedenle, Azure Depolama, mantıksal olarak dışında Azure Compute'a ağ bağlantısı olmayan ayrı bir donanımda çalışır. Bu, sanal bir disk oluşturulduğunda, disk alanının tüm kapasitesi için ayrılmadığı anlamına gelir. Bunun yerine, sanal diskteki adresleri fiziksel diskteki alanlara eşleyen bir tablo oluşturulur ve bu tablo başlangıçta boştur. **Bir müşteri sanal diske ilk kez veri yazdığında, fiziksel diskteki alan ayrılır ve tabloya bir işaretçi yerleştirilir.**
### <a name="isolation-using-storage-access-control"></a>Depolama Erişimi denetimini kullanarak yalıtım
**Azure Depolama'da Erişim Denetimi'nin** basit bir erişim denetim modeli vardır. Her Azure aboneliği bir veya daha fazla Depolama Hesabı oluşturabilir. Her Depolama Hesabı'nın, o Depolama Hesabındaki tüm verilere erişimi denetlemek için kullanılan tek bir gizli anahtarı vardır.

![Depolama Erişimi denetimini kullanarak yalıtım](./media/isolation-choices/azure-isolation-fig9.png)

**Azure Depolama verilerine erişim (Tablolar dahil)** kapsamlı erişim sağlayan bir [SAS (Paylaşılan Erişim İmzası)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) belirteci aracılığıyla denetlenebilir. SAS, [SAK (Depolama Hesap Anahtarı)](https://msdn.microsoft.com/library/azure/ee460785.aspx)ile imzalanmış bir sorgu şablonu (URL) aracılığıyla oluşturulur. Bu [imzalı URL,](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) sorgunun ayrıntılarını doldurup depolama hizmetinin isteğini yerine getirebilecek başka bir işleme (diğer bir deyişle, temsilci) verilebilir. SAS, depolama hesabının gizli anahtarını açıklamadan istemcilere zaman tabanlı erişim sağlamanızı sağlar.

SAS, bir istemciye, depolama hesabımızdaki nesnelere belirli bir süre ve belirli bir izin kümesiyle sınırlı izinler verebileceğimiz anlamına gelir. Bu sınırlı izinleri hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan verebiliriz.

### <a name="ip-level-storage-isolation"></a>IP Düzey Depolama Yalıtımı
Güvenlik duvarları oluşturup, güvenilir müşterileriniz için bir IP adresi aralığı tanımlayabilirsiniz. IP adres aralığıyla, yalnızca tanımlanan aralıkta IP adresi olan istemciler [Azure Depolama'ya](../../storage/blobs/security-recommendations.md)bağlanabilir.

IP depolama verileri, IP depolamasına özel veya özel bir trafik tüneli ayırmak için kullanılan bir ağ mekanizması aracılığıyla yetkisiz kullanıcılara karşı korunabilir.

### <a name="encryption"></a>Şifreleme
Azure, verileri korumak için aşağıdaki Şifreleme türlerini sunar:
-   Aktarım sırasında şifreleme

-   Bekleme sırasında şifreleme

#### <a name="encryption-in-transit"></a>Aktarımda Şifreleme
Aktarım sırasında şifreleme, ağlar arasında aktarıldığında verileri koruma mekanizmasıdır. Azure Depolama ile verileri şu şekilde güvene alabilirsiniz:

-   Azure Depolama'ya veri aktarırken veya Azure Depolama'nın dışına aktarırken HTTPS gibi [aktarım düzeyinde şifreleme.](../../storage/blobs/security-recommendations.md)

-   Azure Dosya paylaşımları için SMB 3.0 şifrelemesi gibi [kablo şifrelemesi.](../../storage/blobs/security-recommendations.md)

-   [İstemci tarafı şifreleme,](../../storage/blobs/security-recommendations.md)depolamaya aktarılmadan önce verileri şifrelemek ve depolama dışına aktarıldıktan sonra verilerin şifresini çözmek için.

#### <a name="encryption-at-rest"></a>Istirahatte Şifreleme
Birçok kuruluş için veri [şifreleme, veri](isolation-choices.md) gizliliği, uyumluluk ve veri egemenliği yolunda zorunlu bir adımdır. "Beklemede" olan verilerin şifrelemesini sağlayan üç Azure özelliği vardır:

-   [Depolama Hizmeti](../../storage/blobs/security-recommendations.md) Şifrelemesi, depolama hizmetinin verileri Azure Depolama'ya yazarken otomatik olarak şifrelemesini istemenize olanak tanır.

-   [İstemci tarafı Şifreleme](../../storage/blobs/security-recommendations.md) de istirahatşifreleme özelliği sağlar.

-   [Azure Disk Şifreleme,](../azure-security-disk-encryption-overview.md) IaaS sanal makinesi tarafından kullanılan işletim sistemi disklerini ve veri disklerini şifrelemenize olanak tanır.

#### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
Sanal makineler için [Azure Disk Şifrelemesi](../azure-security-disk-encryption-overview.md) (VM'ler), [Azure Anahtar Kasası'nda](https://azure.microsoft.com/services/key-vault/)kontrol ettiğiniz anahtarlar ve ilkelerle VM disklerinizi (önyükleme ve veri diskleri dahil) şifreleyerek kuruluş güvenliği ve uyumluluk gereksinimlerini gidermenize yardımcı olur.

Windows için Disk Şifreleme çözümü [Microsoft BitLocker Sürücü Şifreleme](https://technet.microsoft.com/library/cc732774.aspx)dayanmaktadır ve Linux çözümü [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt)dayanmaktadır.

Çözüm, IaaS VM'ler için Microsoft Azure'da etkinleştirildiğinde aşağıdaki senaryoları destekler:
-   Azure Anahtar Kasası ile Tümleştirme

-   Standart kademe VM'ler: A, D, DS, G, GS ve benzeri, Seri IaaS VM'ler

-   Windows ve Linux IaaS VM'lerde şifrelemeyi etkinleştirme

-   Windows IaaS VM'leri için işletim sistemi ve veri sürücülerinde şifrelemeyi devre dışı bırakma

-   Linux IaaS VM'ler için veri sürücülerinde şifrelemenin devre dışı bırakılması

-   Windows istemci işletim sistemi çalıştıran IaaS VM'lerde şifreleme yi etkinleştirme

-   Montaj yolları olan birimlerde şifreleme yi etkinleştirme

-   [Mdadm](https://en.wikipedia.org/wiki/Mdadm) kullanarak disk şeridi (RAID) ile yapılandırılan Linux VM'lerinde şifrelemeyi etkinleştirme

-   Veri diskleri için [LVM (Mantıksal Birim Yöneticisi)](https://msdn.microsoft.com/library/windows/desktop/bb540532) kullanarak Linux VM'lerinde şifrelemeyi etkinleştirme

-   Depolama alanları kullanılarak yapılandırılan Windows VM'lerinde şifreleme yi etkinleştirme

-   Tüm Azure ortak bölgeleri desteklenir

Çözüm, sürümdeki aşağıdaki senaryoları, özellikleri ve teknolojiyi desteklemez:

-   Temel seviye IaaS VM'ler

-   Linux IaaS VM'ler için işletim sistemi sürücüsünde şifrelemeyi devre dışı bırakma

-   Klasik VM oluşturma yöntemi kullanılarak oluşturulan IaaS VM'ler

-   Şirket içi Anahtar Yönetim Hizmetinizle entegrasyon

-   Yazılım tabanlı RAID sistemleriyle yapılandırılan Azure Dosyaları (paylaşılan dosya sistemi), Ağ Dosya Sistemi (NFS), dinamik birimler ve Windows VM'leri

## <a name="sql-azure-database-isolation"></a>SQL Azure Veritabanı Yalıtımı
SQL Veritabanı, piyasa lideri Microsoft SQL Server altyapısını temel alan ve görev açısından kritik iş yüklerini üstlenebilen, Microsoft bulutu tabanlı bir ilişkisel veritabanı hizmetidir. SQL Veritabanı hesap düzeyinde öngörülebilir veri yalıtımı sunar, coğrafya / bölge tabanlı ve ağ tabanlı- tüm sıfıra yakın yönetim ile.

### <a name="sql-azure-application-model"></a>SQL Azure Uygulama Modeli

[Microsoft SQL Azure](../../sql-database/sql-database-single-database-get-started.md) Veritabanı, SQL Server teknolojileri üzerine kurulu bulut tabanlı bir ilişkisel veritabanı hizmetidir. Microsoft tarafından bulutta barındırılan yüksek kullanılabilir, ölçeklenebilir, çok kiracılı bir veritabanı hizmeti sağlar.

Uygulama açısından SQL Azure aşağıdaki hiyerarşiyi sağlar: Her düzey, aşağıda bir-çok düzey içeren bir düzeye sahiptir.

![SQL Azure Uygulama Modeli](./media/isolation-choices/azure-isolation-fig10.png)

Hesap ve abonelik, faturalandırma ve yönetimi ilişkilendirmek için Microsoft Azure platform kavramlarıdır.

Mantıksal sunucular ve veritabanları SQL Azure'a özgü kavramlardır ve SQL Azure kullanılarak, OData ve TSQL arayüzleri sağlayarak veya Azure portalına entegre edilmiş SQL Azure portalı aracılığıyla yönetilir.

SQL Azure sunucuları fiziksel veya VM örnekleri değildir, bunun yerine veritabanları, paylaşım yönetimi ve güvenlik ilkeleri koleksiyonlarıdır ve bunlar "mantıksal ana" veritabanında depolanır.

![SQL Azure](./media/isolation-choices/azure-isolation-fig11.png)

Mantıksal ana veritabanları şunlardır:

-   Sunucuya bağlanmak için kullanılan SQL girişleri

-   Güvenlik duvarı kuralları

Aynı mantıksal sunucudan SQL Azure veritabanları için faturalandırma ve kullanımla ilgili bilgilerin SQL Azure kümesinde aynı fiziksel örnekte olduğu garanti edilmez, bunun yerine uygulamaların bağlanırken hedef veritabanı adını sağlaması gerekir.

Müşteri açısından bakıldığında, mantıksal bir sunucu coğrafi grafik bölgesinde oluşturulurken, sunucunun gerçek oluşturulması bölgedeki kümelerden birinde gerçekleşir.

### <a name="isolation-through-network-topology"></a>Ağ Topolojisi ile Yalıtım

Mantıksal bir sunucu oluşturulduğunda ve DNS adı kaydedildiğinde, DNS adı sunucunun yerleştirildiği belirli veri merkezindeki sözde "Ağ Geçidi VIP" adresine işaret eder.

VIP (sanal IP adresi) arkasında, biz vatansız ağ geçidi hizmetleri bir koleksiyona sahip. Genel olarak, birden çok veri kaynağı (ana veritabanı, kullanıcı veritabanı, vb) arasında koordinasyon gerektiğinde ağ geçitleri dahil olsun. Ağ geçidi hizmetleri aşağıdakileri uygular:
-   **TDS bağlantı proxying.** Bu, kullanıcı veritabanını arka uç kümesinde bulmayı, oturum açma sırasını uygulamayı ve Ardından TDS paketlerini arka uça ve arkaya iletmeyi içerir.

-   **Veritabanı yönetimi.** Bu, CREATE/ALTER/DROP veritabanı işlemleri yapmak için bir iş akışı koleksiyonunu n uygulanmasını içerir. Veritabanı işlemleri, TDS paketlerini koklayarak veya açık OData API'leri ile çağrılabilir.

-   CREATE/ALTER/DROP giriş/kullanıcı işlemleri

-   OData API ile mantıksal sunucu yönetimi işlemleri

![Ağ Topolojisi ile Yalıtım](./media/isolation-choices/azure-isolation-fig12.png)

Ağ geçitlerinin arkasındaki katmana "arka uç" denir. Tüm verilerin son derece kullanılabilir bir şekilde depolandığı yerdir. Her veri parçasının bir "bölüm" veya "başarısız birim"e ait olduğu söylenir ve her birinin en az üç kopyası vardır. Yinelemeler SQL Server altyapısı tarafından depolanır ve çoğaltılır ve genellikle "kumaş" olarak adlandırılan bir başarısız sistem tarafından yönetilir.

Genellikle, arka uç sistemi bir güvenlik önlemi olarak giden diğer sistemlere iletişim kurmaz. Bu, ön uç (ağ geçidi) katmanındaki sistemlere ayrılmıştır. Ağ geçidi katmanı makineleri, saldırı yüzeyini derinlemesine bir savunma mekanizması olarak en aza indirmek için arka uç makinelerde sınırlı ayrıcalıklara sahiptir.

### <a name="isolation-by-machine-function-and-access"></a>Makine Fonksiyonu ve Erişime Göre Yalıtım
SQL Azure (farklı makine işlevleri üzerinde çalışan hizmetlerden oluşur. SQL Azure, "arka uç" Bulut Veritabanı ve "ön uç" (Ağ Geçidi/Yönetim) ortamlarına ayrılır ve trafiğin genel ilkesi yalnızca arka uca girer ve dışarı çıkmaz. Ön uç ortamı diğer hizmetlerin dış dünyasına iletişim kurabilir ve genel olarak, arka uçta yalnızca sınırlı izinlere sahiptir (çağırması gereken giriş noktalarını aramak için yeterlidir).

## <a name="networking-isolation"></a>Ağ Yalıtımı
Azure dağıtımında birden çok ağ yalıtım katmanı vardır. Aşağıdaki diyagram, Azure'un müşterilere sağladığı çeşitli ağ yalıtımı katmanlarını gösterir. Bu katmanlar hem Azure platformunun kendisinde hem de müşteri tanımlı özelliklerde yereldir. Azure DDoS, Internet'ten gelen büyük ölçekli saldırılara karşı yalıtım sağlar. Bir sonraki yalıtım katmanı, bulut hizmetinden sanal ağa hangi trafiğin geçebileceğini belirlemek için kullanılan müşteri tanımlı genel IP adresleridir (uç noktalar). Yerel Azure sanal ağ yalıtımı, diğer tüm ağlardan tam yalıtım sağlar ve trafik yalnızca kullanıcı tarafından yapılandırılan yollar ve yöntemlerüzerinden akar. Bu yollar ve yöntemler, NSG'lerin, UDR'lerin ve ağ sanal cihazlarının koruma altındaki ağdaki uygulama dağıtımlarını korumak için yalıtım sınırları oluşturmak için kullanılabildiği bir sonraki katmandır.

![Ağ Yalıtımı](./media/isolation-choices/azure-isolation-fig13.png)

**Trafik yalıtımı:** [Sanal ağ,](../../virtual-network/virtual-networks-overview.md) Azure platformundaki trafik yalıtım sınırıdır. Bir sanal ağdaki sanal makineler (VM'ler), her iki sanal ağ da aynı müşteri tarafından oluşturulsa bile, farklı bir sanal ağdaki VM'lere doğrudan iletişim kuramaz. Yalıtım, müşteri VM'lerinin ve iletişimin sanal ağ içinde gizli kalmasını sağlayan kritik bir özelliktir.

[Subnet,](../../virtual-network/virtual-networks-overview.md) IP aralığına dayalı sanal ağda ek bir yalıtım katmanı sunar. Sanal ağdaki IP adresleri, sanal bir ağı kuruluş ve güvenlik için birden çok alt ağa bölebilirsiniz. Bir sanal ağ içindeki alt ağlara (aynı veya farklı) dağıtılan VM'ler ve PaaS rolü örnekleri, ek bir yapılandırma gerektirmeden birbirleriyle iletişim kurabilir. [Ayrıca, Ağ güvenliği grubunu (NSGs) NSG'nin](../../virtual-network/virtual-networks-overview.md) erişim denetim listesinde (ACL) yapılandırılan kurallara göre ağ trafiğine izin verecek veya vm örneğine karşı reddedebilir şekilde yapılandırabilirsiniz. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur.

## <a name="next-steps"></a>Sonraki Adımlar

- Windows [Azure Sanal Ağlardaki Makineler için Ağ Yalıtım Seçenekleri](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)hakkında bilgi edinin. Buna, belirli bir arka uç ağındaki veya alt ağdaki makinelerin yalnızca belirli istemcilerin veya diğer bilgisayarların izin veren IP adresleri listesine dayalı olarak belirli bir bitiş noktasına bağlanmasına izin verebileceği klasik ön uç ve arka uç senaryosu dahildir.

- [Azure'da sanal makine yalıtımı](../../virtual-machines/windows/isolation.md)hakkında bilgi edinin. Azure Compute, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar.
