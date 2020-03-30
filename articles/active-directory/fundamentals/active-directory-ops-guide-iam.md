---
title: Azure Active Directory Identity ve erişim yönetimi işlemleri başvuru kılavuzu
description: Bu işlem başvuru kılavuzu, kimlik ve erişim yönetimi işlemlerini güvence altına almak için çekmeniz gereken denetimleri ve eylemleri açıklar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: 5653fa7c67d36dbf2ee71f51f182168bccb69105
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298623"
---
# <a name="azure-active-directory-identity-and-access-management-operations-reference-guide"></a>Azure Active Directory Identity ve erişim yönetimi işlemleri başvuru kılavuzu

Azure AD [işlemleri başvuru kılavuzunun](active-directory-ops-guide-intro.md) bu bölümünde, kimliklerin ve atamalarının yaşam döngüsünü güvenli hale getirmek ve yönetmek için göz önünde bulundurmanız gereken denetimler ve eylemler açıklanmaktadır.

> [!NOTE]
> Bu öneriler yayımlama tarihi itibariyle geçerlidir, ancak zaman içinde değişebilir. Kuruluşlar, Microsoft ürünleri ve hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Temel operasyonel süreçler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipleri atama

Azure Active Directory'nin yönetimi, bir kullanıma sunulması projesinin parçası olmayan önemli operasyonel görevlerin ve işlemlerin sürekli olarak yürütülmesini gerektirir. Ortamınızı korumak için bu görevleri ayarlamanız yine de önemlidir. Önemli görevler ve önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| Azure aboneliklerini oluşturma işlemini tanımlayın | Kuruluşa göre değişir |
| Kurumsal Mobilite + Güvenlik lisanslarını kimin alacağına karar verin | IAM Operasyon Ekibi |
| Office 365 lisanslarını kimin alacağına karar verin | Verimlilik Ekibi |
| Diğer lisansları kimin alacağına karar verin, örneğin, Dynamics, VSO | Uygulama Sahibi |
| Lisans atama | IAM Operasyon Ekibi |
| Sorun giderme ve lisans atama hatalarını düzeltme | IAM Operasyon Ekibi |
| Azure AD'deki uygulamalara kimlik sağlama | IAM Operasyon Ekibi |

Listenizi gözden geçirerken, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilerle uyumlu olmayan sahiplerle olan görevlerin sahipliğini ayarlamanız gerekebilir.

#### <a name="assigning-owners-recommended-reading"></a>Atama sahipleri okuma tavsiye

