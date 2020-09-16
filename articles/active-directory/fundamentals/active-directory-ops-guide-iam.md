---
title: Azure Active Directory kimlik ve erişim yönetimi işlemleri başvuru kılavuzu
description: Bu işlemler başvuru kılavuzu, kimlik ve erişim yönetimi işlemlerini güvenli hale getirmek için gerçekleştirmeniz gereken denetimleri ve eylemleri açıklar
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
ms.openlocfilehash: 2312befa5fe534cc2042b7586755ac5322d036db
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90601324"
---
# <a name="azure-active-directory-identity-and-access-management-operations-reference-guide"></a>Azure Active Directory kimlik ve erişim yönetimi işlemleri başvuru kılavuzu

[Azure AD işlemler başvuru kılavuzu](active-directory-ops-guide-intro.md) 'nun bu bölümünde, kimliklerin ve atamalarının yaşam döngüsünü güvenli hale getirmek ve yönetmek için göz önünde bulundurmanız gereken denetimler ve eylemler açıklanır.

> [!NOTE]
> Bu öneriler, yayımlama tarihi itibariyle geçerli olmakla kalmaz, zaman içinde değişebilir. Kuruluşlar, Microsoft ürün ve Hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Anahtar işletimsel işlemler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipler atama

Azure Active Directory Yönetimi, önemli işlem görevlerinin ve bir dağıtım projesinin parçası olmayan işlemlerin sürekli yürütülmesini gerektirir. Ortamınızı sürdürmek için bu görevleri ayarlamanız de önemlidir. Önemli görevler ve bunların önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| Azure abonelikleri oluşturma işlemini tanımlama | Kuruluşa göre farklılık gösterir |
| Kimlerin Enterprise Mobility + Security Lisans aldığından karar verme | IAM Işlemler ekibi |
| Kimlerin Microsoft 365 lisans aldığından karar verme | Üretkenlik ekibi |
| Dynamics, Visual Studio Codespaces gibi başka lisanslar kimin kime alan hakkında karar verme | Uygulama sahibi |
| Lisans atama | IAM Işlemler ekibi |
| Lisans atama hatalarını giderin ve düzeltin | IAM Işlemler ekibi |
| Azure AD 'de uygulamalara kimlik sağlama | IAM Işlemler ekibi |

Listenizi gözden geçirdikten sonra, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilere göre hizalanmamış olan sahiplerin sahipliğini ayarlamanız gerekebilir.

#### <a name="assigning-owners-recommended-reading"></a>Okuma için önerilen sahipler atanıyor

- [Azure Active Directory’de yönetici rolü atama](../users-groups-roles/directory-assign-admin-roles.md)
- [Azure’da idare](../../governance/index.yml)

## <a name="on-premises-identity-synchronization"></a>Şirket içi kimlik eşitleme

### <a name="identify-and-resolve-synchronization-issues"></a>Eşitleme sorunlarını tanımla ve çözümle

