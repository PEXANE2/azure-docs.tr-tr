---
title: Azure AD Etki Alanı Hizmetleri için yönetim kavramları | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanının nasıl yönetilenve kullanıcı hesaplarının ve parolaların davranışı hakkında bilgi edinin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: iainfou
ms.openlocfilehash: 682935fa2324b8de4992ab2f90c7f71e05c4f8ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264238"
---
# <a name="management-concepts-for-user-accounts-passwords-and-administration-in-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetlerinde kullanıcı hesapları, parolalar ve yönetim için yönetim kavramları

Azure Active Directory Etki Alanı Hizmetleri (AD DS) yönetilen bir etki alanı oluşturup çalıştırdığınızda, geleneksel şirket içi AD DS ortamıyla karşılaştırıldığında davranış ta bazı farklılıklar vardır. Azure AD DS'de aynı yönetim araçlarını kendi kendini yöneten bir etki alanı olarak kullanırsınız, ancak etki alanı denetleyicilerine (DC) doğrudan erişemezsiniz. Ayrıca, kullanıcı hesabı oluşturmanın kaynağına bağlı olarak parola ilkeleri ve parola kalıpları için davranış farklılıkları da vardır.

Bu kavramsal makalede, Azure AD DS yönetilen etki alanının nasıl yönetilen şekli ve kullanıcı hesaplarının oluşturulma şekillerine bağlı olarak farklı davranışları ayrıntılı olarak açıklanmaktadır.

## <a name="domain-management"></a>Etki alanı yönetimi

Azure AD DS'de, kullanıcılar ve gruplar, kimlik bilgileri ve ilkeler gibi tüm kaynakları içeren etki alanı denetleyicileri (DC'ler) yönetilen hizmetin bir parçasıdır. Artıklık için, Azure AD DS yönetilen etki alanının bir parçası olarak iki DC oluşturulur. Yönetim görevlerini gerçekleştirmek için bu DC'lerde oturum açamazsınız. Bunun yerine, Azure AD DS yönetilen etki alanına katılan bir yönetim VM'si oluşturun ve ardından normal AD DS yönetim araçlarınızı yükleyin. Örneğin, DNS veya Grup İlkesi nesneleri gibi Active Directory Administrative Center veya Microsoft Management Console (MMC) snap-ins'i kullanabilirsiniz.

## <a name="user-account-creation"></a>Kullanıcı hesabı oluşturma

Kullanıcı hesapları Azure AD DS'de birden çok şekilde oluşturulabilir. Çoğu kullanıcı hesabı Azure AD'den eşitlenir ve şirket içi AD DS ortamından senkronize edilmiş kullanıcı hesabı da içerebilir. Ayrıca doğrudan Azure AD DS'de el ile hesap oluşturabilirsiniz. İlk parola eşitleme veya parola ilkesi gibi bazı özellikler, kullanıcı hesaplarının nasıl ve nerede oluşturulduğuna bağlı olarak farklı şekilde davranılır.

* Kullanıcı hesabı Azure AD'den senkronize edilebilir. Buna, doğrudan Azure AD'de oluşturulan yalnızca bulutkullanıcı hesapları ve Azure AD Connect'i kullanarak şirket içi AD DS ortamından senkronize edilen karma kullanıcı hesapları dahildir.
    * Azure AD DS'deki kullanıcı hesaplarının çoğu, Azure AD'nin eşitleme işlemi yle oluşturulur.
* Kullanıcı hesabı, Azure AD DS yönetilen bir etki alanında el ile oluşturulabilir ve Azure AD'de bulunmaz.
    * Yalnızca Azure AD DS'de çalışan uygulamalar için hizmet hesapları oluşturmanız gerekiyorsa, bunları yönetilen etki alanında el ile oluşturabilirsiniz. Eşitleme Azure AD'den bir yol olduğundan, Azure AD DS'de oluşturulan kullanıcı hesapları Azure AD ile eşitlenmez.

## <a name="password-policy"></a>Parola ilkesi

Azure AD DS, hesap kilitleme, maksimum parola yaşı ve parola karmaşıklığı gibi nesnelerin ayarlarını tanımlayan varsayılan bir parola ilkesi içerir. Hesap kilitleme ilkesi gibi ayarlar, kullanıcının önceki bölümde belirtildiği gibi nasıl oluşturulduğuna bakılmaksızın Azure AD DS'deki tüm kullanıcılar için geçerlidir. Minimum parola uzunluğu ve parola karmaşıklığı gibi birkaç ayar yalnızca doğrudan Azure AD DS'de oluşturulan kullanıcılar için geçerlidir.

Azure AD DS'deki varsayılan ilkeyi geçersiz kılmak için kendi özel parola ilkelerinizi oluşturabilirsiniz. Bu özel ilkeler daha sonra gerektiğinde belirli kullanıcı gruplarına uygulanabilir.

Parola ilkelerinin kullanıcı oluşturma kaynağına bağlı olarak nasıl uygulandığı konusundaki farklar hakkında daha fazla bilgi için, [yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri][password-policy]bölümüne bakın.