- [Azure Active Directory’de yönetici rolü atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Azure’da idare](https://docs.microsoft.com/azure/security/governance-in-azure)

## <a name="on-premises-identity-synchronization"></a>Şirket içi kimlik eşitleme

### <a name="identify-and-resolve-synchronization-issues"></a>Eşitleme sorunlarını belirleme ve çözme

Microsoft, şirket içi ortamınızdaki bulutla eşitleme sorunlarına neden olabilecek sorunları iyi bir temele ve anlayışa sahip olduğunuzu önerir. [IdFix](https://docs.microsoft.com/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) ve [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-azure-ad-connect#why-use-azure-ad-connect-health) gibi otomatik araçlar yüksek hacimli yanlış pozitif sonuçlar oluşturabildiği için, bu nesneleri yanlışlıkla temizleyerek 100 günden uzun süredir ele alınmamış eşitleme hatalarını belirlemenizi öneririz. Uzun vadeli çözülmemiş eşitleme hataları destek olayları oluşturabilir. [Eşitleme sırasında hata giderme,](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors) farklı eşitleme hataları türlerine, bu hatalara neden olan olası senaryolara ve hataları düzeltmenin olası yollarına genel bir bakış sağlar.

### <a name="azure-ad-connect-sync-configuration"></a>Azure AD Connect Sync yapılandırması

Tüm karma deneyimleri, aygıt tabanlı güvenlik duruşunu ve Azure AD ile tümleştirmeyi etkinleştirmek için, çalışanlarınızın masaüstünde oturum açmak için kullandıkları kullanıcı hesaplarını eşitlemeniz gerekir.

Orman kullanıcılarının oturum unu eşitlemezseniz, eşitlemeyi uygun ormandan gelecek şekilde değiştirmeniz gerekir.

#### <a name="synchronization-scope-and-object-filtering"></a>Eşitleme kapsamı ve nesne filtreleme

Senkronize edilmesi gerekmeyen bilinen nesne kovalarının kaldırılması aşağıdaki operasyonel avantajlara sahiptir:

- Eşitleme hatalarının sayısı daha az
- Daha hızlı eşitleme döngüleri
- Şirket içinde ileriye taşımak için daha az "çöp", örneğin, bulutta alakalı olmayan şirket içi hizmet hesapları için küresel adres listesinin kirlenmesi

> [!NOTE]
> Buluta dışa aktarılmaması gereken birçok nesne içe aktardığınızı fark ederseniz, OU veya belirli özniteliklere göre filtre lemeniz gerekir.

Dışlatılaması gereken nesnelere örnek olarak şunlar verilebilir:

- Bulut uygulamaları için kullanılmayan Hizmet Hesapları
- Kaynaklara erişim sağlamak için kullanılanlar gibi bulut senaryolarında kullanılmaması gereken gruplar
- Azure AD B2B İşbirliği ile temsil edilmesi gereken dış kimlik olan kullanıcılar veya kişiler
- Çalışanların bulut uygulamalarına erişemelerinin amaçlandığı Bilgisayar Hesapları, örneğin sunuculardan

> [!NOTE]
> Tek bir insan kimliğinin eski bir etki alanı geçişi, birleşme veya satın alma gibi bir şeyden sağlanan birden çok hesabı varsa, kullanıcı tarafından kullanılan hesabı yalnızca günlük bazda eşitlemeniz gerekir, örneğin, bilgisayarında oturum açmak için kullandıkları .

İdeal olarak, eşitlenecek nesne sayısını azaltmakla kurallardaki karmaşıklığı azaltmak arasında bir denge sağlamak isteyeceksiniz. Genellikle, OU/konteyner [filtreleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering) ile cloudFiltered özniteliğine basit bir öznitelik eşleme arasındaki bir kombinasyon etkili bir filtreleme birleşimidir.

> [!IMPORTANT]
> Üretimde grup filtreleme kullanıyorsanız, başka bir filtreleme yaklaşımına geçiş yapmalısınız.

#### <a name="sync-failover-or-disaster-recovery"></a>Eşitleme failover veya olağanüstü durum kurtarma

Azure AD Connect, sağlama işleminde önemli bir rol oynar. Eşitleme Sunucusu herhangi bir nedenle çevrimdışı olursa, şirket içi değişiklikler bulutta güncelleştirilemez ve kullanıcılar için erişim sorunlarına neden olabilir. Bu nedenle, eşitleme sunucusu çevrimdışı olduktan sonra yöneticilerin eşitleme yi hızlı bir şekilde devam ettirmesine olanak tanıyan bir hata giderme stratejisi tanımlamak önemlidir. Bu tür stratejiler aşağıdaki kategorilere düşebilir:

- **Azure AD Connect Server(ları) Evreleme Modunda dağıtın** - bir yöneticinin evreleme sunucusunu basit bir yapılandırma anahtarıyla üretime "tanıtmasına" olanak tanır.
- **Sanallaştırma'yı Kullan** - Azure AD bağlantısı sanal bir makinede (VM) dağıtılırsa, yöneticiler sanallaştırma yığınlarından yararlanıp VM'yi hızlı bir şekilde yeniden dağıtabilir ve bu nedenle eşitleme devam ettirebilirler.

Kuruluşunuz Eşitleme için olağanüstü durum kurtarma ve başarısız olma stratejisinden yoksunsa, Azure AD Connect'i Hazırlama Modunda dağıtmaktan çekinmemelidir. Benzer şekilde, üretim ve hazırlama yapılandırmanız arasında bir uyumsuzluk varsa, yazılım sürümleri ve yapılandırmaları da dahil olmak üzere üretim yapılandırmasına uyacak şekilde Azure AD Connect hazırlama modunu yeniden temele yapıştırmalısınız.

![Azure AD Connect hazırlama modu yapılandırmasının ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img1.png)

#### <a name="stay-current"></a>Güncel kalın

Microsoft, Azure AD Connect'i düzenli olarak güncelleştirir. Performans geliştirmelerinden, hata düzeltmelerinden ve her yeni sürümün sağladığı yeni özelliklerden yararlanmak için güncel kalın.

Azure AD Connect sürümünüz altı aydan uzun bir süre gerideyse, en son sürüme yükseltmeniz gerekir.

#### <a name="source-anchor"></a>Kaynak çapa

[Kaynak çapa](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-design-concepts) olarak **ms-DS tutarlılığının** kullanılması, AD Etki Alanı konsolidasyonu/temizleme, birleşme, satın alma ve elden çıkarmalarda yaygın olan nesnelerin ormanlar ve etki alanları arasında daha kolay geçişini sağlar.

Kaynak bağlantı noktası olarak **ObjectGuid** kullanıyorsanız, **ms-DS-ConsistencyGuid**kullanmaya geçmenizi öneririz.

#### <a name="custom-rules"></a>Özel kurallar

Azure AD Connect özel kuralları, şirket içi nesneler le bulut nesneleri arasındaki özniteliklerin akışını denetleme olanağı sağlar. Ancak, özel kuralların aşırı kullanımı veya kötüye kullanılması aşağıdaki risklere neden olabilir:

- Sorun giderme karmaşıklığı
- Nesneler arasında karmaşık işlemler gerçekleştirirken performansın düşmesi
- Üretim sunucusu ve evreleme sunucusu arasında yapılandırma nın sapma olasılığı daha yüksektir
- Additional overhead when upgrading Azure AD Connect if custom rules are created within the precedence greater than 100 (used by built-in rules)

Aşırı karmaşık kurallar kullanıyorsanız, karmaşıklığın nedenlerini araştırmalı ve basitleştirme fırsatları bulmalısınız. Aynı şekilde, öncelik değeri 100'ün üzerinde olan özel kurallar oluşturduysanız, kuralları varsayılan kümeyle risk altında olmayacak şekilde düzeltmeniz gerekir.

Özel kuralları kötüye kullanmaya örnek olarak şunlar verilebilir:

- **Dizindeki kirli verileri telafi etme** - Bu durumda, AD ekibinin sahipleriyle birlikte çalışması ve dizindeki verileri düzeltme görevi olarak temizlemesi ve hatalı verilerin yeniden girişini önlemek için işlemleri ayarlaması önerilir.
- **Tek tek kullanıcıların bir kerelik düzeltmesi** - Genellikle belirli bir kullanıcıyla ilgili bir sorun nedeniyle, özel durum outliers kuralları bulmak yaygındır.
- **Aşırı karmaşık "CloudFiltering"** - Nesnelerin sayısını azaltmak iyi bir uygulama olsa da, birçok eşitleme kuralı kullanarak karmaşık eşitleme kapsamı oluşturma ve aşırı karmaşık bir risk vardır. OU filtreleme nin ötesinde nesneleri dahil etmek/dışlamak için karmaşık bir mantık varsa, bu mantıkla eşitleme dışında başa çıkmak ve nesneleri basit bir Eşitleme Kuralı ile akabilecek basit bir "cloudFiltered" özniteliğiyle etiketlemeniz önerilir.

#### <a name="azure-ad-connect-configuration-documenter"></a>Azure AD Connect yapılandırma belgeleyicisi

[Azure AD Connect Yapılandırma Belgeleyici,](https://github.com/Microsoft/AADConnectConfigDocumenter) eşitleme yapılandırmasının daha iyi anlaşılmasını sağlamak, işleri doğru yapma güvenini oluşturmak ve Azure AD Connect'in yeni bir yapı sını veya yapılandırmasını uyguladığınızda veya özel eşitleme kuralları eklediğinizde nelerin değiştiğini bilmek için bir Azure AD Connect yüklemesinin belgelerini oluşturmak için kullanabileceğiniz bir araçtır. Aracın geçerli yetenekleri şunlardır:

- Azure AD Connect eşitlemesinin tam yapılandırmasının belgelenmesi.
- İki Azure AD Connect eşitleme sunucusunun yapılandırmasındaki değişikliklerin veya belirli bir yapılandırma taban çizgisindeki değişikliklerin belgelenmesi.
- Eşitleme kuralı farklılıklarını veya özelleştirmeleri bir sunucudan diğerine geçirmek için powershell dağıtım komut dosyası oluşturma.

## <a name="assignment-to-apps-and-resources"></a>Uygulamalara ve kaynaklara atama

### <a name="group-based-licensing-for-microsoft-cloud-services"></a>Microsoft bulut hizmetleri için grup tabanlı lisanslama

Azure Active Directory, Microsoft bulut hizmetleri için [grup tabanlı lisanslama](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal) yoluyla lisansyönetimini kolaylaştırır. Bu şekilde, IAM grup altyapısını ve bu grupların yönetimini kuruluşlardaki uygun ekiplere devretmeyi sağlar. Azure AD'deki grupların üyeliğini ayarlamanın birden çok yolu vardır:

- **Şirket içinde senkronize -** Gruplar, office 365'te lisans atamak için genişletilebilen grup yönetimi süreçleri oluşturmuş kuruluşlar için uygun olabilecek şirket içi dizinlerden gelebilir.

- **Öznitelik tabanlı / dinamik** - Gruplar bulutta kullanıcı özniteliklerine dayalı bir ifadeye dayalı olarak oluşturulabilir, örneğin, Bölüm eşittir "satış". Azure AD, grubun üyelerini tanımlanan ifadeyle tutarlı tutarak korur. Lisans ataması için bu tür bir grubun kullanılması, dizinde yüksek veri kalitesine sahip kuruluşlar için uygun olan öznitelik tabanlı lisans atamasına olanak tanır.

- **Temsilci sahipliği** - Gruplar bulutta oluşturulabilir ve sahip olarak atanabilir. Bu şekilde, işletme sahiplerine, örneğin İşbirliği ekibi veya BI ekibi, kimlere erişmesi gerektiğini tanımlamaları için yetki verebilirsiniz.

Şu anda kullanıcılara lisans ve bileşen atamak için el ile bir işlem kullanıyorsanız, grup tabanlı lisanslama uygulamanızı öneririz. Geçerli işleminiz lisans hatalarını veya Kullanılabilir'e göre atananları izlemiyorsa, lisans hatalarını gidermek ve lisans atamasını izlemek için işlemdeki geliştirmeleri tanımlamanız gerekir.

Lisans yönetiminin bir diğer yönü de, kuruluştaki iş işlevlerine göre etkinleştirilmesi gereken hizmet planlarının (lisans bileşenleri) tanımlanmasıdır. Gerekli olmayan hizmet planlarına erişim izni vermek, kullanıcıların Office portalında kullanmadıkları veya kullanmadıkları araçları görmesine neden olabilir. Ek yardım masası hacmi, gereksiz sağlama sağlayabilir ve uyumluluk ve yönetim risk altına, örneğin, içerik paylaşmak için izin verilmeyebilir bireylere İş için OneDrive sağlama sağlayabilir.

Kullanıcılara hizmet planlarını tanımlamak için aşağıdaki yönergeleri kullanın:

- Yöneticiler, kullanıcılara rollerine göre sunulacak hizmet planlarının "demetlerini" tanımlamalıdır, örneğin, beyaz yakalı işçi ye karşı kat işçisi.
- Kümeye göre gruplar oluşturun ve lisansı hizmet planıyla atayın.
- İsteğe bağlı olarak, kullanıcılar için paketleri tutmak için bir öznitelik tanımlanabilir.

> [!IMPORTANT]
> Azure AD'deki grup tabanlı lisanslama, bir lisans hatası durumundaki kullanıcı kavramını sunar. Herhangi bir lisans hatası fark ederseniz, lisans atama sorunlarını hemen [tanımlamalı ve çözmeniz](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-resolve-problems) gerekir.

![Otomatik olarak oluşturulan bir bilgisayar ekranı Açıklamasının ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img2.png)

#### <a name="lifecycle-management"></a>Yaşam döngüsü yönetimi

Şu anda [Microsoft Identity Manager](https://docs.microsoft.com/microsoft-identity-manager/) veya şirket içi altyapıya dayanan üçüncü taraf sistemi gibi bir araç kullanıyorsanız, varolan araçtan atamayı boşaltmanızı, grup tabanlı lisanslama uygulamanızı ve [gruplara](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-group-advanced#use-group-based-licensing-with-dynamic-groups)dayalı bir grup yaşam döngüsü yönetimi tanımlamanızı öneririz. Aynı şekilde, varolan işleminiz kuruluştan ayrılan yeni çalışanları veya çalışanları hesaba katmıyorsa, dinamik gruplara dayalı grup tabanlı lisanslama dağıtmalı ve bir grup üyeliği yaşam döngüsü tanımlamalısınız. Son olarak, yaşam döngüsü yönetimi olmayan şirket içi gruplara karşı grup tabanlı lisanslama dağıtılırsa, yetki devrine sahip olma veya öznitelik tabanlı dinamik üyelik gibi yetenekleri etkinleştirmek için bulut grupları kullanmayı düşünün.

### <a name="assignment-of-apps-with-all-users-group"></a>"Tüm kullanıcılar" grubuyla uygulamaların atanması

Kaynak sahipleri, **Tüm kullanıcı** grubunun yalnızca **Kurumsal Çalışanlar'ı** içerebileceğine ve hem **Kurumsal Çalışanları** hem de **Konukları**içerebileceğine inanabilir. Sonuç olarak, uygulama ataması için **Tüm kullanıcılar** grubunu kullanırken ve SharePoint içeriği veya uygulamaları gibi kaynaklara erişim sağlarken özel dikkat çekmelisiniz.

> [!IMPORTANT]
> Tüm **kullanıcılar** grubu etkinse ve koşullu erişim ilkeleri, uygulama veya kaynak ataması için kullanılıyorsa, konuk kullanıcıları içermesini istemiyorsanız [grubu güvenli hale](https://docs.microsoft.com/azure/active-directory/b2b/use-dynamic-groups) aldığınızdan emin olun. Ayrıca, yalnızca **Kurumsal Çalışanlar** içeren gruplar oluşturarak ve bu görevlere atayarak lisans atamalarınızı düzeltmelisiniz. Diğer taraftan, **Tüm kullanıcılar** grubunun etkin olduğunu ancak kaynaklara erişim sağlamak için kullanılmadığını fark ederseniz, kuruluşunuzun operasyonel kılavuzunun bu grubu kasıtlı olarak **(hem Kurumsal Çalışanları** hem de **Konukları**içeren) kullanmak üzere olduğundan emin olun.

### <a name="automated-user-provisioning-to-apps"></a>Uygulamalara otomatik kullanıcı sağlama

Uygulamalara [otomatik leştirilmiş kullanıcı sağlama,](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) birden çok sistemde kimliklerin tutarlı bir şekilde sağlanması, deprovisioning ve yaşam döngüsü oluşturmanın en iyi yoludur.

Şu anda uygulamaları geçici bir şekilde oluşturuyorsanız veya CSV dosyaları, JIT veya yaşam döngüsü yönetimine hitap etmeyen şirket içi çözüm gibi şeyleri kullanıyorsanız, desteklenen uygulamalar için Azure AD ile [uygulama sağlama uygulamanızı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#how-do-i-set-up-automatic-provisioning-to-an-application) uygulamanızı ve Azure AD tarafından henüz desteklenmeyen uygulamalar için tutarlı bir desen tanımlamanızı öneririz.

![Azure AD sağlama hizmeti](./media/active-directory-ops-guide/active-directory-ops-img3.png)

### <a name="azure-ad-connect-delta-sync-cycle-baseline"></a>Azure AD Connect delta eşitleme döngüsü taban çizgisi

Kuruluşunuzdaki değişikliklerin hacmini anlamak ve öngörülebilir bir eşitleme süresine sahip olmak çok uzun süreceğinden emin olmak önemlidir.

[Varsayılan delta eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler) frekansı 30 dakikadır. Delta eşitleme tutarlı olarak 30 dakikadan uzun sürüyorsa veya evreleme ve üretim delta eşitleme performansı arasında önemli tutarsızlıklar varsa, [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors)performansını etkileyen faktörleri araştırmalı ve gözden geçirmelisiniz.

#### <a name="azure-ad-connect-troubleshooting-recommended-reading"></a>Azure AD Connect sorun giderme önerilen okuma

- [IdFix aracını kullanarak Office 365 ile senkronizasyon için dizin öznitelikleri ni hazırlama - Office 365](https://docs.microsoft.com/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix)
- [Azure AD Connect: Eşitleme sırasında hata giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sync-errors)

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısının beş yönü vardır. Bu liste, kimliklerin ve bunların kuruluşunuzdaki yetkilerinin yaşam döngüsünü güvenli hale getirmek ve yönetmek için gerekli eylemleri hızlı bir şekilde bulmanıza ve yönetmenize yardımcı olur.

- Önemli görevlere sahipleri atayın.
- Eşitleme sorunlarını bulun ve çözün.
- Olağanüstü durum kurtarma için bir başarısızlık stratejisi tanımlayın.
- Lisansların yönetimini ve uygulamaların atanmasını kolaylaştırın.
- Uygulamalara kullanıcı sağlamayı otomatikleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Kimlik Doğrulama [yönetimi denetimleri ve eylemleri](active-directory-ops-guide-auth.md)ile başlayın.