Microsoft, şirket içi ortamınızdaki sorunları, buluta eşitleme sorunlarına yol açacak şekilde iyi bir taban çizgisi ve öğrenmenizi önerir. [Iddüzeltmesini](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) ve [Azure AD Connect Health](../hybrid/whatis-azure-ad-connect.md#why-use-azure-ad-connect-health) gibi otomatikleştirilmiş araçlar yüksek hacimli hatalı pozitif sonuçlar üretebileceğinden, hata durumunda bu nesneleri temizleyerek 100 günden daha uzun süredir ayrıldığımız eşitleme hatalarını tanımlamanızı öneririz. Uzun süreli çözümlenmemiş Eşitleme hataları, destek olayları oluşturabilir. [Eşitleme sırasında oluşan sorunları gidermek](../hybrid/tshoot-connect-sync-errors.md) , farklı eşitleme hatası türlerine genel bir bakış sağlar, bu hatalara neden olan olası senaryoların bazıları ve hataları gidermek için olası yollar sunar.

### <a name="azure-ad-connect-sync-configuration"></a>Azure AD Connect eşitleme yapılandırması

Tüm hibrit deneyimlerini, cihaz tabanlı güvenlik duruşunu ve Azure AD ile tümleştirmeyi etkinleştirmek için, çalışanlarınızın masaüstlerinde oturum açmak üzere kullandığı kullanıcı hesaplarını eşitlemeniz gerekir.

Kullanıcıların oturum açmasını eşitlemediğinizden, doğru ormandan gelmesi için eşitlemeyi değiştirmelisiniz.

#### <a name="synchronization-scope-and-object-filtering"></a>Eşitleme kapsamı ve nesne filtreleme

Eşitlenmesi gerekmeyen nesnelerin bilinen demetlerinin kaldırılması aşağıdaki işletimsel avantajlara sahiptir:

- Daha az eşitleme hatası kaynağı
- Daha hızlı eşitleme döngüleri
- Şirket içinden ileriye doğru taşımak için daha az "çöp", örneğin, bulutta alakalı olmayan şirket içi hizmet hesapları için genel adres listesi kirliliğinizi

> [!NOTE]
> Buluta aktarılmamış birçok nesneyi içeri aktardıysanız, OU 'ya veya belirli özniteliklere göre filtrelemeniz gerekir.

Hariç tutulacak nesne örnekleri şunlardır:

- Bulut uygulamaları için kullanılmayan hizmet hesapları
- Kaynaklara erişim izni vermek için kullanılanlar gibi bulut senaryolarında kullanılması amaçlanan gruplar
- Azure AD B2B Işbirliği ile temsil edilecek dış kimlikler olan kullanıcılar veya kişiler
- Çalışanların bulut uygulamalarına, örneğin sunuculardan erişmesini amaçlanmamış bilgisayar hesapları

> [!NOTE]
> Tek bir insan kimliğinde, eski etki alanı geçişi, birleşme veya alım gibi bir türden birden fazla hesap sağlandıysa, yalnızca Kullanıcı tarafından kullanılan hesabı bir günlük esasına göre eşitlemeniz gerekir, örneğin, bilgisayarında oturum açmak için kullandıkları Özellikler.

İdeal olarak, senkronize edilecek nesne sayısını ve kurallarda karmaşıklığın azaltılması arasında bir bakiyeye ulaşmak isteyeceksiniz. Genellikle, OU/kapsayıcı [filtrelemesi](../hybrid/how-to-connect-sync-configure-filtering.md) ve cloudfiltrelenmiş özniteliğiyle basit bir öznitelik eşlemesi arasında bir birleşim, etkili bir filtreleme birleşimidir.

> [!IMPORTANT]
> Üretimde grup filtrelemesi kullanıyorsanız, başka bir filtreleme yaklaşımına geçiş yapmanız gerekir.

#### <a name="sync-failover-or-disaster-recovery"></a>Yük devretme veya olağanüstü durum kurtarmayı eşitleme

Azure AD Connect, sağlama sürecinde bir anahtar rol oynar. Eşitleme sunucusu herhangi bir nedenle çevrimdışı kalırsa, şirket içi değişiklikler bulutta güncelleştirilemez ve kullanıcılar için erişim sorunlarına neden olabilir. Bu nedenle, eşitleme sunucusu çevrimdışı olduktan sonra yöneticilerin eşitlemeyi hızlı bir şekilde sürdürmesini sağlayan bir yük devretme stratejisi tanımlamak önemlidir. Bu stratejileri aşağıdaki kategorilere düşebilir:

- **Azure AD Connect sunucuları hazırlama modunda dağıt** -yöneticinin, hazırlama sunucusunu basit bir yapılandırma anahtarıyla üretime "yükselmesini" sağlar.
- **Sanallaştırma kullanma** -Azure AD Connect bir sanal MAKINEDE (VM) dağıtılmışsa, Yöneticiler sanal makineyi dinamik olarak geçirmek veya hızlı bir şekilde yeniden dağıtmak için sanallaştırma yığınlarından yararlanabilir ve bu nedenle eşitlemeyi sürdürür.

Kuruluşunuzda eşitleme için bir olağanüstü durum kurtarma ve yük devretme stratejisi yoksa, Azure AD Connect hazırlama modunda dağıtmamanız gerekir. Benzer şekilde, üretim ve hazırlama yapılandırmanız arasında bir uyumsuzluk varsa, yazılım sürümleri ve yapılandırmalar dahil olmak üzere üretim yapılandırmasıyla eşleşecek şekilde hazırlık modu Azure AD Connect yeniden temelsiz yapmalısınız.

![Azure AD Connect hazırlama modu yapılandırması ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img1.png)

#### <a name="stay-current"></a>Güncel kalın

Microsoft güncelleştirmeleri düzenli olarak Azure AD Connect. Performans geliştirmelerinden, hata düzeltmelerinden ve her yeni sürümün sağladığı yeni özelliklerden yararlanmak için güncel kalın.

Azure AD Connect sürümünüz altı aydan daha geride olursa en son sürüme yükseltmeniz gerekir.

#### <a name="source-anchor"></a>Kaynak bağlantısı

[Kaynak Bağlayıcısı](../hybrid/plan-connect-design-concepts.md) olarak **MS-DS-ımlarnımlarguıd** kullanmak, ad etki alanı birleştirme/Temizleme, birleşmeler, alımlar ve divestitures içinde ortak olan ormanlar ve etki alanları arasında nesnelerin daha kolay bir şekilde geçirilmesini sağlar.

Şu anda kaynak Bağlayıcısı olarak **Objectguıd** kullanıyorsanız, **MS-DS-ımlıo GUID**kullanarak geçiş yapmanızı öneririz.

#### <a name="custom-rules"></a>Özel kurallar

Azure AD Connect özel kurallar, şirket içi nesneler ve bulut nesneleri arasındaki özniteliklerin akışını denetleme olanağı sağlar. Ancak, özel kuralların aşırı kullanılması veya yanlış kullanılması aşağıdaki riskleri ortaya çıkarabilir:

- Sorun giderme karmaşıklığı
- Nesneler arasında karmaşık işlemler gerçekleştirirken performansın azalmasına
- Üretim sunucusu ve hazırlama sunucusu arasında yapılandırmanın ayırt olma olasılığı daha yüksektir
- 100 'den büyük öncelik içinde (yerleşik kurallar tarafından kullanılır) özel kurallar oluşturulduysa, yükseltme sırasında ek yük Azure AD Connect.

Aşırı karmaşık kurallar kullanıyorsanız karmaşıklık için nedenleri araştırmalı ve basitme için fırsatları bulabilirsiniz. Benzer şekilde, 100 üzerinde öncelik değeri olan özel kurallar oluşturduysanız, kuralları risk altında olmayan veya varsayılan küme ile çakışacak şekilde düzeltmelisiniz.

Özel kuralların yanlış kullanılmasına örnek olarak şunlar verilebilir:

- **Dizindeki kirli verilerin dengeleniyor** -bu durumda, ad ekibinin sahipleriyle çalışmanız ve dizindeki verileri bir düzeltme görevi olarak temizlemeniz ve hatalı verilerin yeniden giriş yapmaktan kaçınmak için süreçler ayarlamanız önerilir.
- **Bireysel kullanıcılar Için tek bir düzeltme** -belirli bir kullanıcı ile ilgili bir sorun nedeniyle genellikle özel büyük/küçük harf aykırı kuralları bulmak yaygındır.
- **Aşırı karmaşık "CloudFiltering"** -nesne sayısını azaltmak iyi bir uygulamadır, çok sayıda eşitleme kuralını kullanarak eşitleme kapsamı oluşturma ve daha karmaşık hale gelme riski vardır. Nesneleri OU filtrelemenin ötesinde dahil etmek/hariç tutmak için karmaşık mantık varsa, bu mantığla eşitleme dışında ve basit bir eşitleme kuralıyla akabilir basit bir "Cloudfiltrelenmiş" özniteliğiyle birlikte etiketlemek önerilir.

#### <a name="azure-ad-connect-configuration-documenter"></a>Azure AD Connect yapılandırma belgegir

[Azure AD Connect yapılandırma belgebir](https://github.com/Microsoft/AADConnectConfigDocumenter) Aracı, eşitleme yapılandırmasının daha iyi bir şekilde anlaşılmasına, şeyleri doğru şekilde öğrenilmesine ve yeni bir derleme veya yapılandırma veya Azure AD Connect ya da güncelleştirilmiş özel eşitleme kuralları uyguladığınızda nelerin değiştirildiğini bilmeniz için Azure AD Connect kullanabileceğiniz bir araçtır. Aracın geçerli özellikleri şunlardır:

- Azure AD Connect eşitleme 'nin tüm yapılandırmasına ilişkin belgeler.
- İki Azure AD Connect eşitleme sunucusunun yapılandırmasındaki tüm değişikliklere veya belirli bir yapılandırma temeliyle yapılan değişikliklere ilişkin belgeler.
- Eşitleme kuralı farklarını veya özelleştirmelerini bir sunucudan diğerine geçirmek için bir PowerShell dağıtım betiği oluşturma.

## <a name="assignment-to-apps-and-resources"></a>Uygulamalara ve kaynaklara atama

### <a name="group-based-licensing-for-microsoft-cloud-services"></a>Microsoft bulut hizmetleri için grup tabanlı lisanslama

Azure Active Directory, Microsoft bulut hizmetleri için [grup tabanlı lisanslama](./active-directory-licensing-whatis-azure-portal.md) aracılığıyla lisansların yönetimini kolaylaştırır. Bu şekilde, ıAM grup altyapısını ve bu grupların kurumlardaki uygun takımlara Temsilcili olarak yönetilmesini sağlar. Azure AD 'de grupların üyeliğini ayarlamak için çeşitli yollar vardır; örneğin:

- Şirket **içi dizinlerden eşitlenmiş** , şirket içi dizinlerden gelebilir ve bu, Microsoft 365 lisans atamak üzere genişletilebilen Grup Yönetimi işlemlerine sahip kuruluşlara iyi bir uyum sağlayabilir.

- **Öznitelik tabanlı/dinamik** gruplar, Kullanıcı özniteliklerine dayalı bir ifadeye göre bulutta oluşturulabilir, örneğin, departman "Sales" değerine eşittir. Azure AD, grup üyelerini korur, bu, tanımlanan ifadeyle tutarlı kalmasını sağlar. Lisans ataması için bu tür bir grup kullanılması, dizinde yüksek veri kalitesi olan kuruluşlara uygun olan öznitelik tabanlı bir lisans atamasını mümkün hale getiriyor.

- **Temsil edilen sahiplik** -gruplar bulutta oluşturulabilir ve bu sahipler belirlenebilir. Bu şekilde, iş sahiplerini, örneğin, Işbirliği ekibinin veya bı ekibinin erişimine sahip olduğunu tanımlamak için güçlendirin.

Şu anda kullanıcılara lisans ve bileşen atamak için el ile gerçekleştirilen bir işlem kullanıyorsanız, grup tabanlı lisans uygulamanız önerilir. Geçerli işleminiz lisanslama hatalarını veya ne tür bir kullanılabilir olduğunu izlemez, lisans hatalarını ele almak ve lisanslama atamasını izlemek için işlem geliştirmeleri tanımlamanız gerekir.

Lisans yönetiminin başka bir yönü, kuruluştaki iş işlevleri temelinde etkinleştirilmesi gereken hizmet planlarının (lisansın bileşenleri) tanımıdır. Gerekli olmayan hizmet planlarına erişim izni verilmesi, kullanıcıların Office portalında eğitildikleri veya kullanılmamalıdır. Ek Yardım Masası hacmi, gereksiz sağlama ve uyumluluk ve idare işlemlerini riske sokma, örneğin OneDrive Iş 'i içerik paylaşma izni olmayan bireyler için sağlama.

Kullanıcılara hizmet planlarını tanımlamak için aşağıdaki yönergeleri kullanın:

- Yöneticiler, kullanıcılara, örneğin, beyaz katların yanı ve alt düzey işçileriyle ilgili olarak sunulacak hizmet planlarının "paketlerini" tanımlamalıdır.
- Kümeye göre gruplar oluşturun ve Lisans hizmetini hizmet planına atayın.
- İsteğe bağlı olarak, bir öznitelik kullanıcıları için paketleri tutacak şekilde tanımlanabilir.

> [!IMPORTANT]
> Azure AD 'de grup tabanlı lisanslama, Kullanıcı kavramını bir lisanslama hata durumunda sunmaktadır. Lisanslama hatalarıyla karşılaşırsanız, lisans atama sorunlarını hemen [belirleyip çözmeniz](../users-groups-roles/licensing-groups-resolve-problems.md) gerekir.

![Otomatik olarak oluşturulan bilgisayar ekranı açıklamasının ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img2.png)

#### <a name="lifecycle-management"></a>Yaşam döngüsü yönetimi

Şu anda [Microsoft Identity Manager](/microsoft-identity-manager/) veya üçüncü taraf sistem gibi bir aracı kullanıyorsanız, bir şirket içi altyapıya bağlı olan, var olan araçtan atamayı boşaltmanız, grup tabanlı lisans uygulamanız ve [grupları](../users-groups-roles/licensing-group-advanced.md#use-group-based-licensing-with-dynamic-groups)temel alan bir grup yaşam döngüsü yönetimi tanımlamanız önerilir. Benzer şekilde, mevcut işleminiz kuruluştan ayrılmayan yeni çalışanlar veya çalışanlar için hesap içermiyorsa, dinamik gruplara dayalı olarak grup tabanlı lisanslama dağıtmanız ve bir grup üyeliği yaşam döngüsü tanımlamanız gerekir. Son olarak, grup tabanlı lisanslama yaşam döngüsü yönetimi olmayan şirket içi gruplara karşı dağıtılırsa, temsilci olarak sahiplik veya öznitelik tabanlı dinamik üyelik gibi özellikleri etkinleştirmek için bulut grupları kullanmayı göz önünde bulundurun.

### <a name="assignment-of-apps-with-all-users-group"></a>"Tüm kullanıcılar" grubu ile uygulamaların atanması

Kaynak sahipleri, **tüm kullanıcılar** grubunun yalnızca **Kurumsal çalışanları** ve **konukları**gerçekten içerdiğinde **Kurumsal çalışanları** içerdiğini düşünmeyebilir. Sonuç olarak, uygulama ataması için **tüm kullanıcılar** grubunu kullanırken ve SharePoint içeriği veya uygulamalar gibi kaynaklara erişim verirken özel bir dikkatli olmanız gerekir.

> [!IMPORTANT]
> **Tüm kullanıcılar** grubu etkinleştirilir ve koşullu erişim ilkeleri, uygulama veya kaynak ataması için kullanılırsa, Konuk kullanıcıları dahil etmek istemiyorsanız [grubun güvenli hale](../external-identities/use-dynamic-groups.md) geldiğinden emin olun. Ayrıca, yalnızca **Kurumsal çalışanları** içeren grupları oluşturup atayarak lisanslama atamalarınızı düzeltmelisiniz. Öte yandan, **tüm kullanıcılar** grubunun etkin olduğunu ancak kaynaklara erişim izni vermek için kullanılmadığını fark ederseniz, kuruluşunuzun işletimsel kılavuzunun bu grubu kasıtlı olarak ( **Kurumsal çalışanları** ve **konukları**dahil) kullanmak için olduğundan emin olun.

### <a name="automated-user-provisioning-to-apps"></a>Uygulamalara otomatik Kullanıcı hazırlama

Uygulamalara [Otomatik Kullanıcı hazırlama](../app-provisioning/user-provisioning.md) , birden çok sistemde tutarlı bir sağlama, sağlamayı kaldırma ve kimlik yaşam döngüsü oluşturmanın en iyi yoludur.

Şu anda uygulamaları geçici bir şekilde temin ediyorsanız veya CSV dosyaları, JıT veya yaşam döngüsü yönetimine yönelik bir şirket içi çözüm gibi işlemleri kullanıyorsanız, desteklenen uygulamalar için Azure AD ile [uygulama sağlamayı uygulamanızı](../app-provisioning/user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) ve henüz Azure AD tarafından desteklenmeyen uygulamalar için tutarlı bir model tanımlamanızı öneririz.

![Azure AD sağlama hizmeti](./media/active-directory-ops-guide/active-directory-ops-img3.png)

### <a name="azure-ad-connect-delta-sync-cycle-baseline"></a>Azure AD Connect Delta eşitleme döngüsünü temel

Kuruluşunuzdaki değişikliklerin hacminin anlaşılması ve tahmin edilebilir bir eşitleme süresi olması çok uzun sürmediğinden emin olmak önemlidir.

[Varsayılan Delta eşitleme](../hybrid/how-to-connect-sync-feature-scheduler.md) sıklığı 30 dakikadır. Delta eşitlemesi 30 dakikadan sürekli olarak sürüyorsa veya hazırlama ve üretimin Delta eşitleme performansı arasında önemli tutarsızlıklar varsa, [Azure AD Connect performansını etkileyen faktörleri](../hybrid/plan-connect-performance-factors.md)araştırıp gözden geçirmeniz gerekir.

#### <a name="azure-ad-connect-troubleshooting-recommended-reading"></a>Azure AD Connect için önerilen okuma sorunlarını giderme

- [Iddüzeltmesini aracını kullanarak Microsoft 365 ile eşitleme için Dizin özniteliklerini hazırlama](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix)
- [Azure AD Connect: eşitleme sırasında oluşan sorunları giderme](../hybrid/tshoot-connect-sync-errors.md)

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısının beş yönü vardır. Bu liste, kuruluşunuzdaki kimlik yaşam döngüsünü ve bunların haklarını güvenli hale getirmek ve yönetmek için gerekli eylemleri hızlı bir şekilde bulmanıza ve yönetmenize yardımcı olur.

- Anahtar görevlere sahip atayın.
- Eşitleme sorunlarını bulun ve çözümleyin.
- Olağanüstü durum kurtarma için bir yük devretme stratejisi tanımlayın.
- Lisansların ve uygulama atamasının yönetimini kolaylaştırın.
- Uygulamalara Kullanıcı sağlamayı otomatikleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik doğrulama Yönetimi denetimleri ve eylemleri](active-directory-ops-guide-auth.md)ile çalışmaya başlayın.