## <a name="password-hashes"></a>Parola hashes

Azure AD DS, yönetilen etki alanında kullanıcıların kimliğini doğrulamak için NT LAN Yöneticisi (NTLM) ve Kerberos kimlik doğrulaması için uygun bir biçimde parola hashes'e ihtiyaç duyar. Azure AD, kiracınız için Azure AD DS'yi etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gerekli biçimde parola açıkları oluşturmaz veya depolamaz. Azure AD, güvenlik nedeniyle parola kimlik bilgilerini açık metin biçiminde de depolamaz. Bu nedenle, Azure AD bu NTLM veya Kerberos parola hatalarını kullanıcıların varolan kimlik bilgilerini temel alarak otomatik olarak oluşturamaz.

Yalnızca bulutkullanıcıları hesapları için kullanıcıların Azure AD DS'yi kullanabilmeleri için parolalarını değiştirmeleri gerekir. Bu parola değiştirme işlemi, Kerberos ve NTLM kimlik doğrulamasının parola işlenmelerinin Azure AD'de oluşturulmasına ve depolanmasına neden olur.

Azure AD Connect'i kullanarak şirket içi AD DS ortamından senkronize edilen kullanıcılar için [parola açıklarının eşitlenmesine olanak tanır.][hybrid-phs]

> [!IMPORTANT]
> Azure AD Connect, eski parola bağlantı larını yalnızca Azure AD kiracınız için Azure AD DS'yi etkinleştirdiğinizde eşitler. Eski parola açıkları, şirket içi bir AD DS ortamını Azure AD ile eşitlemek için yalnızca Azure AD Connect kullanıyorsanız kullanılmaz.
>
> Eski uygulamalarınız NTLM kimlik doğrulaması veya LDAP basit bağlamaları kullanmıyorsa, Azure AD DS için NTLM şifre karma eşitlemesi devre dışı bırakmanızı öneririz. Daha fazla bilgi için zayıf [şifreleme paketlerini ve NTLM kimlik bilgisi karma senkronizasyonunu devre dışı sayıl.][secure-domain]

Uygun şekilde yapılandırıldıktan sonra, kullanılabilir parola varoşları Azure AD DS yönetilen etki alanında depolanır. Azure AD DS yönetilen etki alanını silerseniz, bu noktada depolanan parola varoşları da silinir. Azure AD'de senkronize edilmiş kimlik bilgisi bilgileri daha sonra bir Azure AD DS yönetilen etki alanı oluşturursanız yeniden kullanılamaz - parola karma eşitlemeyi yeniden yapılandırmanız gerekir. Daha önce etki alanına katılan VM'ler veya kullanıcılar hemen kimlik doğrulaması yapamaz - Azure AD'nin parola hatalarını oluşturması ve yeni Azure AD DS yönetilen etki alanında depolaması gerekir. Daha fazla bilgi için Azure [AD DS ve Azure AD Connect için Parola karma eşitleme işlemine][azure-ad-password-sync]bakın.

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamlarıyla senkronizasyon için yüklenmeli ve yapılandırılmalıdır. Azure AD Connect'i Azure AD DS yönetilen bir etki alanına yüklemek için desteklenmez.

## <a name="forests-and-trusts"></a>Ormanlar ve güvenler

*Orman,* Active Directory Domain Services (AD DS) tarafından bir veya daha fazla *etki alanını*gruplandırmak için kullanılan mantıksal bir yapıdır. Etki alanları daha sonra nesneleri kullanıcı veya gruplar için depolar ve kimlik doğrulama hizmetleri sağlar.

Azure AD DS'de orman yalnızca bir etki alanı içerir. Şirket içi AD DS ormanları genellikle birçok etki alanı içerir. Büyük kuruluşlarda, özellikle birleşme ve satın almalardan sonra, her biri birden çok etki alanı içeren birden çok şirket içi ormanla karşılaşabilirsiniz.

Varsayılan olarak, bir Azure AD DS yönetilen etki alanı *kullanıcı* ormanı olarak oluşturulur. Bu tür orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD'deki tüm nesneleri eşitler. Kullanıcı hesapları, etki alanına katılan bir VM'de oturum açma gibi Azure AD DS yönetilen etki alanına karşı doğrudan kimlik doğrulaması yapabilir. Parola kalıpları eşitlenediğinde ve kullanıcılar akıllı kart kimlik doğrulaması gibi özel oturum açma yöntemleri kullanmadığında kullanıcı ormanı çalışır.

Azure AD DS *kaynak* ormanında, kullanıcılar şirket içi AD DS'lerinden tek yönlü orman *güveni* üzerinden kimlik doğrulaması sağlar. Bu yaklaşımla, kullanıcı nesneleri ve parola kalıpları Azure AD DS ile eşitlenmez. Kullanıcı nesneleri ve kimlik bilgileri yalnızca şirket içi AD DS'de bulunur. Bu yaklaşım, işletmelerin Azure'da LDAPS, Kerberos veya NTLM gibi klasik kimlik doğrulamasına bağlı kaynakları ve uygulama platformlarını barındırmasına olanak tanır, ancak kimlik doğrulama sorunları veya endişeleri kaldırılır. Azure AD DS kaynak ormanları şu anda önizlemede.

