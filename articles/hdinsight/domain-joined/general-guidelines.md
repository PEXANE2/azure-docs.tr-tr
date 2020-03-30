---
title: Azure HDInsight'ta kurumsal güvenlik genel yönergeleri
description: Kurumsal Güvenlik Paketi dağıtımı ve yönetimini kolaylaştıran en iyi uygulamalar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: be6c1fdc5deb6d541656c198469822dae0a5f7c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77463212"
---
# <a name="enterprise-security-general-information-and-guidelines-in-azure-hdinsight"></a>Azure HDInsight'ta kurumsal güvenlik genel bilgileri ve yönergeleri

Güvenli bir HDInsight kümesini dağıtırken, dağıtım ve küme yönetimini kolaylaştıracak en iyi uygulamalar vardır. Bazı genel bilgiler ve yönergeler burada tartışılmıştır.

## <a name="use-of-secure-cluster"></a>Güvenli küme kullanımı

### <a name="recommended"></a>Önerilen

* Küme aynı anda birden çok kullanıcı tarafından kullanılacaktır.
* Kullanıcıların aynı verilere farklı erişim düzeyleri vardır.

### <a name="not-necessary"></a>Gerekli değil

* Yalnızca otomatik işleri (tek kullanıcı hesabı gibi) çalıştırabilirsiniz, standart bir küme yeterince iyidir.
* Veri aktarımını standart bir küme kullanarak yapabilir ve aynı depolama hesabını kullanıcıların analitik işlerini çalıştırabileceği farklı bir güvenli kümede kullanabilirsiniz.

## <a name="use-of-local-account"></a>Yerel hesabın kullanımı

* Paylaşılan bir kullanıcı hesabı veya yerel bir hesap kullanıyorsanız, config veya hizmeti değiştirmek için hesabı kimin kullandığını belirlemek zor olacaktır.
* Kullanıcılar artık kuruluşun bir parçası olmadığında yerel hesapların kullanılması sorunludur.

## <a name="ranger"></a>Ranger

### <a name="policies"></a>İlkeler

* Varsayılan olarak, Ranger ilke olarak **Reddet** kullanır.

* Yetkilendirmenin etkinleştirildiği bir hizmet aracılığıyla veri erişimi yapıldığında:
  * Ranger yetki eklentisi çağrılır ve istek bağlamında verilir.
  * Ranger, hizmet için yapılandırılan ilkeleri uygular. Ranger ilkeleri başarısız olursa, erişim denetimi dosya sistemine ertelenir. MapReduce gibi bazı hizmetler yalnızca dosyanın/klasörün isteği gönderen kullanıcıya ait olup olmadığını denetler. Hive gibi hizmetler, ya mülkiyet eşleşmesi veya`rwx`uygun dosya sistemi izinleri için kontrol ( ).

* Hive için, Oluşturma / Güncelleme / Silme izinleri yapmak için izinlere `rwx`sahip olmanın yanı sıra, kullanıcı depolama ve tüm alt dizinler üzerinde dizini üzerinde izinleri olmalıdır.

* İlkeler bireyler yerine gruplara (tercih edilir) uygulanabilir.

* Ranger yetkili her istek için bu hizmet için tüm Ranger politikalarını değerlendirecektir. Bu değerlendirme, işi veya sorguyu kabul etme süresini etkileyebilir.

### <a name="storage-access"></a>Depolama erişimi

* Depolama türü WASB ise, o zaman hiçbir OAuth belirteci söz konusu değildir.
* Ranger yetkilendirmeyi gerçekleştirmişse, depolama erişimi Yönetilen Kimlik kullanılarak gerçekleşir.
* Ranger herhangi bir yetkilendirme gerçekleştiremediyse, depolama erişimi kullanıcının OAuth belirteci kullanılarak gerçekleşir.

### <a name="hierarchical-name-space"></a>Hiyerarşik ad alanı

Hiyerarşik ad alanı etkinleştirildiğinde:

* Devralınan izin ler yok.
* Yalnızca çalışan dosya sistemi izni, doğrudan Azure portalında kullanıcıya atanmak üzere **Depolama Verileri XXXX** RBAC rolüdür.

### <a name="default-hdfs-permissions"></a>Varsayılan HDFS izinleri

* Varsayılan olarak, kullanıcıların HDFS **/** klasörüne erişimi yoktur (başarılı olmak için depolama blob sahibi rolünde olmaları gerekir).
* Mapreduce ve diğerleri için evreleme dizini için, kullanıcıya `sticky _wx` özel bir dizin oluşturulur ve izinler sağlanır. Kullanıcılar altında dosya ve klasörler oluşturabilir, ancak diğer öğelere bakamaz.

### <a name="url-auth"></a>URL auth

Url auth etkinse:

* Config, url auth'ta (gibi) `adl://`hangi öneklerin kapsanan önekleri içerecektir.
* Erişim bu url içinise, Ranger kullanıcının izin listesinde olup olmadığını denetler.
* Ranger ince taneli poliçelerin hiçbirini kontrol etmeyecek.

## <a name="resource-groups"></a>Kaynak grupları

Küme kaynakları arasında ayrım yapabilmek için her küme için yeni bir kaynak grubu kullanın.

## <a name="nsgs-firewalls-and-internal-gateway"></a>NSG'ler, güvenlik duvarları ve dahili ağ geçidi

* Sanal ağları kilitlemek için ağ güvenlik gruplarını (NSGs) kullanın.
* Giden erişim ilkelerini işlemek için güvenlik duvarLarını kullanın.
* Genel internete açık olmayan dahili ağ geçidini kullanın.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) (Azure AD), Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir.

### <a name="policies"></a>İlkeler

* IP adresi tabanlı ilkesini kullanarak koşullu erişim ilkesini devre dışı kılabilir. Bu, kümelerin dağıtıldığı VNET'lerde hizmet uç noktalarının etkinleştirilmesini gerektirir. MFA için harici bir hizmet kullanıyorsanız (AAD dışında bir şey), IP adresi tabanlı ilke çalışmaz

* `AllowCloudPasswordValidation`politika federe kullanıcılar için gereklidir. HDInsight, Azure AD'den jeton almak için doğrudan kullanıcı adını/ parolayı kullandığından, bu ilkenin tüm federe kullanıcılar için etkinleştirilmesi gerekir.

* Güvenilen IP'leri kullanarak koşullu erişim bypass'ı gerektiriyorsanız hizmet bitiş noktalarını etkinleştirin.

### <a name="groups"></a>Gruplar

* Kümeleri her zaman grupla dağıtın.
* Grup üyeliklerini yönetmek için Azure AD'yi kullanın (kümedeki tek tek hizmetleri yönetmeye çalışmaktan daha kolay).

### <a name="user-accounts"></a>Kullanıcı hesapları

* Her senaryo için benzersiz bir kullanıcı hesabı kullanın. Örneğin, alma için bir hesap kullanın, sorgu veya diğer işleme işleri için başka bir kullanın.
* Tek tek ilkeler yerine grup tabanlı Ranger ilkelerini kullanın.
* Artık kümelere erişimi olmaması gereken kullanıcıları nasıl yöneteceklerine ilişkin bir planınız var.

## <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services

[Azure Active Directory Etki Alanı Hizmetleri](../../active-directory-domain-services/overview.md) (Azure AD DS), etki alanı birleştirme, grup ilkesi, hafif dizin erişim protokolü (LDAP) ve Windows Server Active Directory ile tam uyumlu Kerberos / NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar.

Güvenli kümelerin bir etki alanına katılması için Azure AD DS gereklidir.
HDInsight, çok fazla hata noktası, kimlik bilgisi paylaşımı, DNS izinleri ve benzeri konularda çok fazla hata noktası, kimlik bilgisi paylaşımı ve benzeri konularda şirket içi etki alanı denetleyicilerine veya özel etki alanı denetleyicilerine bağlı olamaz. Daha fazla bilgi için [Azure AD DS SS SS'lerine](../../active-directory-domain-services/faqs.md)bakın.

### <a name="azure-ad-ds-instance"></a>Azure AD DS örneği

* 'yi ile `.onmicrosoft.com domain`örneği oluşturun. Bu şekilde, etki alanına hizmet veren birden çok DNS sunucusu olmaz.
* LDAPS için kendi imzalı bir sertifika oluşturun ve Azure AD DS'ye yükleyin.
* Kümeleri dağıtmak için eşlenmiş bir sanal ağ kullanın (HDInsight ESP kümelerini dağıtan çok sayıda takım varsa, bu yararlı olacaktır). Bu, etki alanı denetleyicisi ile sanal ağda bağlantı noktaları (NSGs) açmanız gerekmez.
* Sanal ağ için DNS'yi düzgün bir şekilde yapılandırın (Azure AD DS etki alanı adı, ana bilgisayar dosya girişleri olmadan çözülmelidir).
* Giden trafiği kısıtlıyorsanız, [HDInsight'taki güvenlik duvarı desteğini](../hdinsight-restrict-outbound-traffic.md) okuduğunuzdan emin olun

### <a name="properties-synced-from-azure-ad-to-azure-ad-ds"></a>Azure AD'den Azure AD DS'ye senkronize edilen özellikler

* Azure AD, senkronize leri şirket içinde Azure AD'ye bağlar.
* Azure AD DS senkronize leri Azure AD'den.

Azure AD DS, Azure AD'deki nesneleri düzenli aralıklarla eşitler. Azure portalındaki Azure AD DS bıçak eşitleme durumunu görüntüler. Eşitlemenin her aşamasında, benzersiz özellikler çakışabilir ve yeniden adlandırılabilir. Azure AD'den Azure AD DS'ye özellik eşlenemesine dikkat edin.

Daha fazla bilgi için Azure [AD UserPrincipalName popülasyonuna](../../active-directory/hybrid/plan-connect-userprincipalname.md)ve [Azure AD DS eşitlemenin nasıl çalıştığına](../../active-directory-domain-services/synchronization.md)bakın.

### <a name="password-hash-sync"></a>Parola karma eşitleme

* Parolalar diğer nesne türlerinden farklı eşitlenir. Azure AD ve Azure AD DS'de yalnızca geri döndürülemez parola kalıpları eşitlenir
* Azure AD'ye şirket içi REKLAM Bağlantısı aracılığıyla etkinleştirilmelidir
* Azure AD ile Azure AD DS eşitleme otomatiktir (gecikmeler 20 dakikanın altındadır).
* Parola varoşları yalnızca değiştirilen bir parola olduğunda eşitlenir. Parola karma eşitlemesini etkinleştirdiğinizde, varolan tüm parolalar geri dönülmez şekilde depolanırken otomatik olarak eşitlenmez. Parolayı değiştirdiğinizde, parola hashes eşitlenir.

### <a name="computer-objects-location"></a>Bilgisayar nesneleri konumu

Her küme tek bir OU ile ilişkilidir. Bir iç kullanıcı OU'da verilmiştir. Tüm düğümler etki alanı aynı OU katıldı.

### <a name="active-directory-administrative-tools"></a>Aktif Dizin yönetim araçları

Etkin Dizin yönetim araçlarının Windows Server VM'ye nasıl yükleniryardımcı olduğu yla ilgili adımlar [için](../../active-directory-domain-services/tutorial-create-management-vm.md)bkz.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="cluster-creation-fails-repeatedly"></a>Küme oluşturma tekrar tekrar başarısız olur

En yaygın nedenleri:

* DNS yapılandırması doğru değil, küme düğümlerinin etki alanı birleştirme başarısız olur.
* NSG'ler çok kısıtlayıcıdır ve etki alanı birliğini engeller.
* Yönetilen Kimlik yeterli izinlere sahip değil.
* Küme adı ilk altı karakterde (başka bir canlı kümeyle veya silinmiş kümeyle) benzersiz değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Azure Active Directory Domain Services ile Kurumsal Güvenlik Paketi yapılandırmaları](./apache-domain-joined-configure-using-azure-adds.md)

* [Azure Active Directory kullanıcılarını bir HDInsight kümesiyle senkronize edin.](../hdinsight-sync-aad-users-to-cluster.md)