Azure AD DS'deki orman türleri hakkında daha fazla [How do forest trusts work in Azure AD DS?][concepts-trust] bilgi için [bkz.][concepts-forest]

## <a name="azure-ad-ds-skus"></a>Azure AD DS SKUs

Azure AD DS'de kullanılabilir performans ve özellikler SKU'ya dayanır. Yönetilen etki alanını oluştururken bir SKU seçersiniz ve yönetilen etki alanı dağıtıldıktan sonra iş gereksinimleriniz değiştikçe SKU'ları değiştirebilirsiniz. Aşağıdaki tabloda kullanılabilir SNU'lar ve aralarındaki farklar özetlenmektedir:

| SKU adı   | Maksimum nesne sayısı | Yedekleme sıklığı | En fazla giden orman güvensayısı |
|------------|----------------------|------------------|----|
| Standart   | Sınırsız            | Her 7 günde bir     | 0  |
| Enterprise | Sınırsız            | Her 3 günde bir     | 5  |
| Premium    | Sınırsız            | Günlük            | 10 |

Bu Azure AD DS SK'larından önce, Azure AD DS yönetilen etki alanındaki nesne (kullanıcı ve bilgisayar hesapları) sayısına dayalı bir faturalandırma modeli kullanılmıştır. Yönetilen etki alanında nesnelerin sayısına göre artık değişken fiyatlandırma yoktur.

Daha fazla bilgi için [Azure AD DS fiyatlandırma sayfasına][pricing]bakın.

### <a name="managed-domain-performance"></a>Yönetilen etki alanı performansı

Etki alanı performansı, bir uygulama için kimlik doğrulamanın nasıl uygulandığına bağlı olarak değişir. Ek bilgi işlem kaynakları sorgu yanıt süresini iyileştirmeye ve eşitleme işlemlerinde harcanan zamanı azaltmaya yardımcı olabilir. SKU düzeyi arttıkça, yönetilen etki alanı için kullanılabilir işlem kaynakları artar. Uygulamalarınızın performansını izleyin ve gerekli kaynakları planlayın.

İşletmeniz veya uygulamanız değişiklik gerektiriyorsa ve Azure AD DS yönetilen etki alanınız için ek bilgi işlem gücüne ihtiyacınız varsa, farklı bir SKU'ya geçebilirsiniz.

### <a name="backup-frequency"></a>Yedekleme sıklığı

Yedekleme sıklığı, yönetilen etki alanının anlık görüntüsünün ne sıklıkta alındığını belirler. Yedeklemeler, Azure platformu tarafından yönetilen otomatik bir işlemdir. Yönetilen etki alanınızla ilgili bir sorun durumunda, Azure desteği yedeklemeden geri almanıza yardımcı olabilir. Eşitleme yalnızca Azure AD'den tek bir şekilde *oluştuğundan,* Azure AD DS yönetilen etki alanındaki sorunlar Azure AD veya şirket içi AD DS ortamlarını ve işlevselliğini etkilemez.

SKU düzeyi arttıkça, bu yedekleme anlık görüntü sıklığı artar. Yönetilen etki alanınız için gerekli yedekleme sıklığını belirlemek için iş gereksinimlerinizi ve kurtarma noktası hedefinizi (RPO) gözden geçirin. İşletmeniz veya uygulama gereksinimleriniz değişirse ve daha sık yedeklemeye ihtiyacınız varsa, farklı bir SKU'ya geçebilirsiniz.

### <a name="outbound-forests"></a>Giden ormanlar

Önceki bölümde, Azure AD DS yönetilen bir etki alanından şirket içi AD DS ortamına (şu anda önizlemede) tek yönlü giden orman güvenleri ayrıntılı olarak açıklanmaktadır. SKU, Azure AD DS yönetilen bir etki alanı için oluşturabileceğiniz maksimum orman güvensayısını belirler. Gerçekte kaç güvene ihtiyacınız olduğunu belirlemek için işletmenizi ve uygulama gereksinimlerinizi gözden geçirin ve uygun Azure AD DS SKU'yu seçin. Yine, iş gereksinimleriniz değişirse ve ek orman güvenleri oluşturmanız gerekiyorsa, farklı bir SKU'ya geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [Azure AD DS yönetilen bir etki alanı oluşturun.][create-instance]

<!-- INTERNAL LINKS -->
[password-policy]: password-policy.md
[hybrid-phs]: tutorial-configure-password-hash-sync.md#enable-synchronization-of-password-hashes
[secure-domain]: secure-your-domain.md
[azure-ad-password-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[create-instance]: tutorial-create-instance.md
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md

<!-- EXTERNAL LINKS -->
[pricing]: https://azure.microsoft.com/pricing/details/active-directory-ds/
