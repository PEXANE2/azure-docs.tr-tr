---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için İş Gününü Yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını İş Günü'ne otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: d7eb01f3997ac4ab2e439c00f07990c51ec3e3d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370357"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için İş Gününü Yapılandır

Bu öğreticinin amacı, çalışma gününden hem Active Directory'ye hem de Azure Etkin Dizini'ne işçi profillerini içe aktarmanız için gerçekleştirmeniz gereken adımları ve e-posta adresinin ve kullanıcı adının İş Günü'ne isteğe bağlı olarak yazılmasıdır.

## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti,](../app-provisioning/user-provisioning.md) kullanıcı hesaplarını sağlamak için [İş Günü İnsan Kaynakları API'siyle](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) tümleşir. Azure AD, aşağıdaki kullanıcı sağlama iş akışlarını etkinleştirmek için bu bağlantıyı kullanır:

* **Kullanıcıları Active Directory'ye sağlama** - İş Günü'nden seçilen kullanıcı kümelerini bir veya daha fazla Active Directory etki alanına sağlama.

* **Yalnızca bulut kullanıcıları Azure Etkin Dizini'ne sağlama** - Şirket içi Active Directory'nin kullanılmadığı senaryolarda, kullanıcılar Azure AD kullanıcı sağlama hizmetini kullanarak doğrudan İş Günü'nden Azure Etkin Dizin'ine kadar sağlanabilir.

* **E-posta adresini ve kullanıcı adını İş Günü'ne geri yazın** - Azure AD kullanıcı sağlama hizmeti, e-posta adreslerini ve kullanıcı adını Azure AD'den Iş Gününe geri yazabilir.

### <a name="what-human-resources-scenarios-does-it-cover"></a>Hangi insan kaynakları senaryolarını kapsıyor?

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen İş Günü kullanıcı sağlama iş akışları, aşağıdaki insan kaynaklarının ve kimlik yaşam döngüsü yönetimi senaryolarının otomasyonuna olanak tanır:

* **Yeni çalışanların işe alınması** - İş Günü'ne yeni bir çalışan eklendiğinde, Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak bir kullanıcı hesabı oluşturulur ve e-posta adresinin İş Günü'ne geri yazılması yla birlikte.

* **Çalışan özniteliği ve profil güncelleştirmeleri** - Bir çalışan kaydı İş Günü'nde (adı, unvanı veya yöneticisi gibi) güncelleştirildiğinde, kullanıcı hesapları Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak güncelleştirilir.

* **Çalışan sonlandırmaları** - Bir çalışanın İş Günü'nde sonlandırılması durumunda, kullanıcı hesabı Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak devre dışı bırakılır.

* **Çalışan yeniden işe alma -** Bir çalışan İş Günü'nde yeniden işe alındığında, eski hesabı otomatik olarak yeniden etkinleştirilebilir veya yeniden kullanılabilir (tercihinize bağlı olarak) Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer [SaaS uygulamalarına.](../app-provisioning/user-provisioning.md)

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu kullanıcı sağlama çözümü kimin için en uygun?

Bu İş Günü kullanıcı sağlama çözümü aşağıdakiler için idealdir:

* İş Günü kullanıcı sağlama için önceden oluşturulmuş, bulut tabanlı bir çözüm isteyen kuruluşlar

* İş Günü'nden Etkin Dizin'e veya Azure Etkin Dizini'ne doğrudan kullanıcı sağlama gerektiren kuruluşlar

* İş Günü HCM modülünden elde edilen veriler kullanılarak kullanıcıların sağlanmasını gerektiren kuruluşlar (bkz. [Get_Workers)](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html)

* Yalnızca İş Günü HCM modülünde algılanan değişiklik bilgilerine dayalı olarak bir veya daha fazla Etkin Dizin Ormanları, Etki Alanları ve OS'lere eşitlenmeleri için katılmayı, taşımayı ve kullanıcıların ayrılmasını gerektiren kuruluşlar (bkz. [Get_Workers)](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html)

* E-posta için Office 365 kullanan kuruluşlar

## <a name="solution-architecture"></a>Çözüm Mimarisi

Bu bölümde, ortak karma ortamlar için uçtan uca kullanıcı sağlama çözüm mimarisi açıklanmaktadır. İlgili iki akış vardır:

* **Yetkili İk Veri Akışı – İş Gününden şirket içi Aktif Dizine:** Bu akışta çalışan olaylar (Yeni İşe alımlar, aktarımlar, sonlandırmalar gibi) önce bulut İş Günü İk kiracısında oluşur ve ardından olay verileri Azure AD ve Geçici Aracı aracılığıyla şirket içi Active Directory'ye akar. Olaya bağlı olarak, AD'de oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerine yol açabilir.
* **E-posta ve Kullanıcı Adı Writeback Flow – şirket içi Aktif Dizinden İş Gününe:** Hesap oluşturma Etkin Dizin'de tamamlandıktan sonra Azure AD Connect aracılığıyla Azure AD ile senkronize edilir ve e-posta ve kullanıcı adı özniteliği İş Günü'ne geri yazılabilir.

![Genel Bakış](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Uçuça kullanıcı veri akışı

1. İk ekibi, Iş Günü HCM'de işçi işlemlerini (Joiners/Movers/Leavers veya New Hires/Transfers/Terminations) gerçekleştirir
2. Azure AD Sağlama Hizmeti, İş Günü İk'daki kimliklerin zamanlanmış eşitlemelerini çalıştırıyor ve şirket içi Active Directory ile eşitlenmesi için işlenmesi gereken değişiklikleri tanımlar.
3. Azure AD Sağlama Hizmeti, şirket içi Azure AD Bağlantı Sağlama Aracısını AD hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerini içeren bir istek yüküyle çağırır.
4. Azure AD Bağlantı Sağlama Aracısı, AD hesabı verilerini eklemek/güncellemek için bir hizmet hesabı kullanır.
5. Azure AD Connect / AD Sync motoru, AD'deki güncelleştirmeleri çekmek için delta sync çalıştırın.
6. Etkin Dizin güncelleştirmeleri Azure Etkin Dizini ile senkronize edilir.
7. Workday Writeback bağlayıcısı yapılandırılırsa, kullanılan eşleşen özniteliğe bağlı olarak e-posta özniteliğini ve kullanıcı adını Workday'e geri yazar.

## <a name="planning-your-deployment"></a>Dağıtımınızı planlama

İş Günü tümleştirmenize başlamadan önce, aşağıdaki ön koşulları kontrol edin ve geçerli Active Directory mimariniz ve kullanıcı sağlama gereksinimleriniz Azure Active Directory tarafından sağlanan çözüm(ler) ile nasıl eşleşip eşleştirilemeye ilişkin aşağıdaki kılavuzu okuyun. Çalışma sayfalarını planlamayı içeren kapsamlı bir [dağıtım planı,](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans) İş Günü tümleştirme iş ortağınız ve İk paydaşlarınızla işbirliği nizde size yardımcı olmak için de kullanılabilir.

Bu bölümde planlamanın aşağıdaki yönleri yer almaktadır:

* [Ön koşullar](#prerequisites)
* [Dağıtmak için sağlama bağlayıcı sıyrık uygulamalarını seçme](#selecting-provisioning-connector-apps-to-deploy)
* [Azure AD Bağlantı Sağlama Aracısı'nın dağıtımını planlama](#planning-deployment-of-azure-ad-connect-provisioning-agent)
* [Birden çok Active Directory etki alanıyla tümleştirme](#integrating-with-multiple-active-directory-domains)
* [Active Directory Kullanıcı Öznitelik Eşleme ve Dönüşümler Için İş Günü Planlama](#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)

### <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* İş Günü'nden kaynaklanacak ve şirket içi Active Directory veya Azure Active Directory'de sağlanacak her kullanıcı için geçerli bir Azure AD Premium P1 veya daha yüksek abonelik lisansı.
* Sağlama aracısını yapılandırmak için Azure AD global yönetici erişimi
* Test ve tümleştirme amacıyla Bir İş Günü uygulama kiracı
* Sistem tümleştirme kullanıcısı oluşturmak ve test amacıyla çalışan verilerini sınamak için değişiklik yapmak için İş Günü'nde yönetici izinleri
* Active Directory'ye kullanıcı sağlama için, .NET 4.7.1+ çalışma süresine sahip Windows Server 2012 veya daha büyük bir sunucunun [şirket içi sağlama aracısını](https://go.microsoft.com/fwlink/?linkid=847801) barındırması gerekir
* Kullanıcıları Active Directory ve Azure AD arasında senkronize etmek için [Azure AD Connect](../hybrid/whatis-hybrid-identity.md)

### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Dağıtmak için sağlama bağlayıcı sıyrık uygulamalarını seçme

Azure AD, İş Günü ve Etkin Dizin arasında iş akışı sağlamayı kolaylaştırmak için Azure AD uygulama galerisinden ekleyebileceğiniz birden çok kullanılabilir bağlayıcı uygulama sağlar:

![Azure AD Uygulama Galerisi](./media/workday-inbound-tutorial/wd_gallery.png)

* **Active Directory User Provisioning için Iş günü** - Bu uygulama, Kullanıcı hesabının İş Günü'nden tek bir Active Directory etki alanına sağlanmasını kolaylaştırır. Birden çok etki alanınız varsa, sağlamanız gereken her Active Directory etki alanı için Azure AD uygulama galerisinden bu uygulamanın bir örneğini ekleyebilirsiniz.

* **Azure AD Kullanıcı Sağlama** Için İş Günü - Azure AD Connect, Active Directory kullanıcılarını Azure Etkin Dizini ile senkronize etmek için kullanılması gereken bir araç olsa da, bu uygulama Yalnızca Bulut Kullanıcılarının İş Günü'nden tek bir Azure Etkin Dizin kiracısına sağlanmasını kolaylaştırmak için kullanılabilir.

* **İş Günü Writeback** - Bu uygulama, Kullanıcının e-posta adreslerinin Azure Active Directory'den İş Günü'ne geri yazılmasını kolaylaştırır.

> [!TIP]
> Normal "İş Günü" uygulaması, İş Günü ve Azure Etkin Dizini arasında tek oturum açma ayarlamak için kullanılır.

Hangi İş Günü sağlama uygulamalarının senaryonuzla alakalı olduğunu belirlemek için aşağıdaki karar akış şemasını kullanın.
    ![Karar Akış Şeması](./media/workday-inbound-tutorial/wday_app_flowchart.png "Karar Akış Şeması")

Bu öğreticinin ilgili bölümüne gitmek için içindekiler tablosunu kullanın.

### <a name="planning-deployment-of-azure-ad-connect-provisioning-agent"></a>Azure AD Bağlantı Sağlama Aracısı'nın dağıtımını planlama

> [!NOTE]
> Bu bölüm yalnızca İş Gününü Active Directory Kullanıcı Sağlama Uygulamasına dağıtmayı planlıyorsanız geçerlidir. İş Günü Yazma Veya Çalışma Günü'nü Azure AD Kullanıcı Sağlama Uygulamasına dağıtıyorsanız bunu atlayabilirsiniz.

AD Kullanıcı Sağlama çözümü için İş Günü, windows 2012 R2 veya daha fazla çalıştıran sunucularda en az 4 GB RAM ve .NET 4.7.1+ çalışma süresiyle bir veya daha fazla Sağlama Aracısı dağıtmayı gerektirir. Provizyon Aracısını yüklemeden önce aşağıdaki hususlar göz önünde bulundurulmalıdır:

* Sağlama Aracısını çalıştıran ana bilgisayar sunucusunun hedef AD etki alanına ağ erişimine sahip olduğundan emin olun
* Sağlama Aracısı Yapılandırma Sihirbazı aracıyı Azure AD kiracınıza kaydeder ve kayıt işlemi TLS bağlantı noktası 443 üzerinden *.msappproxy.net erişim gerektirir. Bu iletişimi sağlayan giden güvenlik duvarı kurallarının yerinde olduğundan emin olun. Aracı [giden HTTPS proxy yapılandırmasını](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)destekler.
* Sağlama Aracısı, şirket içi AD etki alanı(lar) ile iletişim kurmak için bir hizmet hesabı kullanır. Aracının yüklenmesinden önce, etki alanı yöneticisi izinleri ve süresi dolmayan bir parola içeren bir hizmet hesabı oluşturmanız önerilir.  
* Sağlama Aracısı yapılandırması sırasında, sağlama isteklerini işlemesi gereken etki alanı denetleyicilerini seçebilirsiniz. Coğrafi olarak dağıtılmış birkaç etki alanı denetleyiciniz varsa, uçtan uca çözümün güvenilirliğini ve performansını artırmak için Provisioning Agent'ı tercih ettiğiniz etki alanı denetleyicisi(ler) ile aynı sitede kurun
* Yüksek kullanılabilirlik için, birden fazla Sağlama Aracısı dağıtabilir ve aynı şirket içi AD etki alanlarını işlemek için kaydedebilirsiniz.

> [!IMPORTANT]
> Üretim ortamlarında Microsoft, yüksek kullanılabilirlik için Azure AD kiracınızla yapılandırılan en az 3 Sağlama Aracınız olduğunu önerir.

### <a name="integrating-with-multiple-active-directory-domains"></a>Birden çok Active Directory etki alanıyla tümleştirme

> [!NOTE]
> Bu bölüm yalnızca İş Gününü Active Directory Kullanıcı Sağlama Uygulamasına dağıtmayı planlıyorsanız geçerlidir. İş Günü Yazma Veya Çalışma Günü'nü Azure AD Kullanıcı Sağlama Uygulamasına dağıtıyorsanız bunu atlayabilirsiniz.

Active Directory topolojinize bağlı olarak, yapılandırmanız için Kullanıcı Sağlama BağlayıcıSı Uygulamalarının sayısına ve Sağlama Aracılarının sayısına karar vermeniz gerekir. Aşağıda, dağıtımınızı planlarken başvurabileceğiniz yaygın dağıtım desenlerinden bazıları listelenmiştir.

#### <a name="deployment-scenario-1--single-workday-tenant---single-ad-domain"></a>Dağıtım Senaryosu #1 : Tek İş Günü Kiracı -> Tek AD etki alanı

Bu senaryoda, bir İş günü kiracınız vardır ve kullanıcıları tek bir hedef AD etki alanına sağlamak istersiniz. Aşağıda, bu dağıtım için önerilen üretim yapılandırması ve önerilmesi önerilir.

|   |   |
| - | - |
| Hayır. şirket içinde dağıtmak için sağlama ajanlarının | 3 (yüksek kullanılabilirlik ve üzerinde başarısız için) |
| Hayır. Azure portalında yapılandırmak için AD Kullanıcı Sağlama Uygulamaları'na Çalışma Günü | 1 |

  ![Senaryo 1](./media/workday-inbound-tutorial/dep_scenario1.png)

#### <a name="deployment-scenario-2--single-workday-tenant---multiple-child-ad-domains"></a>Dağıtım Senaryosu #2 : Tek İş Günü Kiracı -> Birden Çok alt AD etki alanı

Bu senaryo, kullanıcıları Çalışma Günü'nden bir ormandaki birden çok hedef AD alt etki alanına sağlamayı içerir. Aşağıda, bu dağıtım için önerilen üretim yapılandırması ve önerilmesi önerilir.

|   |   |
| - | - |
| Hayır. şirket içinde dağıtmak için sağlama ajanlarının | 3 (yüksek kullanılabilirlik ve üzerinde başarısız için) |
| Hayır. Azure portalında yapılandırmak için AD Kullanıcı Sağlama Uygulamaları'na Çalışma Günü | alt etki alanı başına bir uygulama |

  ![Senaryo 2](./media/workday-inbound-tutorial/dep_scenario2.png)

#### <a name="deployment-scenario-3--single-workday-tenant---disjoint-ad-forests"></a>Dağıtım Senaryosu #3 : Tek İş Günü Kiracı -> Ayrık AD ormanları

Bu senaryo, kullanıcıları İş Günü'nden ayrı ad ormanlarında etki alanına sağlamayı içerir. Aşağıda, bu dağıtım için önerilen üretim yapılandırması ve önerilmesi önerilir.

|   |   |
| - | - |
| Hayır. şirket içinde dağıtmak için sağlama ajanlarının | 3 ayrı AD orman başına |
| Hayır. Azure portalında yapılandırmak için AD Kullanıcı Sağlama Uygulamaları'na Çalışma Günü | alt etki alanı başına bir uygulama |

  ![3. Senaryo](./media/workday-inbound-tutorial/dep_scenario3.png)

### <a name="planning-workday-to-active-directory-user-attribute-mapping-and-transformations"></a>Active Directory Kullanıcı Öznitelik Eşleme ve Dönüşümler Için İş Günü Planlama

> [!NOTE]
> Bu bölüm yalnızca İş Gününü Active Directory Kullanıcı Sağlama Uygulamasına dağıtmayı planlıyorsanız geçerlidir. İş Günü Yazma Veya Çalışma Günü'nü Azure AD Kullanıcı Sağlama Uygulamasına dağıtıyorsanız bunu atlayabilirsiniz.

Kullanıcı sağlamayı Etkin Dizin etki alanına yapılandırmadan önce aşağıdaki soruları göz önünde bulundurun. Bu soruların yanıtları kapsam filtrelerinizin ve öznitelik eşlemelerinizin nasıl ayarlanması gerektiğini belirler.

* **İş Günü'ndeki kullanıcıların bu Active Directory ormanına sağlanması gerekir?**

  * *Örnek: İş Günü "Şirket" özniteliğinin "Contoso" değerini, "Worker_Type" özniteliğinin ise "Düzenli" olduğu kullanıcılar*

* **Kullanıcılar farklı organizasyon birimlerine (OSB) nasıl yönlendirilir?**

  * *Örnek: Kullanıcılar, İş Günü "Belediye" ve "Country_Region_Reference" özniteliklerinde tanımlandığı şekilde, bir ofis konumuna karşılık gelen OS B'lere yönlendirilir*

* **Aşağıdaki öznitelikler Active Directory'de nasıl doldurulmalıdır?**

  * Ortak Ad (cn)
    * *Örnek: İnsan kaynakları tarafından belirlenen İş Günü User_ID değerini kullanın*

  * Çalışan Kimliği (employeeId)
    * *Örnek: İş Günü Worker_ID değerini kullanın*

  * SAM Hesap Adı (sAMAccountName)
    * *Örnek: Geçersiz karakterleri kaldırmak için Azure AD sağlama ifadesi ne kadar filtre uygulanarak İş Günü User_ID değerini kullanma*

  * Kullanıcı Adı (userPrincipalName)
    * *Örnek: Bir etki alanı adını eklemek için Azure AD sağlama ifadesiyle İş Günü User_ID değerini kullanın*

* **Kullanıcılar İş Günü ve Etkin Dizin arasında nasıl eşlenmelidir?**

  * *Örnek: Belirli bir İş Günü "Worker_ID" değerine sahip kullanıcılar, "employeeID"in aynı değere sahip olduğu Active Directory kullanıcılarıyla eşleşir. Worker_ID değeri Active Directory'de bulunmazsa, yeni bir kullanıcı oluşturun.*
  
* **Etkin Dizin ormanı, eşleşen mantığın çalışması için gereken kullanıcı iliklerini zaten içeriyor mu?**

  * *Örnek: Bu kurulum yeni bir İş Günü dağıtımıysa, Eşleşen mantığı mümkün olduğunca basit tutmak için Active Directory'nin doğru İş Günü Worker_ID değerleriyle (veya seçtiğiniz benzersiz kimlik değeriyle) önceden doldurulması önerilir.*

Bu özel sağlama bağlayıcı uygulamalarının nasıl kurulup yapılandırılabildiğini bu öğreticinin kalan bölümlerinin konusudur. Yapılandırmayı seçtiğiniz uygulamalar, hangi sistemlere sağlamanız gerektiğine ve ortamınızda kaç Etkin Dizin Etki Alanı ve Azure AD kiracısı olduğuna bağlıdır.

## <a name="configure-integration-system-user-in-workday"></a>İş Günü'nde entegrasyon sistemi kullanıcılarını yapılandırma

Tüm İş Günü sağlama bağlayıcılarının ortak bir gereksinimi, İş Günü İnsan Kaynakları API'sine bağlanmak için bir İş Günü tümleştirme sistemi kullanıcısının kimlik bilgilerini gerektirmeleridir. Bu bölümde, İş Günü'nde bir tümleştirme sistemi kullanıcısının nasıl oluşturulacak ları açıklanır ve aşağıdaki bölümler vardır:

* [Entegrasyon sistemi kullanıcısı oluşturma](#creating-an-integration-system-user)
* [Tümleştirme güvenlik grubu oluşturma](#creating-an-integration-security-group)
* [Etki alanı güvenlik ilkesi izinlerini yapılandırma](#configuring-domain-security-policy-permissions)
* [İş süreci güvenlik ilkesi izinlerini yapılandırma](#configuring-business-process-security-policy-permissions)
* [Güvenlik ilkesi değişikliklerini etkinleştirme](#activating-security-policy-changes)

> [!NOTE]
> Bu yordamı atlamak ve bunun yerine sistem tümleştirme hesabı olarak bir İş Günü global yönetici hesabı kullanmak mümkündür. Bu demolar için iyi çalışabilir, ancak üretim dağıtımları için önerilmez.

### <a name="creating-an-integration-system-user"></a>Entegrasyon sistemi kullanıcısı oluşturma

**Bir entegrasyon sistemi kullanıcısı oluşturmak için:**

1. Yönetici hesabını kullanarak İş Günü kiracınızda oturum açın. İş **Günü Uygulamasında,** arama kutusuna kullanıcı oluştur'u girin ve ardından **Tümleştirme Sistemi Kullanıcısını Oluştur'u**tıklatın.

   ![Kullanıcı oluştur](./media/workday-inbound-tutorial/wd_isu_01.png "Kullanıcı oluştur")
2. Yeni Bir Entegrasyon Sistemi Kullanıcısı için bir kullanıcı adı ve parola sağlayarak **Entegrasyon Sistemi Kullanıcı Oluştur** görevini tamamlayın.  
  
   * Bu kullanıcı programlı olarak oturum açacağı **için, Sonraki Oturum Açma** seçeneğinde Yeni Parola İste'yi işaretsiz bırakın.
   * Oturum **Zaman Ekme Dakikalarını** varsayılan değeri 0 olan ve kullanıcının oturumlarının zamanından önce zamanlamasını engelleyecek şekilde bırakın.
   * Kullanıcı **İçi Gün Oturumlarına İzin Verme** seçeneğini seçin, çünkü tümleştirme sisteminin parolasına sahip bir kullanıcının İş Günü'ne giriş yapmasını engelleyen ek bir güvenlik katmanı sağlar.

   ![Entegrasyon Sistemi Kullanıcısı Oluştur](./media/workday-inbound-tutorial/wd_isu_02.png "Entegrasyon Sistemi Kullanıcısı Oluştur")

### <a name="creating-an-integration-security-group"></a>Tümleştirme güvenlik grubu oluşturma

Bu adımda, Çalışma Günü'nde sınırlandırılmamış veya kısıtlanmış bir tümleştirme sistemi güvenlik grubu oluşturur ve önceki adımda oluşturulan tümleştirme sistemi kullanıcısını bu gruba atarsınız.

**Bir güvenlik grubu oluşturmak için:**

1. Arama kutusuna güvenlik grubu oluştur'u girin ve ardından **Güvenlik Grubu Oluştur'u**tıklatın.

    ![Güvenlik Grubu Oluşturma](./media/workday-inbound-tutorial/wd_isu_03.png "Güvenlik Grubu Oluşturma")
2. Güvenlik **Grubu Oluştur** görevini tamamlayın. 

   * İş Günü'nde iki tür güvenlik grubu vardır:
     * **Sınırlandırılmamış:** Güvenlik grubunun tüm üyeleri, güvenlik grubu tarafından güvenli olarak güvence altına alınan tüm veri örneklerine erişebilir.
     * **Kısıtlı:** Tüm güvenlik grubu üyeleri, güvenlik grubunun erişebileceği bir alt veri örnekleri kümesine (satırlar) bağlamsal erişime sahiptir.
   * Tümleştirme için uygun güvenlik grubu türünü seçmek için lütfen İş Günü tümleştirme ortağınıza danışın.
   * Grup türünü anladıktan sonra, Kiracı Güvenlik Grubu açılır bırakma **türünden** **Tümleştirme Sistemi Güvenlik Grubu (Sınırlandırılmamış)** veya **Tümleştirme Sistemi Güvenlik Grubu'nu (Kısıtlı)** seçin.

     ![Güvenlik Grubu Oluşturma](./media/workday-inbound-tutorial/wd_isu_04.png "Güvenlik Grubu Oluşturma")

3. Güvenlik Grubu oluşturma başarılı olduktan sonra, Güvenlik Grubu'na üye atayabileceğiniz bir sayfa görürsünüz. Bu güvenlik grubuna önceki adımda oluşturulan yeni tümleştirme sistemi kullanıcısını ekleyin. *Kısıtlı* güvenlik grubu kullanıyorsanız, uygun kuruluş kapsamını da seçmeniz gerekir.

    ![Güvenlik Grubunu Edit](./media/workday-inbound-tutorial/wd_isu_05.png "Güvenlik Grubunu Edit")

### <a name="configuring-domain-security-policy-permissions"></a>Etki alanı güvenlik ilkesi izinlerini yapılandırma

Bu adımda, güvenlik grubuna çalışan veriler için "etki alanı güvenliği" ilkesi izinleri verirsiniz.

**Etki alanı güvenlik ilkesi izinlerini yapılandırmak için:**

1. Arama kutusuna **Etki Alanı Güvenlik Yapılandırması'nı** girin ve ardından Etki Alanı Güvenlik Yapılandırma **Raporu**bağlantısına tıklayın.  

    ![Etki Alanı Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_06.png "Etki Alanı Güvenlik İlkeleri")  
2. Etki **Alanı** metin kutusunda, aşağıdaki etki alanlarını arayın ve bunları filtreye tek tek ekleyin.  
   * *Dış Hesap Sağlama*
   * *İşçi Verileri: Kamu Çalışanı Raporları*
   * *Kişi Verileri: İş İletişim Bilgileri*
   * *İşçi Verileri: Tüm Pozisyonlar*
   * *İşçi Verileri: Güncel Personel Bilgileri*
   * *İşçi Verileri: İşçi Profilinde İşletme Unvanı*
   * *İş Günü Hesapları*
   
     ![Etki Alanı Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_07.png "Etki Alanı Güvenlik İlkeleri")  

     ![Etki Alanı Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_08.png "Etki Alanı Güvenlik İlkeleri") 

     **Tamam**'a tıklayın.

3. Görünen raporda, **Dış Hesap Sağlama'nın** yanında görünen elipsleri (...) seçin ve **Güvenlik İlkesi İzinlerini > Edit** Menü seçeneğine tıklayın

    ![Etki Alanı Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_09.png "Etki Alanı Güvenlik İlkeleri")  

4. Etki **Alanı Güvenlik İlkesi İzinlerini Edit** sayfasında, **Tümleştirme İzinleri**bölümüne gidin. Tümleştirme sistemi grubunu **Al** ve **Koy** tümleştirme izinleri olan güvenlik grupları listesine eklemek için "+" işaretini tıklatın.

    ![İzin Leri Edle](./media/workday-inbound-tutorial/wd_isu_10.png "İzin Leri Edle")  

5. Tümleştirme sistemi grubunu **Al** ve **Koy** tümleştirme izinleri olan güvenlik grupları listesine eklemek için "+" işaretini tıklatın.

    ![İzin Leri Edle](./media/workday-inbound-tutorial/wd_isu_11.png "İzin Leri Edle")  

6. Bu kalan güvenlik ilkelerinin her biri için yukarıdaki 3-5 adımları yineleyin:

   | İşlem | Etki Alanı Güvenlik İlkesi |
   | ---------- | ---------- |
   | Alın ve Koyun | İşçi Verileri: Kamu Çalışanı Raporları |
   | Alın ve Koyun | Kişi Verileri: İş İletişim Bilgileri |
   | Al | İşçi Verileri: Tüm Pozisyonlar |
   | Al | İşçi Verileri: Güncel Personel Bilgileri |
   | Al | İşçi Verileri: İşçi Profilinde İşletme Unvanı |
   | Alın ve Koyun | İş Günü Hesapları |

### <a name="configuring-business-process-security-policy-permissions"></a>İş süreci güvenlik ilkesi izinlerini yapılandırma

Bu adımda, güvenlik grubuna çalışan veriler için "iş süreci güvenliği" ilkesi izinleri verirsiniz. Bu adım, Workday Writeback uygulama konektörünü ayarlamak için gereklidir.

**İş süreci güvenlik ilkesi izinlerini yapılandırmak için:**

1. Arama kutusuna **İş Süreci İlkesi'ni** girin ve ardından **İş Süreci Güvenlik İlkesi** görevini edit bağlantısını tıklatın.  

    ![İş Süreci Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_12.png "İş Süreci Güvenlik İlkeleri")  

2. İş **Süreci Türü** metin kutusunda, *Kişi'yi* arayın ve **İletişim Değiştir** iş sürecini seçin ve **Tamam'ı**tıklatın.

    ![İş Süreci Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_13.png "İş Süreci Güvenlik İlkeleri")  

3. İş **Süreci Güvenlik İlkesini Edit** sayfasında, **Kişi Bilgilerini Koru (Web Hizmeti)** bölümüne gidin.

    ![İş Süreci Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_14.png "İş Süreci Güvenlik İlkeleri")  

4. Web hizmetleri isteğini başlatabilecek güvenlik grupları listesine yeni tümleştirme sistemi güvenlik grubunu seçin ve ekleyin. **Bitti'ye**tıklayın. 

    ![İş Süreci Güvenlik İlkeleri](./media/workday-inbound-tutorial/wd_isu_15.png "İş Süreci Güvenlik İlkeleri")  

### <a name="activating-security-policy-changes"></a>Güvenlik ilkesi değişikliklerini etkinleştirme

**Güvenlik ilkesi değişikliklerini etkinleştirmek için:**

1. Arama kutusuna etkinleştir'i girin ve ardından **Bekleyen Güvenlik İlkesi Değişikliklerini Etkinleştir**bağlantısını tıklatın.

    ![Etkinleştir](./media/workday-inbound-tutorial/wd_isu_16.png "Etkinleştir")

1. Denetim amacıyla bir açıklama girerek Bekleyen Güvenlik İlkesi Değişikliklerini Etkinleştir'i başlatın ve ardından **Tamam'ı**tıklatın.
1. Onay kutusunu **işaretle**seçeneğini işaretleyerek bir sonraki ekranda görevi tamamlayın ve **ardından Tamam'ı**tıklatın.

    ![Bekleyen Güvenliği Etkinleştir](./media/workday-inbound-tutorial/wd_isu_18.png "Bekleyen Güvenliği Etkinleştir")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>İş Günü'nden Active Directory'ye kullanıcı sağlama yapılandırma

Bu bölümde, iş gününden tümleştirmeniz kapsamındaki her Etkin Dizin etki alanına kullanıcı hesabı sağlama adımları sağlanır.

* [Sağlama bağlayıcısı uygulamasını ekleyin ve Provisioning Agent'ı indirin](#part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent)
* [Şirket içi Provizyon Aracısı(lar) yükleme ve yapılandırma](#part-2-install-and-configure-on-premises-provisioning-agents)
* [İş Günü ve Etkin Dizini bağlantı yapılandırma](#part-3-in-the-provisioning-app-configure-connectivity-to-workday-and-active-directory)
* [Öznitelik eşlemelerini yapılandırma](#part-4-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent"></a>Bölüm 1: Sağlama bağlayıcısı uygulamasını ekleyin ve Provisioning Agent'ı indirin

**İş Gününü Etkin Dizin sağlama için yapılandırmak için:**

1. <https://portal.azure.com> kısmına gidin.

2. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama Ekle'yi**seçin ve **Tüm** kategoriyi seçin.

5. Active **Directory'ye İş Günü Sağlama'yı**arayın ve bu uygulamayı galeriden ekleyin.

6. Uygulama eklendikten ve uygulama ayrıntıları ekranı gösterildikten **sonra, Sağlama'yı**seçin.

7. Sağlama **Provisioning** **Modunu** **Otomatik**olarak değiştirin.

8. Provisioning Agent'ı indirmek için görüntülenen bilgi başlığına tıklayın. 

   ![Aracıyı İndir](./media/workday-inbound-tutorial/pa-download-agent.png "Agent Ekran İndir")


### <a name="part-2-install-and-configure-on-premises-provisioning-agents"></a>Bölüm 2: Şirket içi Provizyon Aracısı(lar) yükleme ve yapılandırma

Active Directory'ye şirket içinde sağlamak için, Sağlama aracısı .NET 4.7.1+ Framework ve istenilen Active Directory etki alanına (lar) ağ erişimi olan bir sunucuya yüklenmelidir.

> [!TIP]
> [Burada](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)verilen yönergeleri kullanarak sunucunuzdaki .NET çerçevesinin sürümünü kontrol edebilirsiniz.
> Sunucuda .NET 4.7.1 veya daha yüksek yüklü yoksa buradan [here](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows)indirebilirsiniz.  

İndirilen aracı yıyükleyiciyi sunucu ana bilgisayara aktarın ve aracı yapılandırmasını tamamlamak için aşağıda verilen adımları izleyin.

1. Yeni aracıyı yüklemek istediğiniz Windows Server'da oturum açın.

1. Provizyon Aracısı yükleyicisini başlatın, şartları kabul edin ve **Yükle** düğmesine tıklayın.

   ![Ekranı Yükle](./media/workday-inbound-tutorial/pa_install_screen_1.png "Ekranı Yükle")
   
1. Yükleme tamamlandıktan sonra sihirbaz başlatılır ve **Azure REKLAM** Bağlantısını ekranını görürsünüz. Azure REKLAM örneğinize bağlanmak için **Kimlik Doğrulama** düğmesini tıklatın.

   ![Azure AD’yi bağlama](./media/workday-inbound-tutorial/pa_install_screen_2.png "Azure AD’yi bağlama")
   
1. Genel Yönetici Kimlik Bilgilerini kullanarak Azure REKLAM örneğinize kimlik doğrulaması verin.

   ![Yönetici Auth](./media/workday-inbound-tutorial/pa_install_screen_3.png "Yönetici Auth")

   > [!NOTE]
   > Azure AD yönetici kimlik bilgileri yalnızca Azure AD kiracınıza bağlanmak için kullanılır. Aracı, kimlik bilgilerini sunucuda yerel olarak depolamaz.

1. Azure AD ile başarılı kimlik doğrulamadan sonra **Etkin Dizin Bağla** ekranını görürsünüz. Bu adımda, AD etki alanı adınızı girin ve **Dizin Ekle** düğmesini tıklatın.

   ![Dizin Ekle](./media/workday-inbound-tutorial/pa_install_screen_4.png "Dizin Ekle")
  
1. Artık AD Etki Alanına bağlanmak için gereken kimlik bilgilerini girmeniz istenir. Aynı ekranda, aracının sağlama istekleri göndermek için kullanması gereken etki alanı denetleyicilerini belirtmek için **etki alanı denetleyicisini seç önceliğini** kullanabilirsiniz.

   ![Etki Alanı Kimlik Bilgileri](./media/workday-inbound-tutorial/pa_install_screen_5.png)
   
1. Etki alanını yapılandırıldıktan sonra, yükleyici yapılandırılan etki alanlarının listesini görüntüler. Bu ekranda, daha fazla etki alanı eklemek için adım #5 ve #6 tekrarlayabilir veya aracı kaydına geçmek için **İleri'ye** tıklayabilirsiniz.

   ![Yapılandırılmış Etki Alanları](./media/workday-inbound-tutorial/pa_install_screen_6.png "Yapılandırılmış Etki Alanları")

   > [!NOTE]
   > Birden fazla AD etki alanınız varsa (örn. na.contoso.com, emea.contoso.com), lütfen her etki alanını listeye ayrı ayrı ekleyin.
   > Yalnızca ana etki alanını (örn. contoso.com) eklemek yeterli değildir. Her alt etki alanını aracıya kaydetmeniz gerekir.
   
1. Yapılandırma ayrıntılarını gözden geçirin ve aracıyı kaydetmek için **Onayla'yı** tıklatın.
  
   ![Ekranı Onayla](./media/workday-inbound-tutorial/pa_install_screen_7.png "Ekranı Onayla")
   
1. Yapılandırma sihirbazı aracı kaydının ilerlemesini görüntüler.
  
   ![Acente Kaydı](./media/workday-inbound-tutorial/pa_install_screen_8.png "Acente Kaydı")
   
1. Aracı kaydı başarılı olduktan sonra Sihirbazdan çıkmak için **Çıkış'a** tıklayabilirsiniz.
  
   ![Çıkış Ekranı](./media/workday-inbound-tutorial/pa_install_screen_9.png "Çıkış Ekranı")
   
1. Aracının yüklemesini doğrulayın ve "Hizmetler" Snap-In'i açarak çalıştırdığından emin olun ve "Microsoft Azure AD Bağlantı Sağlama Aracısı" adlı Hizmeti arayın
  
   ![Hizmetler](./media/workday-inbound-tutorial/services.png)

### <a name="part-3-in-the-provisioning-app-configure-connectivity-to-workday-and-active-directory"></a>Bölüm 3: Sağlama uygulamasında, İş Günü ve Active Directory bağlantısını yapılandırın
Bu adımda, Azure portalında İş Günü ve Active Directory ile bağlantı kuruyoruz. 

1. Azure portalında, [Bölüm 1'de](#part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent) oluşturulan Active Directory Kullanıcı Sağlama Uygulamasına Iş Günü'ne geri dön

1. Yönetici **Kimlik Bilgileri** bölümünü aşağıdaki gibi tamamlayın:

   * **Yönetici Kullanıcı Adı** – Kiracı alan adı eklenen İş Günü entegrasyon sistemi hesabının kullanıcı adını girin. Bu gibi bir şey görünmelidir: **kullanıcı adı\@tenant_name**

   * **Yönetici şifresi –** İş Günü entegrasyon sistemi hesabının parolasını girin

   * **Kiracı URL ' i –** Kiracınız için İş Günü web hizmetleri bitiş noktasına URL'yi girin. Bu değer şu https://wd3-impl-services1.workday.com/ccx/service/contoso4şekilde görünmelidir: , *contoso4'ün* doğru kiracı adınızla değiştirildiği ve *wd3-impl'in* doğru ortam dizesi ile değiştirildiği yer.

   * **Aktif Dizin Ormanı -** Etkin Dizin etki alanınızın aracıya kayıtlı olarak "Adı". Sağlama için hedef etki alanını seçmek için açılır alanı kullanın. Bu değer genellikle şöyle bir dizedir: *contoso.com*

   * **Aktif Dizin Konteyner -** Aracının varsayılan olarak kullanıcı hesapları oluşturması gereken DN kapsayıcısını girin.
        Örnek: *OU=Standart Kullanıcılar,OU=Kullanıcılar,DC=contoso,DC=test*
        
     > [!NOTE]
     > Bu ayar yalnızca *parentDistinguishedName* özniteliği öznitelik eşlemelerinde yapılandırılmamışsa, kullanıcı hesabı oluşturmaları için devreye girer. Bu ayar, kullanıcı arama veya güncelleştirme işlemleri için kullanılmaz. Tüm etki alanı alt ağacı arama işlemi kapsamında düşer.

   * **Bildirim E-postası –** E-posta adresinizi girin ve "hata olursa e-posta gönder" onay kutusunu işaretleyin.

     > [!NOTE]
     > Azure AD Sağlama Hizmeti, sağlama işi [karantina](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) durumuna girerse e-posta bildirimi gönderir.

   * Test **Bağlantısı** düğmesini tıklatın. Bağlantı testi başarılı olursa, üstteki **Kaydet** düğmesini tıklatın. Başarısız olursa, iş günü kimlik bilgileri ve aracı kurulumunda yapılandırılan AD kimlik bilgilerinin geçerli olup olmadığını iki kez denetleyin.

     ![Azure portalında](./media/workday-inbound-tutorial/wd_1.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **Eşlemeler** bölümü Varsayılan Eşleme **İş Günü Çalışanlarını Yerinde Etkin Dizine Senkronize** Eder

### <a name="part-4-configure-attribute-mappings"></a>Bölüm 4: Öznitelik eşlemelerini yapılandırma

Bu bölümde, kullanıcı verilerinin İş Günü'nden Active Directory'ye akışını nasıl yapılandıracağınız.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **İş Günü Çalışanlarını Şirket Içi Etkin Dizini Eşitle'yi**tıklatın.

1. Kaynak **Nesne Kapsamı** alanında, bir öznitelik tabanlı filtre kümesi tanımlayarak, İş Günü'ndeki hangi kullanıcı kümelerinin AD'ye karşılık vermek için kapsamda olması gerektiğini seçebilirsiniz. Varsayılan kapsam "İş Günü'ndeki tüm kullanıcılar"tır. Örnek filtreler:

   * Örnek: 1000000 ile 2000000 arasında İşçi Numarası olan kullanıcılara kapsam (20000000 hariç)

      * Öznitelik: İşçi Kimliği

      * Operatör: REGEX Maç

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Sadece çalışanlar değil, şartlı çalışanlar

      * Öznitelik: EmployeeID

      * Operatör: NULL Değİl

   > [!TIP]
   > Sağlama uygulamasını ilk kez yapılandırırken, size istenen sonucu verdiğinden emin olmak için öznitelik eşlemelerinizi ve ifadelerinizi test etmeniz ve doğrulamanız gerekir. Microsoft, eşlemelerinizi İş Günü'nden birkaç test kullanıcısıyla test etmek için **Kaynak Nesne Kapsamı** altındaki kapsam filtrelerini kullanmanızı önerir. Eşlemelerin çalıştığını doğruladıktan sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

   > [!CAUTION] 
   > Sağlama altyapısının varsayılan davranışı, kapsam dışına çıkan kullanıcıları devre dışı/silmektir. Bu, Ad tümleştirmesi için Çalışma gününüzde istilen olmayabilir. Bu varsayılan davranışı geçersiz kılmak [için, kapsam dışına çıkan kullanıcı hesaplarının silinmesini atla makalesine](../app-provisioning/skip-out-of-scope-deletions.md) bakın
  
1. Hedef **Nesne Eylemleri** alanında, Etkin Dizini'nde hangi eylemlerin gerçekleştirildiğini genel olarak filtreleyebilirsiniz. **Oluştur** ve **Güncelleştir** en yaygın olanıdır.

1. **Öznitelik eşlemeleri** bölümünde, tek tek İş Günü'nün eşlemeyi Etkin Dizin özniteliklerine nasıl bağldadığını tanımlayabilirsiniz.

1. Güncelleştirmek için varolan bir öznitelik eşleciliğini tıklatın veya yeni eşlemeler eklemek için ekranın alt kısmında **yeni eşleme ekle'yi** tıklatın. Tek tek öznitelik eşleme bu özellikleri destekler:

      * **Haritalama Türü**

         * **Doğrudan** – İş Günü özniteliğinin değerini AD özniteliğine hiçbir değişiklik olmadan yazar

         * **Sabit** - AD özniteliğine statik, sabit bir dize değeri yazın

         * **İfade** – Bir veya daha fazla İş Günü özniteliğine dayalı olarak AD özniteliğine özel bir değer yazmanızı sağlar. [Daha fazla bilgi için, ifadeler bu makaleye bakın.](../app-provisioning/functions-for-customizing-application-data.md)

      * **Kaynak özniteliği** - İş Günü'nden kullanıcı özniteliği. Aradığınız öznitelik yoksa, [bkz.](#customizing-the-list-of-workday-user-attributes)

      * **Varsayılan değer** – İsteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bunun yerine bu değeri yazar.
            En yaygın yapılandırma bu boş bırakmaktır.

      * **Hedef öznitelik** – Active Directory'deki kullanıcı özniteliği.

      * **Bu özniteliği kullanarak nesneleri eşleştirin** – Bu eşlemenin İş Günü ile Etkin Dizin arasındaki kullanıcıları benzersiz olarak tanımlamak için kullanılıp kullanılmaması gerektiği. Bu değer genellikle Çalışma Günü için İşçi Kimliği alanında ayarlanır ve bu değer genellikle Active Directory'deki Çalışan Kimliği özniteliklerinden birine eşlenir.

      * **Eşleşen öncelik** – Birden çok eşleşen öznitelik ayarlanabilir. Birden çok olduğunda, bu alan tarafından tanımlanan sırada değerlendirilir. Eşleşme bulunur bulunmaz, başka eşleşen öznitelikler değerlendirilmez.

      * **Bu eşlemi uygulayın**

         * **Her zaman** – Bu eşlemi hem kullanıcı oluşturma hem de güncelleştirme eylemlerine uygulayın

         * **Yalnızca oluşturma sırasında** - Bu eşlemi yalnızca kullanıcı oluşturma eylemlerine uygulayın

1. Eşlemelerinizi kaydetmek için, Atrit-Eşleme bölümünün üst **kısmındakaydet'i** tıklatın.

   ![Azure portalında](./media/workday-inbound-tutorial/wd_2.png)

#### <a name="below-are-some-example-attribute-mappings-between-workday-and-active-directory-with-some-common-expressions"></a>Aşağıda, Bazı ortak ifadeler ile İş Günü ve Etkin Dizin arasında bazı örnek öznitelik eşlemeleri ve

* Bir veya daha fazla İş Günü kaynak özniteliklerine dayalı olarak bir kullanıcıyı farklı OS'lere sağlamak için *bir kullanıcıyı ana DistinguishedName* özniteliğine eşleyen ifade kullanılır. Bu örnek, kullanıcıları hangi şehirde olduklarına göre farklı İş'lere yerleştiren kullanıcılardır.

* Active Directory'deki *userPrincipalName* özniteliği, hedef AD etki alanında oluşturulan bir değerin varlığını denetleyen ve yalnızca benzersizse ayarlayan [SelectUniqueValue](../app-provisioning/functions-for-customizing-application-data.md#selectuniquevalue) de-duplikasyon işlevi kullanılarak oluşturulur.  

* [Burada ifadeler yazma belgeleri vardır.](../app-provisioning/functions-for-customizing-application-data.md) Bu bölümde, özel karakterlerin nasıl kaldırılılabilene ilişkin örnekler yer almaktadır.

| İş GÜNÜ ÖZNITELIĞI | ACTIVE DIZIN ÖZNITELIĞI |  EŞLEŞEN KIMLIK MI? | OLUŞTUR / GÜNCELLE |
| ---------- | ---------- | ---------- | ---------- |
| **İşçi Kimliği**  |  EmployeeID | **Evet** | Yalnızca oluşturma üzerine yazıldı |
| **Tercih Edilen Ad Verileri**    |  Cn    |   |   Yalnızca oluşturma üzerine yazıldı |
| **SelectUniqueValue(",\@ \[Join(",\]FirstName \[\], "contoso.com", Join("\@"Join(", Mid(FirstName\[\], 1, 1), \[\]LastName ), "contoso.com"),\@Join(" ",\[Join(", Mid(Name\], 1, 2), \[Soyadı\]), "contoso.com")**   | userPrincipalName     |     | Yalnızca oluşturma üzerine yazıldı 
| **Değiştir(Orta(Replace(\[\]UserID ,\[\\\\/\\\\\\\\\\\\\[\\\\\]\\,\\\\ "(\\:\\ \\\|\\\\=\\\\,\\\\+\\\\\*\\\\? \\\\\\)", , "", , 1, 20), " ([.) \\ &lt; \\ \\ &gt; \] \*](file:///\\ \$.) *$)", , "", , )**      |    sAMAccountName            |     |         Yalnızca oluşturma üzerine yazıldı |
| **Switch(\[\]Etkin , , "0", "Doğru", "1", "False")** |  hesapDevre Dışı      |     | Oluşturma + güncelleme |
| **FirstName**   | givenName       |     |    Oluşturma + güncelleme |
| **Soyadı**   |   sn   |     |  Oluşturma + güncelleme |
| **Tercih Edilen Ad Verileri**  |  displayName |     |   Oluşturma + güncelleme |
| **Şirket**         | şirket   |     |  Oluşturma + güncelleme |
| **Denetleyici Organizasyon**  | bölüm  |     |  Oluşturma + güncelleme |
| **Yönetici Referans**   | manager  |     |  Oluşturma + güncelleme |
| **BusinessTitle**   |  başlık     |     |  Oluşturma + güncelleme | 
| **AdresLineData**    |  Streetaddress  |     |   Oluşturma + güncelleme |
| **Belediyesi**   |   l   |     | Oluşturma + güncelleme |
| **ÜlkeReferansİki Mektubu**      |   Co |     |   Oluşturma + güncelleme |
| **ÜlkeReferansİki Mektubu**    |  c  |     |         Oluşturma + güncelleme |
| **CountryRegionReference** |  st     |     | Oluşturma + güncelleme |
| **Çalışma AlanıBaşvurusu** | physicalDeliveryOfficeName    |     |  Oluşturma + güncelleme |
| **Posta Kodu**  |   Postakodu  |     | Oluşturma + güncelleme |
| **Birincil Çalışma Telefonu**  |  Telephonenumber   |     | Oluşturma + güncelleme |
| **Faks**      | faksTelefon Numarası     |     |    Oluşturma + güncelleme |
| **Mobil**  |    mobil       |     |       Oluşturma + güncelleme |
| **Yerel Referans** |  tercihDil  |     |  Oluşturma + güncelleme |                                               
| **Switch\[(\]Belediye , "OU = Standart Kullanıcılar,OU=Kullanıcılar,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standart Kullanıcılar,OU=Kullanıcılar,OU=Austin,Ou=Locations,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", "London", "OU=Standard Users,OU=London,OU=Locations,DC=contoso,DC=com")**  | parentDistinguishedName     |     |  Oluşturma + güncelleme |

Öznitelik eşleme yapılandırmanız tamamlandıktan sonra, [artık kullanıcı sağlama hizmetini etkinleştirebilir ve başlatabilirsiniz.](#enable-and-launch-user-provisioning)

## <a name="configuring-user-provisioning-to-azure-ad"></a>Kullanıcı sağlamayı Azure AD olarak yapılandırma

Aşağıdaki bölümlerde, yalnızca buluta yönelik dağıtımlar için İş Günü'nden Azure AD'ye kullanıcı sağlama yapılandırma adımları açıklanmaktadır.

* [Azure AD sağlama bağlayıcısı uygulamasını ekleme ve İş Günü bağlantısı oluşturma](#part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday)
* [İş Günü ve Azure AD öznitelik eşlemelerini yapılandırma](#part-2-configure-workday-and-azure-ad-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

> [!IMPORTANT]
> Yalnızca şirket içi Active Directory'ye değil, Azure AD'ye verilmesi gereken yalnızca bulut kullanıcılarınız varsa aşağıdaki yordamı uygulayın.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Bölüm 1: Azure AD sağlama bağlayıcısı uygulamasını ekleme ve İş Günü bağlantısı oluşturma

**İş Günü'nü yalnızca bulut kullanıcıları için Azure Etkin Dizin sağlama için yapılandırmak için:**

1. <https://portal.azure.com> kısmına gidin.

2. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama ekle'yi**seçin ve ardından **Tüm** ler kategorisini seçin.

5. İş **Günü'nden Azure AD sağlamasına**arama yapın ve bu uygulamayı galeriden ekleyin.

6. Uygulama eklendikten ve uygulama ayrıntıları ekranı gösterildikten **sonra, Sağlama'yı**seçin.

7. Sağlama **Provisioning** **Modunu** **Otomatik**olarak değiştirin.

8. Yönetici **Kimlik Bilgileri** bölümünü aşağıdaki gibi tamamlayın:

   * **Yönetici Kullanıcı Adı** – Kiracı alan adı eklenen İş Günü entegrasyon sistemi hesabının kullanıcı adını girin. Şöyle bir şey görünmelidir:username@contoso4

   * **Yönetici şifresi –** İş Günü entegrasyon sistemi hesabının parolasını girin

   * **Kiracı URL ' i –** Kiracınız için İş Günü web hizmetleri bitiş noktasına URL'yi girin. Bu değer şu https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesşekilde görünmelidir: , *contoso4'ün* doğru kiracı adınızla değiştirildiği ve *wd3-impl'in* doğru ortam dizesi ile değiştirildiği yer. Bu URL bilinmiyorsa, kullanılacak doğru URL'yi belirlemek için lütfen Iş Günü tümleştirme ortağınızla veya destek temsilcinizle birlikte çalışın.

   * **Bildirim E-postası –** E-posta adresinizi girin ve "hata olursa e-posta gönder" onay kutusunu işaretleyin.

   * Test **Bağlantısı** düğmesini tıklatın.

   * Bağlantı testi başarılı olursa, üstteki **Kaydet** düğmesini tıklatın. Başarısız olursa, İş Günü URL'sinin ve kimlik bilgilerinin İş Günü'nde geçerli olup olmadığını iki kez denetleyin.

### <a name="part-2-configure-workday-and-azure-ad-attribute-mappings"></a>Bölüm 2: İş Günü ve Azure AD öznitelik eşlemelerini yapılandırma

Bu bölümde, yalnızca bulut kullanıcıları için kullanıcı verilerinin İş Günü'nden Azure Etkin Dizini'ne nasıl aktığını yapılandırırsınız.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **Çalışanları Azure AD'a Eşitle'yi**tıklatın.

2. Kaynak **Nesne Kapsamı** alanında, bir öznitelik tabanlı filtre kümesi tanımlayarak, İş Günü'ndeki hangi kullanıcı kümelerinin Azure AD'ye karşılık vermek için kapsamda olması gerektiğini seçebilirsiniz. Varsayılan kapsam "İş Günü'ndeki tüm kullanıcılar"tır. Örnek filtreler:

   * Örnek: 1000000 ile 2000000 arasında İşçi Numarası olan kullanıcılara kapsam

      * Öznitelik: İşçi Kimliği

      * Operatör: REGEX Maç

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Sadece şartlı işçiler değil, düzenli çalışanlar

      * Öznitelik: ContingentID

      * Operatör: NULL Değİl

3. Hedef **Nesne Eylemleri** alanında, Azure AD'de hangi eylemlerin gerçekleştirildiğini genel olarak filtreleyebilirsiniz. **Oluştur** ve **Güncelleştir** en yaygın olanıdır.

4. **Öznitelik eşlemeleri** bölümünde, tek tek İş Günü'nün eşlemeyi Etkin Dizin özniteliklerine nasıl bağldadığını tanımlayabilirsiniz.

5. Güncelleştirmek için varolan bir öznitelik eşleciliğini tıklatın veya yeni eşlemeler eklemek için ekranın alt kısmında **yeni eşleme ekle'yi** tıklatın. Tek tek öznitelik eşleme bu özellikleri destekler:

   * **Haritalama Türü**

      * **Doğrudan** – İş Günü özniteliğinin değerini AD özniteliğine hiçbir değişiklik olmadan yazar

      * **Sabit** - AD özniteliğine statik, sabit bir dize değeri yazın

      * **İfade** – Bir veya daha fazla İş Günü özniteliğine dayalı olarak AD özniteliğine özel bir değer yazmanızı sağlar. [Daha fazla bilgi için, ifadeler bu makaleye bakın.](../app-provisioning/functions-for-customizing-application-data.md)

   * **Kaynak özniteliği** - İş Günü'nden kullanıcı özniteliği. Aradığınız öznitelik yoksa, [bkz.](#customizing-the-list-of-workday-user-attributes)

   * **Varsayılan değer** – İsteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bunun yerine bu değeri yazar.
            En yaygın yapılandırma bu boş bırakmaktır.

   * **Hedef öznitelik** – Azure AD'deki kullanıcı özniteliği.

   * **Bu özniteliği kullanarak nesneleri eşleştirin** – Bu öznitelik, İş Günü ve Azure AD arasındaki kullanıcıları benzersiz olarak tanımlamak için kullanılıp kullanılmaması gerektiğini. Bu değer genellikle, genellikle Çalışan Kimliği özniteliğine (yeni) veya Azure AD'deki bir uzantı özniteliğine eşlenen İş Günü için İşçi Kimliği alanında ayarlanır.

   * **Eşleşen öncelik** – Birden çok eşleşen öznitelik ayarlanabilir. Birden çok olduğunda, bu alan tarafından tanımlanan sırada değerlendirilir. Eşleşme bulunur bulunmaz, başka eşleşen öznitelikler değerlendirilmez.

   * **Bu eşlemi uygulayın**

     * **Her zaman** – Bu eşlemi hem kullanıcı oluşturma hem de güncelleştirme eylemlerine uygulayın

     * **Yalnızca oluşturma sırasında** - Bu eşlemi yalnızca kullanıcı oluşturma eylemlerine uygulayın

6. Eşlemelerinizi kaydetmek için, Atrit-Eşleme bölümünün üst **kısmındakaydet'i** tıklatın.

Öznitelik eşleme yapılandırmanız tamamlandıktan sonra, [artık kullanıcı sağlama hizmetini etkinleştirebilir ve başlatabilirsiniz.](#enable-and-launch-user-provisioning)

## <a name="configuring-azure-ad-attribute-writeback-to-workday"></a>Azure AD özniteliği ni İş Gününe yeniden yapılandırma

Azure Active Directory'den Workday'e kullanıcı e-posta adreslerinin ve kullanıcı adının geri yazısını yapılandırmak için bu yönergeleri izleyin.

* [Writeback bağlayıcısı uygulamasını ekleme ve İş Günü'ne bağlantı oluşturma](#part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday)
* [Writeback öznitelik eşlemelerini yapılandırma](#part-2-configure-writeback-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-adding-the-writeback-connector-app-and-creating-the-connection-to-workday"></a>Bölüm 1: Writeback bağlayıcısı uygulamasını ekleme ve İş Günü'ne bağlantı oluşturma

**Workday Writeback konektörünü yapılandırmak için:**

1. <https://portal.azure.com> kısmına gidin.

2. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama ekle'yi**seçin ve ardından **Tüm** ler kategorisini seçin.

5. **Workday Writeback'i**arayın ve bu uygulamayı galeriden ekleyin.

6. Uygulama eklendikten ve uygulama ayrıntıları ekranı gösterildikten **sonra, Sağlama'yı**seçin.

7. Sağlama **Provisioning** **Modunu** **Otomatik**olarak değiştirin.

8. Yönetici **Kimlik Bilgileri** bölümünü aşağıdaki gibi tamamlayın:

   * **Yönetici Kullanıcı Adı** – Kiracı alan adı eklenen İş Günü entegrasyon sistemi hesabının kullanıcı adını girin. Gibi bir şey görünmelidir: *kullanıcı adı\@contoso4*

   * **Yönetici şifresi –** İş Günü entegrasyon sistemi hesabının parolasını girin

   * **Kiracı URL ' i –** Kiracınız için İş Günü web hizmetleri bitiş noktasına URL'yi girin. Bu değer şu https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resourcesşekilde görünmelidir: *contoso4'ün* doğru kiracı adınızla değiştirildiği ve *wd3-impl'in* doğru ortam dizesi (gerekirse) ile değiştirildiği yer.

   * **Bildirim E-postası –** E-posta adresinizi girin ve "hata olursa e-posta gönder" onay kutusunu işaretleyin.

   * Test **Bağlantısı** düğmesini tıklatın. Bağlantı testi başarılı olursa, üstteki **Kaydet** düğmesini tıklatın. Başarısız olursa, İş Günü URL'sinin ve kimlik bilgilerinin İş Günü'nde geçerli olup olmadığını iki kez denetleyin.

### <a name="part-2-configure-writeback-attribute-mappings"></a>Bölüm 2: Writeback öznitelik eşlemelerini yapılandırma

Bu bölümde, geri yazma özniteliklerinin Azure AD'den İş Günü'ne akışını nasıl yapılandıracağınız. Şu anda, bağlayıcı yalnızca E-posta adresi nin ve kullanıcı adının Workday'e yazılmasını destekler.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **Azure Etkin Dizin Kullanıcılarını İş Günü'ne Senkronize Et'i**tıklatın.

2. Kaynak **Nesne Kapsamı** alanında isteğe bağlı olarak, Azure Etkin Dizini'ndeki hangi kullanıcı kümelerinin e-posta adreslerinin İş Günü'ne geri yazılması gerektiğini filtreleyebilirsiniz. Varsayılan kapsam "Azure AD'deki tüm kullanıcılar"tır.

3. **Atnitelik eşlemeleri** bölümünde, Çalışma Günü çalışan kimliğinin veya çalışan kimliğinin depolandığı Azure Etkin Dizini'ndeki özniteliği belirtmek için eşleşen kimliği güncelleştirin. Popüler bir eşleştirme yöntemi, İş Günü çalışan kimliğini veya çalışan kimliğini Azure AD'deki Attribute1-15 uzantısıyla eşitlemek ve ardından bu özniteliği İş Günü'ndeki kullanıcıları eşleştirmek için Azure AD'de kullanmaktır.

4. Genellikle Azure AD *userPrincipalName* özniteliğini İş Günü *UserID* özniteliğine göre eşlersiniz ve Azure AD posta özniteliğini İş Günü *E-posta Adresi* özniteliğiyle eşlersiniz. *mail* Eşlemelerinizi kaydetmek için, Atrit-Eşleme bölümünün üst **kısmındakaydet'i** tıklatın.

Öznitelik eşleme yapılandırmanız tamamlandıktan sonra, [artık kullanıcı sağlama hizmetini etkinleştirebilir ve başlatabilirsiniz.](#enable-and-launch-user-provisioning)

## <a name="enable-and-launch-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirme ve başlatma

İş Günü sağlama uygulaması yapılandırmaları tamamlandıktan sonra, Azure portalındaki sağlama hizmetini açabilirsiniz.

> [!TIP]
> Varsayılan olarak, sağlama hizmetini açtığınızda, kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatılır. Eşleme veya İş Günü veri sorunlarında hatalar varsa, sağlama işi başarısız olabilir ve karantina durumuna geçebilirsiniz. Bunu önlemek için, en iyi yöntem olarak, **Kaynak Nesne Kapsamı** filtresini yapılandırmanızı ve tüm kullanıcılar için tam eşitleme başlatmadan önce öznitelik eşlemelerinizi birkaç test kullanıcısıyla test etmenizi öneririz. Eşlemelerin işe yarayıp yaradığını doğruladıktan ve size istenen sonuçları verdikten sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

1. **Sağlama** sekmesinde, Sağlama **Durumunu** **A.C.** olarak ayarlayın.

2. **Kaydet**'e tıklayın.

3. Bu işlem, İş Günü kiracısında kaç kullanıcı olduğuna bağlı olarak değişken saat sayısı alabilecek ilk eşitleme başlatılır. 

4. İstediğinzaman, sağlama hizmetinin hangi eylemleri gerçekleştirdiğini görmek için Azure portalındaki **Denetim günlükleri** sekmesini kontrol edin. Denetim günlükleri, hangi kullanıcıların İş Günü dışında okunduğu ve daha sonra Etkin Dizin'e eklendiği veya güncelleştirildiği gibi, sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olaylarını listeler. Denetim günlüklerinin nasıl gözden geçirileceği ve sağlama hatalarını nasıl düzelteceğimizle ilgili yönergeler için Sorun Giderme bölümüne bakın.

5. İlk eşitleme tamamlandıktan sonra, aşağıda gösterildiği **gibi, Sağlama** sekmesine bir denetim özet raporu yazar.

   ![Azure portalında](./media/workday-inbound-tutorial/wd_3.png)

## <a name="frequently-asked-questions-faq"></a>Sık Sorulan Sorular (SSS)

* **Çözüm yeteneği soruları**
  * [Workday'den yeni bir işe alma işlemi yapılırken, çözüm Active Directory'deki yeni kullanıcı hesabının parolasını nasıl ayarlar?](#when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory)
  * [Çözüm, sağlama işlemleri tamamlandıktan sonra e-posta bildirimleri göndermeyi destekliyor mu?](#does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete)
  * [Yeni işe alımlar için parola teslimini nasıl yönetirim ve parolalarını sıfırlamak için güvenli bir şekilde bir mekanizma sağlarım?](#how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password)
  * [Çözüm, Azure AD bulutundaki veya sağlama aracısı katmanındaki İş Günü kullanıcı profillerini önbelleğe alıyor mu?](#does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer)
  * [Çözüm, şirket içi REKLAM gruplarının kullanıcıya atanmasını destekliyor mu?](#does-the-solution-support-assigning-on-premises-ad-groups-to-the-user)
  * [Çözüm, İş Günü çalışanı profillerini sorgulamak ve güncelleştirmek için hangi İş Günü API'lerini kullanır?](#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles)
  * [İş günü HCM kiracımı iki Azure AD kiracısıyla yapılandırabilir miyim?](#can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants)
  * [Azure AD Connect'i dağıttıysak neden "Azure AD'ye Çalışma Günü" kullanıcı sağlama uygulaması desteklenmez?](#why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect)
  * [İş Günü ve Azure REKLAM tümleştirmesi ile ilgili iyileştirmeleri nasıl önerirveya yeni özellikler isteyebilirim?](#how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration)

* **Agent sorularını sağlama**
  * [Provizyon Aracısı'nın GA sürümü nedir?](#what-is-the-ga-version-of-the-provisioning-agent)
  * [Provizyon Ajanımın sürümünü nasıl bilirim?](#how-do-i-know-the-version-of-my-provisioning-agent)
  * [Microsoft, Provisioning Agent güncelleştirmelerini otomatik olarak itiyor mu?](#does-microsoft-automatically-push-provisioning-agent-updates)
  * [Azure AD Connect çalıştıran aynı sunucuya Sağlama Aracısı yükleyebilir miyim?](#can-i-install-the-provisioning-agent-on-the-same-server-running-azure-ad-connect)
  * [Giden HTTP iletişimi için bir proxy sunucusu kullanmak üzere Geçici Aracıyı nasıl yapılandırıyorum?](#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)
  * [Sağlama Aracısının Azure AD kiracısıyla iletişim kurabilmesinden ve aracının gerektirdiği bağlantı noktalarını engelleyen güvenlik duvarları olmadığından nasıl emin olabilirim?](#how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent)
  * [Geçici Aracımla ilişkili etki alanının kaydını nasıl açabilirim?](#how-do-i-de-register-the-domain-associated-with-my-provisioning-agent)
  * [Geçici Aracıyı nasıl kaldıracağım?](#how-do-i-uninstall-the-provisioning-agent)
  
* **AD öznitelik eşleme ve yapılandırma soruları için iş günü**
  * [İş Günü Sağlama Öznitelik Haritalama ve Şema'mın çalışan bir kopyasını nasıl yedeklerveya dışa aktarım?](#how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema)
  * [İş Günü ve Active Directory'de özel özelliklerim var. Çözümü özel özniteliklerimle çalışacak şekilde nasıl yapılandırıyorum?](#i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes)
  * [Kullanıcının fotoğrafını İş Günü'nden Active Directory'ye sağlayabilir miyim?](#can-i-provision-users-photo-from-workday-to-active-directory)
  * [İş Günü'ndeki cep telefonu numaralarını, genel kullanım için kullanıcı onayına dayanarak nasıl senkronize edebilirim?](#how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage)
  * [Ad'de görüntüleme adlarını kullanıcının departman/ülke/şehir özelliklerine göre nasıl biçimlendirebilirim ve bölgesel farkları nasıl işlerim?](#how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances)
  * [SamAccountName özniteliği için benzersiz değerler oluşturmak için SelectUniqueValue'ı nasıl kullanabilirim?](#how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute)
  * [Aksit etekli karakterleri nasıl kaldırıp normal İngilizce alfabeye dönüştürebilirim?](#how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets)

### <a name="solution-capability-questions"></a>Çözüm yeteneği soruları

#### <a name="when-processing-a-new-hire-from-workday-how-does-the-solution-set-the-password-for-the-new-user-account-in-active-directory"></a>Workday'den yeni bir işe alma işlemi yapılırken, çözüm Active Directory'deki yeni kullanıcı hesabının parolasını nasıl ayarlar?

Şirket içi sağlama aracısı yeni bir AD hesabı oluşturmak için bir istek aldığında, AD sunucusu tarafından tanımlanan parola karmaşıklığı gereksinimlerini karşılamak üzere tasarlanmış karmaşık bir rasgele parolayı otomatik olarak oluşturur ve bunu kullanıcı nesnesine ayarlar. Bu parola hiçbir yerde günlüğe kaydedilmez.

#### <a name="does-the-solution-support-sending-email-notifications-after-provisioning-operations-complete"></a>Çözüm, sağlama işlemleri tamamlandıktan sonra e-posta bildirimleri göndermeyi destekliyor mu?

Hayır, sağlama işlemlerini tamamladıktan sonra e-posta bildirimleri göndermek geçerli sürümde desteklenmez.

#### <a name="how-do-i-manage-delivery-of-passwords-for-new-hires-and-securely-provide-a-mechanism-to-reset-their-password"></a>Yeni işe alımlar için parola teslimini nasıl yönetirim ve parolalarını sıfırlamak için güvenli bir şekilde bir mekanizma sağlarım?

Yeni AD hesabı sağlamayla ilgili son adımlardan biri, kullanıcının AD hesabına atanan geçici parolanın teslim edilmesidir. Birçok işletme, geçici parolayı kullanıcı yöneticisine teslim etme geleneksel yaklaşımını kullanmaya devam eder ve parolayı yeni işe alma/şartlı çalışana teslim eder. Bu işlem doğal bir güvenlik kusuruna sahiptir ve Azure AD özelliklerini kullanarak daha iyi bir yaklaşım uygulamak için kullanılabilir bir seçenek vardır.

İşe alma sürecinin bir parçası olarak, İk ekipleri genellikle bir arka plan kontrolü çalıştırmak ve yeni işe cep telefonu numarasını veteriner. AD Kullanıcı Sağlama tümleştirmesi için İş Günü ile, bu gerçeğin üzerine inşa edebilir ve 1. Bu, Yeni işe alma nın "Mobil Numara" özniteliğini İş Günü'nden AD'ye, ardından da Azure AD Connect'i kullanarak AD'den Azure AD'ye yayılarak gerçekleştirilir. Azure AD'de "Mobil Numara" bulunduğunda, kullanıcının hesabı için [Self Servis Parola Sıfırlama'yı (SSPR)](../authentication/howto-sspr-authenticationdata.md) etkinleştirebilirsiniz, böylece 1.

#### <a name="does-the-solution-cache-workday-user-profiles-in-the-azure-ad-cloud-or-at-the-provisioning-agent-layer"></a>Çözüm, Azure AD bulutundaki veya sağlama aracısı katmanındaki İş Günü kullanıcı profillerini önbelleğe alıyor mu?

Hayır, çözüm kullanıcı profillerinin önbelleğini korumaz. Azure AD sağlama hizmeti, yalnızca bir veri işlemcisi, İş Günü'nden verileri okuma ve hedef Active Directory veya Azure AD'ye yazma görevi görür. Bkz. Kullanıcı gizliliği ve veri saklama yla ilgili ayrıntılar için [kişisel verileri yönetme](#managing-personal-data) bölümüne bakın.

#### <a name="does-the-solution-support-assigning-on-premises-ad-groups-to-the-user"></a>Çözüm, şirket içi REKLAM gruplarının kullanıcıya atanmasını destekliyor mu?

Bu işlevsellik şu anda desteklenmez. Önerilen geçici çözüm, [denetim günlüğü verileri](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-beta) için Microsoft Graph API bitiş noktasını sorgulayan bir PowerShell komut dosyası dağıtmak ve bunu grup ataması gibi senaryoları tetiklemek için kullanmaktır. Bu PowerShell komut dosyası bir görev zamanlayıcısına eklenebilir ve sağlama aracısını çalıştıran aynı kutuda dağıtılabilir.  

#### <a name="which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles"></a>Çözüm, İş Günü çalışanı profillerini sorgulamak ve güncelleştirmek için hangi İş Günü API'lerini kullanır?

Çözüm şu anda aşağıdaki İş Günü API'lerini kullanır:

* Get_Workers (v21.1) işçi bilgilerini almak için
* İş E-posta Yazma özelliği için Maintain_Contact_Information (v26.1)
* Kullanıcı adı Yazma özelliği için Update_Workday_Account (v31.2)

#### <a name="can-i-configure-my-workday-hcm-tenant-with-two-azure-ad-tenants"></a>İş günü HCM kiracımı iki Azure AD kiracısıyla yapılandırabilir miyim?

Evet, bu yapılandırma desteklenir. Bu senaryoyu yapılandırmak için üst düzey adımlar şunlardır:

* Geçici aracı #1 dağıtın ve Azure AD kiracı #1 kaydedin.
* Geçici aracı #2 dağıtın ve Azure AD kiracı #2 kaydedin.
* Her Geçici Aracının yöneteceği "Alt Etki Alanları"na göre, her aracı etki alanı(lar) ile yapılandırın. Bir aracı birden çok etki alanı işleyebilir.
* Azure portalında, İş Günü'nü her kiracıda AD Kullanıcı Sağlama Uygulamasına ayarla ve ilgili etki alanlarıyla yapılandırın.

#### <a name="why-workday-to-azure-ad-user-provisioning-app-is-not-supported-if-we-have-deployed-azure-ad-connect"></a>Azure AD Connect'i dağıttıysak neden "Azure AD'ye Çalışma Günü" kullanıcı sağlama uygulaması desteklenmez?

Azure AD karma modda kullanıldığında (bulut + şirket içi kullanıcıların bir karışımını içerdiği durumlarda), "yetki kaynağı" tanımının net bir tanımına sahip olmak önemlidir. Genellikle karma senaryolar Azure AD Connect'in dağıtımını gerektirir. Azure AD Connect dağıtıldığında, şirket içi AD yetki kaynağıdır. İş Günü'nü Azure AD bağlayıcısına tanıtmak, İş Günü öznitelik değerlerinin Azure AD Connect tarafından belirlenen değerlerin üzerine yazabileceği bir duruma yol açabilir. Bu nedenle Azure AD Connect etkinleştirildiğinde "Azure AD'ye Çalışma Günü" sağlama uygulamasının kullanımı desteklenmez. Bu gibi durumlarda, kullanıcıları şirket içi REKLAM'a sokmak ve Azure AD Connect'i kullanarak Azure AD'ye senkronize etmek için "AD Kullanıcıya Çalışma Günü" sağlama uygulamasını kullanmanızı öneririz.

#### <a name="how-do-i-suggest-improvements-or-request-new-features-related-to-workday-and-azure-ad-integration"></a>İş Günü ve Azure REKLAM tümleştirmesi ile ilgili iyileştirmeleri nasıl önerirveya yeni özellikler isteyebilirim?

Geri bildiriminiz, gelecekteki sürümler ve geliştirmelerin yönünü belirlememize yardımcı olduğu için son derece değerlidir. Tüm geri bildirimleri memnuniyetle karşılarız ve Azure [AD'nin geri bildirim forumunda](https://feedback.azure.com/forums/169401-azure-active-directory)fikrinizi veya geliştirme önerinizi göndermenizi öneririz. İş Günü tümleştirmesi ile ilgili belirli geri bildirimler için, İş Günü ile ilgili varolan geri bildirimleri bulmak için *SaaS Uygulamaları* kategorisini seçin ve *İş Günü* anahtar kelimelerini kullanarak arama yapın.

![UserVoice SaaS Uygulamaları](media/workday-inbound-tutorial/uservoice_saas_apps.png)

![UserVoice İş Günü](media/workday-inbound-tutorial/uservoice_workday_feedback.png)

Yeni bir fikir önerirken, lütfen başka birinin benzer bir özellik önerip önerdiğini kontrol edin. Bu durumda, özelliği veya geliştirme isteğini oylayabilirsiniz. Ayrıca, fikre olan desteğinizi göstermek ve özelliğin sizin için de ne kadar değerli olacağını göstermek için özel kullanım durumunuzla ilgili bir yorum bırakabilirsiniz.

### <a name="provisioning-agent-questions"></a>Agent sorularını sağlama

#### <a name="what-is-the-ga-version-of-the-provisioning-agent"></a>Provizyon Aracısı'nın GA sürümü nedir?

* Provizyon Aracısı'nın GA sürümü 1.1.30 ve üzeridir.
* Aracı sürümünüz 1.1.30'dan azsa, genel önizleme sürümünü çalıştırMışsınız dır ve aracıyı barındıran sunucuda .NET 4.7.1 çalışma süresi varsa, genel önizleme sürümünü çalıştırAbilirsiniz ve otomatik olarak GA sürümüne güncelleştirilir.
  * Sunucunuzda yüklü [olan .NET sürümünü kontrol](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) edebilirsiniz. Sunucu .NET 4.7.1 çalışmıyorsa [,NET 4.7.1'i indirip yükleyebilirsiniz.](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows) .NET 4.7.1'i yükledikten sonra sağlama aracınız GA sürümüne otomatik olarak güncellenir.

#### <a name="how-do-i-know-the-version-of-my-provisioning-agent"></a>Provizyon Ajanımın sürümünü nasıl bilirim?

* Sağlama Aracısı'nın yüklü olduğu Windows sunucusunda oturum açın.
* Denetim **Masası'na** -> git**Kaldır veya Programı Değiştir** menüsünü değiştir
* Microsoft Azure AD Connect **Provisioning Aracısı** girişine karşılık gelen sürümü arayın

  ![Azure portalında](./media/workday-inbound-tutorial/pa_version.png)

#### <a name="does-microsoft-automatically-push-provisioning-agent-updates"></a>Microsoft, Provisioning Agent güncelleştirmelerini otomatik olarak itiyor mu?

Evet, Microsoft sağlama aracısını otomatik olarak güncelleştirir. Windows hizmetini durdurarak otomatik güncelleştirmeleri devre dışı kullanabilirsiniz **Microsoft Azure AD Connect Agent Updater.**

#### <a name="can-i-install-the-provisioning-agent-on-the-same-server-running-azure-ad-connect"></a>Azure AD Connect çalıştıran aynı sunucuya Sağlama Aracısı yükleyebilir miyim?

Evet, Sağlama Aracısını Azure AD Connect'i çalıştıran sunucuya yükleyebilirsiniz.

#### <a name="at-the-time-of-configuration-the-provisioning-agent-prompts-for-azure-ad-admin-credentials-does-the-agent-store-the-credentials-locally-on-the-server"></a>Yapılandırma sırasında, Azure AD yönetici kimlik bilgileri için Sağlama Aracısı istemleri. Aracı, kimlik bilgilerini sunucuda yerel olarak depolar mı?

Yapılandırma sırasında, Sağlama Aracısı Yalnızca Azure AD yöneticinizin kiracınıza bağlanması nı ister. Kimlik bilgilerini sunucuda yerel olarak depolamaz. Ancak, yerel bir Windows parola tonozundaki *şirket içi Active Directory etki alanına* bağlanmak için kullanılan kimlik bilgilerini korur.

#### <a name="how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication"></a>Giden HTTP iletişimi için bir proxy sunucusu kullanmak üzere Geçici Aracıyı nasıl yapılandırıyorum?

Geçici Aracı giden proxy kullanımını destekler. Aracı config dosyasını düzenleyerek yapılandırabilirsiniz **C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config**. Kapanış `</configuration>` etiketinden hemen önce dosyanın sonuna doğru aşağıdaki satırları ekleyin.
Değişkenleri [proxy-server] ve [proxy-port] ile proxy sunucu adı ve bağlantı noktası değerleri değiştirin.

```xml
    <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
             <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
             />
         </defaultProxy>
    </system.net>
```

#### <a name="how-do-i-ensure-that-the-provisioning-agent-is-able-to-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent"></a>Sağlama Aracısının Azure AD kiracısıyla iletişim kurabilmesinden ve aracının gerektirdiği bağlantı noktalarını engelleyen güvenlik duvarları olmadığından nasıl emin olabilirim?

[Konektör Bağlantı Noktaları Test Aracı'nı](https://aadap-portcheck.connectorporttest.msappproxy.net/) şirket içi ağınızdan açarak gerekli tüm bağlantı noktalarını açıp açmadığınızı da kontrol edebilirsiniz. Daha fazla yeşil onay işareti daha fazla esneklik anlamına gelir.

Aracın size doğru sonuçları verdiğinden emin olmak için aşağıdakilerden emin olun:

* Aracı, Sağlama Aracısını yüklediğiniz sunucudan bir tarayıcıda açın.
* Bu sayfaya, Geçici Aracınız için geçerli olan ek veya güvenlik duvarlarının da uygulandığından emin olun. Bu, Internet Explorer'da **Ayarlar -> Internet Seçenekleri -> Bağlantıları -> LAN Ayarları'na**giderek yapılabilir. Bu sayfada, "LAN'ınız için Proxy Sunucusu kullanın" alanını görürsünüz. Bu kutuyu seçin ve proxy adresini "Adres" alanına koyun.

#### <a name="can-one-provisioning-agent-be-configured-to-provision-multiple-ad-domains"></a>Bir Sağlama Aracısı birden çok AD etki alanı sağlamak üzere yapılandırılabilir mi?

Evet, bir Sağlama Aracısı, aracının ilgili etki alanı denetleyicilerine görüş hattı olduğu sürece birden çok AD etki alanını işlenecek şekilde yapılandırılabilir. Microsoft, yüksek kullanılabilirlik sağlamak ve destek üzerinde başarısız olmasını sağlamak için aynı AD etki alanı kümesine hizmet veren 3 geçici aracılardan oluşan bir grup oluşturmanızı önerir.

#### <a name="how-do-i-de-register-the-domain-associated-with-my-provisioning-agent"></a>Geçici Aracımla ilişkili etki alanının kaydını nasıl açabilirim?

* Azure portalından Azure AD kiracınızın *kiracı kimliğini* alın.
* Provisioning Agent'ı çalıştıran Windows sunucusunda oturum açın.
* PowerShell'i Windows Yöneticisi olarak açın.
* Kayıt komut dosyalarını içeren dizine geçiş inizi değiştirin \[ve\] kiracı kimlik parametrenizi kiracı kimliğinizin değeriyle değiştirerek aşağıdaki komutları çalıştırın.

  ```powershell
  cd “C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder”
  Import-Module "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\RegistrationPowershell\Modules\PSModulesFolder\AppProxyPSModule.psd1"
  Get-PublishedResources -TenantId "[tenant ID]"
  ```

* Görünen aracılar listesinden – *kaynak* Adı `id` AD etki alanı adınıza eşit olan bu kaynaktan alanın değerini kopyalayın.
* Kimlik değerini bu komuta yapıştırın ve PowerShell'deki komutu çalıştırın.

  ```powershell
  Remove-PublishedResource -ResourceId "[resource ID]" -TenantId "[tenant ID]"
  ```

* Aracı yapılandırma sihirbazını yeniden çalıştırın.
* Daha önce bu etki alanına atanmış diğer aracıların yeniden yapılandırılması gerekir.

#### <a name="how-do-i-uninstall-the-provisioning-agent"></a>Geçici Aracıyı nasıl kaldıracağım?

* Sağlama Aracısı'nın yüklü olduğu Windows sunucusunda oturum açın.
* Denetim **Masası'na** -> git**Kaldır veya Programı Değiştir** menüsünü değiştir
* Aşağıdaki programları kaldırın:
  * Microsoft Azure AD Connect Sağlama Aracısı
  * Microsoft Azure AD Bağlantı Aracısı Güncelleyici
  * Microsoft Azure AD Connect Sağlama Aracısı Paketi

### <a name="workday-to-ad-attribute-mapping-and-configuration-questions"></a>AD öznitelik eşleme ve yapılandırma soruları için iş günü

#### <a name="how-do-i-back-up-or-export-a-working-copy-of-my-workday-provisioning-attribute-mapping-and-schema"></a>İş Günü Sağlama Öznitelik Haritalama ve Şema'mın çalışan bir kopyasını nasıl yedeklerveya dışa aktarım?

İş Günü Kullanıcı Sağlama yapılandırmanızı dışa aktarmak için Microsoft Graph API'yi kullanabilirsiniz. Ayrıntılar [için İş Günü Kullanıcı Sağlama Öznitelik Eşleme yapılandırmanızı Dışa](#exporting-and-importing-your-configuration) Aktarma ve Alma bölümündeki adımlara bakın.

#### <a name="i-have-custom-attributes-in-workday-and-active-directory-how-do-i-configure-the-solution-to-work-with-my-custom-attributes"></a>İş Günü ve Active Directory'de özel özelliklerim var. Çözümü özel özniteliklerimle çalışacak şekilde nasıl yapılandırıyorum?

Çözüm, özel İş Günü ve Etkin Dizin özniteliklerini destekler. Özel özniteliklerinizi eşleme şemasına eklemek **için, Attribute Mapping** blade'i açın ve gelişmiş **seçenekleri göster**bölümünü genişletmek için aşağı kaydırın. 

![Öznitelik Listesini Edit](./media/workday-inbound-tutorial/wd_edit_attr_list.png)

Özel İş Günü özniteliklerinizi eklemek *için, İş Günü için öznitelik edit listesini* seçin ve özel AD özniteliklerinizi eklemek *için, Yerinde Etkin Dizini için öznitelik edit listesini*seçin.

Ayrıca bkz:

* [İş Günü kullanıcı öznitelikleri listesini özelleştirme](#customizing-the-list-of-workday-user-attributes)

#### <a name="how-do-i-configure-the-solution-to-only-update-attributes-in-ad-based-on-workday-changes-and-not-create-any-new-ad-accounts"></a>Çözümü, yalnızca İş Günü değişikliklerine dayalı olarak AD'deki öznitelikleri güncelleştirmek ve yeni AD hesapları oluşturmak için nasıl yapılandırıyorum?

Bu yapılandırma, aşağıda gösterildiği gibi **Atföz Eşlemeler** bıçak **Hedef Nesne Eylemleri** ayarlayarak elde edilebilir:

![Eylemi güncelleştir](./media/workday-inbound-tutorial/wd_target_update_only.png)

Yalnızca Iş Günü'nden AD'ye akacak güncelleştirme işlemleri için "Güncelleştir" onay kutusunu seçin. 

#### <a name="can-i-provision-users-photo-from-workday-to-active-directory"></a>Kullanıcının fotoğrafını İş Günü'nden Active Directory'ye sağlayabilir miyim?

Çözüm şu anda Active *Directory'de küçük resimPhoto* ve *jpegPhoto* gibi ikili öznitelikleri ayarlamayı desteklemez.

#### <a name="how-do-i-sync-mobile-numbers-from-workday-based-on-user-consent-for-public-usage"></a>İş Günü'ndeki cep telefonu numaralarını, genel kullanım için kullanıcı onayına dayanarak nasıl senkronize edebilirim?

* İş Günü Sağlama Uygulamanızın "Sağlama" bıçağına gidin.
* Öznitelik Eşlemeleri'ne tıklayın 
* **Eşlemeler**altında, **İş Günü Çalışanlarını Şirket Içi Etkin Dizini Eşitle'yi** seçin (veya İş Günü Çalışanlarını Azure AD ile **Senkronize Edin).**
* Attribute Eşlemeler sayfasında, aşağı kaydırın ve "Gelişmiş Seçenekleri Göster" kutusunu işaretleyin.  İş **Günü için Öznitelik Listesini Edit'e** tıklayın
* Açılan bıçakta, "Mobil" özniteliğini bulun ve **API Expression** ![Mobile GDPR'ı yeniden yapabilmeniz için satıra tıklayın](./media/workday-inbound-tutorial/mobile_gdpr.png)

* **API İfadesini,** yalnızca "Genel Kullanım Bayrağı" İş Günü'nde "True" olarak ayarlanmışsa iş cep telefonu numarasını alan aşağıdaki yeni ifadeyle değiştirin.

    ```
     wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Contact_Data/wd:Phone_Data[translate(string(wd:Phone_Device_Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='MOBILE' and translate(string(wd:Usage_Data/wd:Type_Data/wd:Type_Reference/@wd:Descriptor),'abcdefghijklmnopqrstuvwxyz','ABCDEFGHIJKLMNOPQRSTUVWXYZ')='WORK' and string(wd:Usage_Data/@wd:Public)='1']/@wd:Formatted_Phone
    ```

* Öznitelik Listesini Kaydet.
* Öznitelik Eşlemi kaydet.
* Geçerli durumu temizleyin ve tam eşitlemeyi yeniden başlatın.

#### <a name="how-do-i-format-display-names-in-ad-based-on-the-users-departmentcountrycity-attributes-and-handle-regional-variances"></a>Ad'de görüntüleme adlarını kullanıcının departman/ülke/şehir özelliklerine göre nasıl biçimlendirebilirim ve bölgesel farkları nasıl işlerim?

Ayrıca kullanıcının departmanı ve ülke /bölge hakkında bilgi sağlamak için *AD'de displayName* özniteliği ni yapılandırmak yaygın bir gereksinimdir. Örneğin John Smith ABD'de Pazarlama Departmanı'nda çalışıyorsa, *displayName'inin* *Smith, John (Marketing-US)* olarak gösterilmesini isteyebilirsiniz.

Şirket, iş birimi, şehir veya ülke/bölge gibi özellikleri içerecek şekilde *CN* veya *displayName* oluşturmak için bu tür gereksinimleri şu şekilde işleyebilirsiniz.

* Her İş Günü özniteliği, İş Günü için **Öznitelik > Gelişmiş Bölüm -> Edit öznitelik listesinde**yapılandırılabilir olan altta yatan bir XPATH API ifadesi kullanılarak alınır. İşte Workday *PreferredFirstName*için varsayılan XPATH API ifadesi , *PreferredLastName*, *Şirket* ve *Denetleyici Organizasyon* öznitelikleri.

     | İş Günü Özniteliği | API XPATH İfadesi |
     | ----------------- | -------------------- |
     | Tercih Edilen İlkAd | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:First_Name/text() |
     | Tercih Edilen Son Ad | wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Name_Data/wd:Preferred_Name_Data/wd:Name_Detail_Data/wd:Last_Name/text() |
     | Şirket | wd:Worker/wd:Worker_Data/wd:Organization_Data/wd:Worker_Organization_Data[wd:Organization_Data/wd:Organization_Type_Reference/wd:ID[@wd:type='Organization_Type_ID']]='Şirket']/wd:Organization_Reference/@wd:Descriptor |
     | Denetleyici Organizasyon | wd:Worker/wd:Worker_Data/wd:Organization_Data/wd:Worker_Organization_Data/wd:Organization_Data[wd:Organization_Type_Reference/wd:ID[@wd:type='Organization_Type_ID']='Supervisory']/wd:Organization_Name/text() |
  
   İş Günü ekibinizle yukarıdaki API ifadesinin İş Günü kiracı yapılandırmanız için geçerli olduğunu doğrulayın. Gerekirse, [bunları İş Günü kullanıcı öznitelikleri listesini özelleştirme](#customizing-the-list-of-workday-user-attributes)bölümünde açıklandığı şekilde yeniden atabilirsiniz.

* Benzer şekilde, İş Günü'nde bulunan ülke bilgileri aşağıdaki XPATH kullanılarak alınır: *wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference*

     İş Günü öznitelik listesi bölümünde bulunan ülkeyle ilgili 5 öznitelik vardır.

     | İş Günü Özniteliği | API XPATH İfadesi |
     | ----------------- | -------------------- |
     | Ülke Referans | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Alpha-3_Code']/text() |
     | CountryReferenceFriendly | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/@wd:Descriptor |
     | ÜlkeReferansSayısal | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Numeric-3_Code']/text() |
     | ÜlkeReferansİki Mektubu | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Reference/wd:ID[@wd:type='ISO_3166-1_Alpha-2_Code']/text() |
     | CountryRegionReference | wd:Worker/wd:Worker_Data/wd:Employment_Data/wd:Position_Data/wd:Business_Site_Summary_Data/wd:Address_Data/wd:Country_Region_Reference/@wd:Descriptor |

  İş Günü ekibinizle yukarıdaki API ifadelerinin İş Günü kiracı yapılandırmanız için geçerli olduğunu doğrulayın. Gerekirse, [bunları İş Günü kullanıcı öznitelikleri listesini özelleştirme](#customizing-the-list-of-workday-user-attributes)bölümünde açıklandığı şekilde yeniden atabilirsiniz.

* Doğru öznitelik eşleme ifadesini oluşturmak için, hangi İş Günü özniteliğinin "yetkili" kullanıcının adını, soyadını, ülke/bölgesini ve bölümünü temsil ettiğini belirleyin. Öznitelikleri sırasıyla *PreferredFirstName*, *PreferredLastName*, *CountryReferenceTwoLetter* ve *SupervisoryOrganization* olduğunu söylüyorlar. *Smith, John (Marketing-US)* gibi bir görüntü adı almak için aşağıdaki gibi AD *displayName* özniteliği için bir ifade oluşturmak için kullanabilirsiniz.

    ```
     Append(Join(", ",[PreferredLastName],[PreferredFirstName]), Join(""," (",[SupervisoryOrganization],"-",[CountryReferenceTwoLetter],")"))
    ```
    Doğru ifadeye sahip olduktan sonra, Attribute Mappings tablosunu edin ve *displayName* öznitelik eşlemini aşağıda gösterildiği gibi değiştirin: ![DisplayName Mapping](./media/workday-inbound-tutorial/wd_displayname_map.png)

* Yukarıdaki örneği genişleterek, İş Günü'nden gelen şehir adlarını steno değere dönüştürmek ve daha sonra *Smith, John (CHI)* veya *Doe, Jane (NYC)* gibi ekran adları oluşturmak için kullanmak istediğinizi varsayalım, ardından bu sonuç, Determinant değişkeni olarak İş Günü *Belediyesi* özniteliği ile bir Switch ifadesi kullanılarak elde edilebilir.

     ```
    Switch
    (
      [Municipality],
      Join(", ", [PreferredLastName], [PreferredFirstName]),  
           "Chicago", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(CHI)"),
           "New York", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(NYC)"),
           "Phoenix", Append(Join(", ",[PreferredLastName], [PreferredFirstName]), "(PHX)")
    )
     ```
    Ayrıca bkz:
  * [Anahtar Fonksiyonu Sözdizimi](../app-provisioning/functions-for-customizing-application-data.md#switch)
  * [İşlev Sözdizimini Birleştirme](../app-provisioning/functions-for-customizing-application-data.md#join)
  * [Ek Işlev Sözdizimi](../app-provisioning/functions-for-customizing-application-data.md#append)

#### <a name="how-can-i-use-selectuniquevalue-to-generate-unique-values-for-samaccountname-attribute"></a>SamAccountName özniteliği için benzersiz değerler oluşturmak için SelectUniqueValue'ı nasıl kullanabilirim?

İş Günü'ndeki *FirstName* ve *Soyadı* özniteliklerinin bir birleşimini kullanarak *samAccountName* özniteliği için benzersiz değerler oluşturmak istediğinizi varsayalım. Aşağıda verilen bir ifade ile başlayabilirsiniz:

```
SelectUniqueValue(
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,1), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,2), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , ),
    Replace(Mid(Replace(NormalizeDiacritics(StripSpaces(Join("",  Mid([FirstName],1,3), [LastName]))), , "([\\/\\\\\\[\\]\\:\\;\\|\\=\\,\\+\\*\\?\\<\\>])", , "", , ), 1, 20), , "(\\.)*$", , "", , )
)
```

Yukarıdaki ifade nasıl çalışır: Kullanıcı John Smith ise, ilk JSmith oluşturmak için çalışır, JSmith zaten varsa, o zaman JoSmith oluşturur, eğer varsa, JohSmith oluşturur. İfade ayrıca, oluşturulan değerin uzunluk kısıtlamasını ve *samAccountName*ile ilişkili özel karakter kısıtlamasını karşılamasını da sağlar.

Ayrıca bkz:

* [Orta Fonksiyon sözdizimi](../app-provisioning/functions-for-customizing-application-data.md#mid)
* [İşlev Sözdizimini Değiştir](../app-provisioning/functions-for-customizing-application-data.md#replace)
* [SelectUniqueValue İşlev Sözdizimi](../app-provisioning/functions-for-customizing-application-data.md#selectuniquevalue)

#### <a name="how-do-i-remove-characters-with-diacritics-and-convert-them-into-normal-english-alphabets"></a>Aksit etekli karakterleri nasıl kaldırıp normal İngilizce alfabeye dönüştürebilirim?

Kullanıcı için e-posta adresini veya CN değerini yaparken, kullanıcının adı ve soyadındaki özel karakterleri kaldırmak için [NormalizeDiacritics](../app-provisioning/functions-for-customizing-application-data.md#normalizediacritics) işlevini kullanın.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Bu bölümde, Azure AD Denetim Günlükleri ve Windows Server Olay Görüntüleyicisi günlüklerini kullanarak Çalışma Günü tümleştirmenizdeki sağlama sorunlarını nasıl giderçalışacağınız konusunda özel kılavuzlar sağlanmaktadır. Öğretici'de yakalanan genel sorun giderme adımlarının ve kavramların üzerine inşa [eder: Otomatik kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md)

Bu bölümde sorun giderme aşağıdaki yönlerini kapsar:

* [Aracı sorun giderme için Windows Event Viewer ayarlama](#setting-up-windows-event-viewer-for-agent-troubleshooting)
* [Hizmet sorun giderme için Azure portalı Denetim Günlükleri ayarlama](#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
* [AD Kullanıcı Hesabı için günlükleri anlama işlemleri oluşturmak](#understanding-logs-for-ad-user-account-create-operations)
* [Yönetici güncelleştirme işlemleri için günlükleri anlama](#understanding-logs-for-manager-update-operations)
* [Sık karşılaşılan hataları çözme](#resolving-commonly-encountered-errors)

### <a name="setting-up-windows-event-viewer-for-agent-troubleshooting"></a>Aracı sorun giderme için Windows Event Viewer ayarlama

* Sağlama Aracısının dağıtıldığı Windows Server makinesinde oturum açma
* **Windows Server Event Viewer** masaüstü uygulamasını açın.
* **Windows Günlükleri > Uygulamasını**seçin.
* Filtre **Geçerli Günlük'i kullanın...** kaynak AAD altında günlüğe kaydedilen tüm olayları görüntüleme **seçeneği. Connect.ProvisioningAgent** ve aşağıda gösterildiği gibi filtre "-5" belirterek Olay Kimliği "5" ile olayları hariç.

  ![Windows Olay Görüntüleyici](media/workday-inbound-tutorial/wd_event_viewer_01.png))

* **Tamam'ı** tıklatın ve sonuç görünümünü **Tarih ve Saat** sütununa göre sıralayın.

### <a name="setting-up-azure-portal-audit-logs-for-service-troubleshooting"></a>Hizmet sorun giderme için Azure portalı Denetim Günlükleri ayarlama

* Azure [portalını](https://portal.azure.com)başlatın ve İş Günü sağlama uygulamanızın **Denetim günlükleri** bölümüne gidin.
* Görünümde yalnızca aşağıdaki sütunları (Tarih, Etkinlik, Durum, Durum Nedeni) görüntülemek için Denetim Günlükleri sayfasındaki **Sütunlar** düğmesini kullanın. Bu yapılandırma, yalnızca sorun giderme yle ilgili verilere odaklanmanızı sağlar.

  ![Denetim günlüğü sütunları](media/workday-inbound-tutorial/wd_audit_logs_00.png)

* Görünümü filtrelemek için **Hedef** ve **Tarih Aralığı** sorgu parametrelerini kullanın. 
  * **Hedef** sorgu parametresini İş Günü çalışanı nesnesinin "İşçi Kimliği" veya "Çalışan Kimliği" olarak ayarlayın.
  * Tarih **Aralığını,** sağlamayla ilgili hatalar veya sorunlar için araştırmak istediğiniz uygun bir zaman aralığına ayarlayın.

  ![Denetim günlüğü filtreleri](media/workday-inbound-tutorial/wd_audit_logs_01.png)

### <a name="understanding-logs-for-ad-user-account-create-operations"></a>AD Kullanıcı Hesabı için günlükleri anlama işlemleri oluşturmak

Çalışma Günü'nde yeni bir işe alım algılandığında (diyelim ki Çalışan Kimliği *21023*ile), Azure AD sağlama hizmeti çalışan için yeni bir AD kullanıcı hesabı oluşturmaya çalışır ve bu süreçte aşağıda açıklandığı gibi 4 denetim günlüğü kaydı oluşturur:

  [![Denetim günlüğü ops oluşturmak](media/workday-inbound-tutorial/wd_audit_logs_02.png)](media/workday-inbound-tutorial/wd_audit_logs_02.png#lightbox)

Denetim günlüğü kayıtlarından herhangi birini tıklattığınızda, **Etkinlik Ayrıntıları** sayfası açılır. Etkinlik **Ayrıntıları** sayfasının her günlük kayıt türü için görüntülemesi aşağıda açıklanır.

* **İş Günü Alma** kaydı: Bu günlük kaydı, İş Günü'nden getirilen alt bilgileri görüntüler. İş Günü'nden veri alma yla ilgili sorunları gidermek için günlük kaydının *Ek Ayrıntılar* bölümündeki bilgileri kullanın. Her alanın nasıl yorumlanacağına ilişkin işaretçilerle birlikte aşağıda bir örnek kayıt gösterilmiştir.

  ```JSON
  ErrorCode : None  // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportAdd // For full sync, value is "EntryImportAdd" and for delta sync, value is "EntryImport"
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID (usually the Worker ID or Employee ID field)
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the record
  ```

* **AD Alma** kaydı: Bu günlük kaydı, AD'den alınan hesabın bilgilerini görüntüler. İlk kullanıcı oluşturma sırasında AD hesabı olmadığı *için, Etkinlik Durum Nedeni,* Etkin Dizin'de Eşleşen Kimlik özniteliği değerine sahip hiçbir hesabın bulunmadığını gösterir. İş Günü'nden veri alma yla ilgili sorunları gidermek için günlük kaydının *Ek Ayrıntılar* bölümündeki bilgileri kullanın. Her alanın nasıl yorumlanacağına ilişkin işaretçilerle birlikte aşağıda bir örnek kayıt gösterilmiştir.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot Workday issues
  EventName : EntryImportObjectNotFound // Implies that object was not found in AD
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  ```

  Bu AD alma işlemine karşılık gelen Provisioning Agent günlük kayıtlarını bulmak için Windows Olay Görüntüleyicisi günlüklerini açın ve **Bul...** eşleşen kimlik/Birleştirme Özelliği öznitelik değerini içeren günlük girişlerini bulmak için menü seçeneği (bu durumda *21023).*

  ![Bul](media/workday-inbound-tutorial/wd_event_viewer_02.png)

  AD hesabını almak için aracı tarafından kullanılan LDAP arama filtresini sağlayacak *Event ID = 9*ile girişi arayın. Benzersiz kullanıcı girişlerini almak için bunun doğru arama filtresi olup olmadığını doğrulayabilirsiniz.

  ![LDAP Arama](media/workday-inbound-tutorial/wd_event_viewer_03.png)

  *Olay Kimliği = 2* ile hemen takip eden kayıt, arama işleminin sonucunu yakalar ve herhangi bir sonuç döndürürse.

  ![LDAP Sonuçları](media/workday-inbound-tutorial/wd_event_viewer_04.png)

* **Eşitleme kuralı eylem** kaydı: Bu günlük kaydı, gelen İş Günü olayını işlemek için yapılacak sağlama eylemiyle birlikte öznitelik eşleme kurallarının ve yapılandırılmış kapsam filtrelerinin sonuçlarını görüntüler. Eşitleme eylemiyle ilgili sorunları gidermek için günlük kaydının *Ek Ayrıntılar* bölümündeki bilgileri kullanın. Her alanın nasıl yorumlanacağına ilişkin işaretçilerle birlikte aşağıda bir örnek kayıt gösterilmiştir.

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot sync issues
  EventName : EntrySynchronizationAdd // Implies that the object will be added
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  ```

  Öznitelik eşleme ifadelerinizle ilgili sorunlar varsa veya gelen İş Günü verilerinde sorunlar varsa (örneğin: gerekli öznitelikler için boş veya boş değer), bu aşamada hatanın ayrıntılarını belirten ErrorCode ile bir hata gözlemlersiniz.

* **AD Dışa Aktarma** kaydı: Bu günlük kaydı, işlemde ayarlanan öznitelik değerleriyle birlikte AD hesabı oluşturma işleminin sonucunu görüntüler. Hesap oluşturma işlemiyle ilgili sorunları gidermek için günlük kaydının *Ek Ayrıntılar* bölümündeki bilgileri kullanın. Her alanın nasıl yorumlanacağına ilişkin işaretçilerle birlikte aşağıda bir örnek kayıt gösterilmiştir. "Ek Ayrıntılar" bölümünde ,"EventName" "EntryExportAdd" olarak ayarlanır, "JoiningProperty" Eşleşen Kimlik özniteliğinin değerine ayarlanır, "SourceAnchor" kayıtla ilişkili WorkdayID (WID) ve "TargetAnchor" yeni oluşturulan kullanıcının AD "ObjectGuid" özniteliğinin değeri. 

  ```JSON
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportAdd // Implies that object will be created
  JoiningProperty : 21023 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : a071861412de4c2486eb10e5ae0834c3 // set to the WorkdayID (WID) associated with the profile in Workday
  TargetAnchor : 83f0156c-3222-407e-939c-56677831d525 // set to the value of the AD "objectGuid" attribute of the new user
  ```

  Bu AD dışa aktarma işlemine karşılık gelen Provisioning Agent günlük kayıtlarını bulmak için Windows Olay Görüntüleyicisi günlüklerini açın ve **Bul...** eşleşen kimlik/Birleştirme Özelliği öznitelik değerini içeren günlük girişlerini bulmak için menü seçeneği (bu durumda *21023).*  

  *Olay Kimliği = 2*ile ihracat işleminin zaman damgasına karşılık gelen bir HTTP POST kaydı arayın. Bu kayıt, sağlama hizmeti tarafından gönderen öznitelik değerlerini içerir.

  [![SCIM Ekle](media/workday-inbound-tutorial/wd_event_viewer_05.png)](media/workday-inbound-tutorial/wd_event_viewer_05.png#lightbox)

  Yukarıdaki olaydan hemen sonra, ad hesabı oluşturma işleminin yanıtını yakalayan başka bir olay olmalıdır. Bu olay, AD'de oluşturulan yeni objectGuid'i döndürür ve sağlama hizmetinde TargetAnchor özniteliği olarak ayarlanır.

  [![SCIM Ekle](media/workday-inbound-tutorial/wd_event_viewer_06.png)](media/workday-inbound-tutorial/wd_event_viewer_06.png#lightbox)

### <a name="understanding-logs-for-manager-update-operations"></a>Yönetici güncelleştirme işlemleri için günlükleri anlama

Yönetici özniteliği, AD'deki bir başvuru özniteliğidir. Sağlama hizmeti, yönetici özniteliğini kullanıcı oluşturma işleminin bir parçası olarak ayarlamaz. Bunun yerine yönetici özniteliği, kullanıcı için AD hesabı oluşturulduktan sonra bir *güncelleştirme* işleminin parçası olarak ayarlanır. Yukarıdaki örneği genişleterek, İş Günü'nde "21451" olan yeni bir işe alma nın etkinleştirildiğini ve yeni işe alma yöneticisinin *(21023)* zaten bir AD hesabı olduğunu varsayalım. Bu senaryoda, kullanıcı 21451 için Denetim günlükleri arama 5 girişleri gösterir.

  [![Yönetici Güncellemesi](media/workday-inbound-tutorial/wd_audit_logs_03.png)](media/workday-inbound-tutorial/wd_audit_logs_03.png#lightbox)

İlk 4 kayıt, kullanıcı oluşturma işleminin bir parçası olarak araştırdığımız kayıtlara benzer. 5. kayıt, yönetici öznitelik güncelleştirmesiyle ilişkili dışa aktarmadır. Günlük kaydı, yöneticinin *objectGuid* özniteliği kullanılarak gerçekleştirilen AD hesap yöneticisi güncelleştirme işleminin sonucunu görüntüler.

  ```JSON
  // Modified Properties
  Name : manager
  New Value : "83f0156c-3222-407e-939c-56677831d525" // objectGuid of the user 21023

  // Additional Details
  ErrorCode : None // Use the error code captured here to troubleshoot AD account creation issues
  EventName : EntryExportUpdate // Implies that object will be created
  JoiningProperty : 21451 // Value of the Workday attribute that serves as the Matching ID
  SourceAnchor : 9603bf594b9901693f307815bf21870a // WorkdayID of the user
  TargetAnchor : 43b668e7-1d73-401c-a00a-fed14d31a1a8 // objectGuid of the user 21451

  ```

### <a name="resolving-commonly-encountered-errors"></a>Sık karşılaşılan hataları çözme

Bu bölümde, İş Günü kullanıcı sağlama ile sık görülen hataları ve nasıl çözüleceğini kapsar. Hatalar aşağıdaki gibi gruplandırılır:

* [Aracı hatalarını sağlama](#provisioning-agent-errors)
* [Bağlantı hataları](#connectivity-errors)
* [AD kullanıcı hesabı oluşturma hataları](#ad-user-account-creation-errors)
* [AD kullanıcı hesabı güncelleştirme hataları](#ad-user-account-update-errors)

#### <a name="provisioning-agent-errors"></a>Aracı hatalarını sağlama

|#|Hata Senaryosu |Olası Nedenler|Önerilen Çözünürlük|
|--|---|---|---|
|1.| Hata iletisi ile bulma aracısını yükleme hatası: *'Microsoft Azure AD Connect Provisioning Agent' (AADConnectProvisioningAgent) hizmeti başlatılamamış. Sistemi başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın.* | Bir etki alanı denetleyicisine geçici aracıyı yüklemeye çalışıyorsanız ve grup ilkesi hizmetin başlatılmasını engelliyorsa, bu hata genellikle ortaya çıkar.  Ayrıca, çalışan aracının önceki bir sürümü varsa ve yeni bir yükleme başlatmadan önce kaldırmamışsanız da görülür.| Sağlama aracısını DC olmayan bir sunucuya yükleyin. Yeni aracıyı yüklemeden önce aracının önceki sürümlerinin kaldırdığından emin olun.|
|2.| Windows Hizmeti 'Microsoft Azure AD Connect Provisioning Agent' *Başlangıç* durumundadır ve *Çalışma* durumuna geçmez. | Yüklemenin bir parçası olarak, aracı sihirbazı sunucuda yerel bir hesap **(NT Service\\AADConnectProvisioningAgent)** oluşturur ve bu hizmeti başlatmak için kullanılan oturum açma hesabıdır. Windows sunucunuzdaki bir güvenlik ilkesi yerel hesapların hizmetleri çalıştırmasını engelliyorsa, bu hatayla karşılaşırsınız. | Hizmetler *konsolu'nu*açın. Windows Hizmeti 'Microsoft Azure AD Connect Provisioning Agent' ve oturum açma sekmesinde hizmeti çalıştırmak için bir etki alanı yöneticisinin hesabını belirtin sağ tıklayın. Hizmeti yeniden başlatın. |
|3.| Active *Directory'yi bağla*adımda AD etki alanınız ile birlikte sağlama aracısını yapılandırırken, sihirbaz AD şemasını yüklemeye çalışırken uzun zaman alır ve sonunda zaman ları dışarı çıkarır. | Bu hata genellikle, güvenlik duvarı sorunlarından dolayı sihirbaz AD etki alanı denetleyicisi sunucusuna bağlanamadığında gösterilir. | Etkin *Dizin Bağlantısı* sihirbazı ekranında, AD etki alanınızın kimlik bilgilerini sağlarken, *etki alanı denetleyicisini seç önceliği*adlı bir seçenek vardır. Aracı sunucusuyla aynı sitede bulunan bir etki alanı denetleyicisi seçmek ve iletişimi engelleyen güvenlik duvarı kuralları olmadığından emin olmak için bu seçeneği kullanın. |

#### <a name="connectivity-errors"></a>Bağlantı hataları

Sağlama hizmeti İş Günü'ne veya Etkin Dizin'e bağlanamıyorsa, hükmün karantinaya alınmasına neden olabilir. Bağlantı sorunlarını gidermek için aşağıdaki tabloyu kullanın.

|#|Hata Senaryosu |Olası Nedenler|Önerilen Çözünürlük|
|--|---|---|---|
|1.| **Test Bağlantısı'nı**tıklattığınızda, hata iletisi alırsınız: *Active Directory'ye bağlanan bir hata oluştu. Lütfen şirket içi Sağlama Aracısının çalıştığını ve doğru Active Directory etki alanıyla yapılandırıldığından emin olun.* | Bu hata genellikle, sağlama aracısı çalışmıyorsa veya Azure AD ile sağlama aracısı arasındaki iletişimi engelleyen bir güvenlik duvarı varsa ortaya çıkar. Etki alanı Aracı Sihirbazı'nda yapılandırılmamışsa, bu hatayı da görebilirsiniz. | Aracının çalıştığını doğrulamak için Windows sunucusundaki *Hizmetler* konsolunu açın. Sağlama aracısihirbazı sihirbazını açın ve doğru etki alanının aracıya kayıtlı olduğunu onaylayın.  |
|2.| Sağlama işi hafta sonları karantina durumuna geçer (Cum-Sat) ve senkronizasyonda bir hata olduğuna dair bir e-posta bildirimi alıyoruz. | Bu hatanın yaygın nedenlerinden biri Workday'in planlı kapalı kalma süresidir. Workday uygulama kiracısını kullanıyorsanız, Workday'in uygulama kiracıları için hafta sonları zamanladığı kapalı kalma sürelerini (genellikle Cuma akşamından Cumartesi sabahına kadar) ve bu süre boyunca Workday sağlama uygulamalarının Workday'e bağlanamadığı için karantina durumuna geçebileceğini unutmayın. Workday uygulama kiracısı yeniden çevrimiçi olduğunda normal durumuna geri döner. Nadir durumlarda, kiracı yenilendiği veya hesap kilitlendiği ya da süresi dolduğu için Tümleştirme Sistemi Kullanıcısının parolası değiştirildiğinde de bu hatayı görebilirsiniz. | Workday'in kapalı kalma zamanlamasını öğrenmek için Workday yöneticinize veya iş ortağınıza danışın. Kapalı kalma süresince uyarı iletilerini yoksayın ve Workday örneği yeniden çevrimiçi olduğunda kullanılabilirliği onaylayın.  |


#### <a name="ad-user-account-creation-errors"></a>AD kullanıcı hesabı oluşturma hataları

|#|Hata Senaryosu |Olası Nedenler|Önerilen Çözünürlük|
|--|---|---|---|
|1.| Hata: OperationsError-SvcErr: Bir işlem hatası oluştu iletisi ile denetim günlüğündeki işlem hatalarını dışa *aktarma. Dizin hizmeti için üstün bir başvuru yapılandırılmamamıştır. Bu nedenle dizin hizmeti, bu ormanın dışındaki nesnelere yönlendirme düzenleyemiyor.* | *Active Directory Container* OU doğru ayarlanmazsa veya *parentDistinguishedName*için kullanılan İfade Eşlemesi ile ilgili sorunlar varsa bu hata genellikle görünür. | Yazım hataları için *Active Directory Container* OU parametresini kontrol edin. Öznitelik eşlemesinde *parentDistinguishedName* kullanıyorsanız, bunun her zaman AD etki alanı içinde bilinen bir kapsayıcı olarak değerlendirildiğinden emin olun. Oluşturulan değeri görmek için denetim günlüklerinde *Dışa* Aktarma olayını denetleyin. |
|2.| Hata kodu ile denetim günlüğündeki işlem hatalarını dışa aktarma: *SystemForCrossDomainIdentityManagementBadResponse* ve mesaj *Hatası: ConstraintViolation-AtrErr: İstekteki bir değer geçersizdir. Öznitelik için bir değer kabul edilebilir değerler aralığında değildi. \nHata Detayları: CONSTRAINT_ATT_TYPE - şirket*. | Bu hata *şirket* özniteliğine özgü olsa da, *cn* gibi diğer öznitelikler için de bu hatayı görebilirsiniz. Bu hata, AD zorunlu şema kısıtlaması nedeniyle görünür. Varsayılan olarak, *AD'deki şirket* ve *CN* gibi özniteliklerin üst sınırı 64 karakterdir. Workday'den gelen değer 64 karakterden fazlaysa, bu hata iletisini görürsünüz. | Hata iletisinde bildirilen öznitelik değerini görmek için denetim günlüklerinde *Dışa* Aktarma olayını denetleyin. [Orta](../app-provisioning/functions-for-customizing-application-data.md#mid) işlevi kullanarak İş Günü'nden gelen değeri kesmeyi veya eşlemeleri benzer uzunluk kısıtlamaları olmayan bir AD özniteliğiyle değiştirmeyi düşünün.  |

#### <a name="ad-user-account-update-errors"></a>AD kullanıcı hesabı güncelleştirme hataları

AD kullanıcı hesabı güncelleştirme işlemi sırasında, sağlama hizmeti hem İş günü hem de AD'den bilgileri okur, öznitelik eşleme kurallarını çalıştırAr ve herhangi bir değişikliğin yürürlüğe girmesi gerekip gerekmeyeceğini belirler. Buna göre bir güncelleştirme olayı tetiklenir. Bu adımlardan herhangi biri bir hatayla karşılaşırsa, denetim günlüklerinde günlüğe kaydedilir. Sık karşılaşılan güncelleştirme hatalarını gidermek için aşağıdaki tabloyu kullanın.

|#|Hata Senaryosu |Olası Nedenler|Önerilen Çözünürlük|
|--|---|---|---|
|1.| Denetim günlüğündeki senkronizasyon kuralı eylem hataları *ile eventname = EntrySynchronizationError and ErrorCode = EndpointUnavailable*. | Bu hata, sağlama hizmeti, şirket içi sağlama aracısının karşılaştığı bir işlem hatası nedeniyle Active Directory'den kullanıcı profili verilerini alamıyorsa ortaya çıkar. | Okuma işlemiyle ilgili sorunları gösteren hata olayları için Madde Sağlama Aracısı Olay Görüntüleyicisi günlüklerini denetleyin (Event ID #2 tarafından filtreleyin). |
|2.| AD'deki yönetici özniteliği, AD'deki belirli kullanıcılar için güncelleştirilmeyecektir. | Bu hatanın en olası nedeni kapsam kuralları kullanıyorsanız ve kullanıcı yöneticisi kapsamın bir parçası değilse. Yöneticinin eşleşen kimlik özniteliği (örn. EmployeeID) hedef AD etki alanında bulunmazsa veya doğru değere ayarlanmazsa da bu sorunla karşılaşabilirsiniz. | Kapsam filtresini gözden geçirin ve yönetici kullanıcıyı kapsama ekleyin. Eşleşen kimlik özniteliği için bir değer olduğundan emin olmak için AD'deki yöneticinin profilini denetleyin. |

## <a name="managing-your-configuration"></a>Yapılandırmanızı yönetme

Bu bölümde, İş Günü odaklı kullanıcı sağlama yapılandırmanızı nasıl daha da genişletebileceğiniz, özelleştirebileceğiniz ve yönetebileceğiniz açıklanmaktadır. Aşağıdaki konuları kapsar:

* [İş Günü kullanıcı öznitelikleri listesini özelleştirme](#customizing-the-list-of-workday-user-attributes)  
* [Yapılandırmanızı dışarı ve içeri aktarma](#exporting-and-importing-your-configuration)

### <a name="customizing-the-list-of-workday-user-attributes"></a>İş Günü kullanıcı öznitelikleri listesini özelleştirme

Active Directory ve Azure AD için İş Günü sağlama uygulamaları, aralarından seçim yapabileceğiniz Varsayılan İş Günü kullanıcı öznitelikleri listesini içerir. Ancak, bu listeler kapsamlı değildir. İş günü, standart veya İş Günü kiracınıza özgü olabilecek yüzlerce olası kullanıcı özniteliğini destekler.

Azure AD sağlama hizmeti, İnsan Kaynakları API'sinin [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) çalışmasına maruz kalan öznitelikleri içerecek şekilde listenizi veya İş Günü özniteliğinizi özelleştirme olanağını destekler.

Bu değişikliği yapmak için, kullanmak istediğiniz öznitelikleri temsil eden XPath ifadelerini ayıklamak ve azure portalındaki gelişmiş öznitelik düzenleyicisini kullanarak bunları sağlama yapılandırmanıza eklemek için [Workday Studio'yu](https://community.workday.com/studio-download) kullanmanız gerekir.

**İş Günü kullanıcı özniteliği için bir XPath ifadesi almak için:**

1. [Workday Studio'u](https://community.workday.com/studio-download)indirin ve kurun. Yükleyiciye erişmek için bir İş Günü topluluk hesabına ihtiyacınız olacaktır.

2. İş Günü Human_Resources WSDL dosyasını bu URL'den indirin:https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. İş Günü Stüdyosu'ni başlatın.

4. Komut çubuğundan **Tester seçeneğinde İş Günü > Test Web Hizmeti'ni** seçin.

5. **Harici'yi**seçin ve adım 2'de indirdiğiniz Human_Resources WSDL dosyasını seçin.

    ![İş Günü Stüdyosu](./media/workday-inbound-tutorial/wdstudio1.png)

6. **Konum** alanını `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`gerçek örnek türünüzle "IMPL-CC" ve gerçek kiracı adınız ile "KIRACı" olarak değiştirin.

7. **İşlemi** **Get_Workers** ayarlayın

8.  İş günü kimlik bilgilerinizi ayarlamak için İstek/Yanıt bölmelerinin altındaki küçük **yapıyı yapılandır** bağlantısını tıklatın. **Kimlik Doğrulama'yı**denetleyin ve ardından İş Günü entegrasyon sistemi hesabınız için kullanıcı adı ve parolagirini girin. Kullanıcı adını ad\@kiracı olarak biçimlendirdiğinden ve **WS-Security Kullanıcı AdıToken** seçeneğini seçtiğinizden emin olun.

    ![İş Günü Stüdyosu](./media/workday-inbound-tutorial/wdstudio2.png)

9. **Tamam'ı**seçin.

10. **İstek** bölmesine, aşağıdaki XML'e yapıştırın ve Iş günü kiracınızdaki gerçek bir kullanıcının çalışan kimliğine **Employee_ID** ayarlayın. Ayıklamak istediğiniz özniteliğe sahip bir kullanıcı seçin.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="https://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
          <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```

11. Komutu yürütmek için **İstek Gönder'i** (yeşil ok) tıklatın. Başarılı olursa, yanıt **Yanıt** bölmesi içinde görünmelidir. Girdiğiniz kullanıcı kimliğinin verilerine sahip olduğundan emin olmak için yanıtı denetleyin, hata olmadığından emin olun.

12. Başarılı olursa, XML'i **Yanıt** bölmesinden kopyalayın ve XML dosyası olarak kaydedin.

13. Workday Studio'nun komut çubuğunda **Dosya > Dosyayı seçin...** ve kaydettiğiniz XML dosyasını açın. Bu eylem, Workday Studio XML düzenleyicisinde dosyayı açar.

    ![İş Günü Stüdyosu](./media/workday-inbound-tutorial/wdstudio3.png)

14. Dosya ağacında, **/env: Zarf > env: Gövde > wd:Get_Workers_Response > wd:Response_Data > wd: Kullanıcınızın** verilerini bulmak için işçi.

15. **wd altında: İşçi**, eklemek istediğiniz özniteliği bulmak ve seçin.

16. Seçili özniteliğiniz için **XPath** ifadesini Belge Yolu alanından kopyalayın.

17. Kopyalanan ifadeden **/env:Zarf/env:Gövde/wd:Get_Workers_Response/wd:Response_Data/** önekini kaldırın.

18. Kopyalanan ifadedeki son öğe bir düğümse (örnek: "/wd: Birth_Date"), ifadenin sonuna **/text()** ekleyin. Son öğe bir öznitelik (örnek: " tür")/@wd: ise bu gerekli değildir.

19. Sonuç gibi `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`bir şey olmalıdır . Bu değer, Azure portalına kopyalayabildiğiniz değerdir.

**Özel İş Günü kullanıcı özniteliğinizi sağlama yapılandırmanıza eklemek için:**

1. Azure [portalını](https://portal.azure.com)başlatın ve bu eğitimde daha önce açıklandığı gibi İş Günü sağlama uygulamanızın Sağlama bölümüne gidin.

2. **Proveverme Durumunu** **Kapalı**olarak ayarlayın ve **Kaydet'i**seçin. Bu adım, değişikliklerinizin yalnızca hazır olduğunuzda etkili olmasını sağlamaya yardımcı olur.

3. **Eşlemeler**altında, **İş Günü Çalışanlarını Şirket Içi Etkin Dizini Eşitle'yi** seçin (veya İş Günü Çalışanlarını Azure AD ile **Senkronize Edin).**

4. Bir sonraki ekranın altına gidin ve **gelişmiş seçenekleri göster'i**seçin.

5. **İş Günü için Öznitelik Listesini Edit'i**seçin.

    ![İş Günü Stüdyosu](./media/workday-inbound-tutorial/wdstudio_aad1.png)

6. Öznitelik listesinin altına, giriş alanlarının bulunduğu yere kaydırın.

7. **Ad**için, özniteliğiniz için bir görüntü adı girin.

8. **Tür**için, özniteliğe uygun şekilde karşılık gelen türü seçin **(String** en yaygınolanıdır).

9. **API İfadesi**için, Workday Studio'dan kopyaladığınız XPath ifadesini girin. Örnek: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. **Öznitelik Ekle'yi**seçin.

    ![İş Günü Stüdyosu](./media/workday-inbound-tutorial/wdstudio_aad2.png)

11. Yukarıda **Kaydet'i** ve ardından iletişim kutusuna **Evet'i** seçin. Hala açıksa Öznitelik Eşleme ekranını kapatın.

12. Ana **Sağlama** sekmesinde, **İş Günü Çalışanlarını Şirket Içi Etkin Dizini eşitle** 'yi (veya **Çalışanları Azure AD'ye Senkronize Etme)** seçeneğini yeniden seçin.

13. **Yeni eşleme ekle'yi**seçin.

14. Yeni özniteliğiniz artık **Kaynak öznitelik** listesinde görünmelidir.

15. Yeni özniteliğiniz için istediğiniz gibi bir eşleme ekleyin.

16. Tamamlandığında, **Provisioning Durumunu** **Tekrar A'ya** ayarlamayı ve kaydetmeyi unutmayın.

### <a name="exporting-and-importing-your-configuration"></a>Yapılandırmanızı dışarı ve içeri aktarma

Maddeye bakın [Verme ve alma sağlama yapılandırması](../app-provisioning/export-import-provisioning-configuration.md)

## <a name="managing-personal-data"></a>Kişisel verileri yönetme

Active Directory için İş Günü sağlama çözümü, şirket içi bir Windows sunucusuna bir sağlama aracısının yüklenmesini gerektirir ve bu aracı Windows Olay günlüğünde İş Günü'nden AD özniteliğinize bağlı olarak kişisel veriler içerebilecek günlükler oluşturur Eşleştirmeleri. Kullanıcı gizliliği yükümlülüklerine uymak için, olay günlüğünü temizlemek için bir Windows zamanlanmış görev ayarlayarak Olay günlüklerinde 48 saatten fazla veri tutulmadığından emin olabilirsiniz.

Azure AD sağlama hizmeti, GDPR sınıflandırmasının **veri işlemcisi** kategorisine girer. Bir veri işlemcisi boru hattı olarak, hizmet önemli ortaklara ve son tüketicilere veri işleme hizmetleri sağlar. Azure AD sağlama hizmeti kullanıcı verileri oluşturmaz ve hangi kişisel verilerin toplandığı ve nasıl kullanıldığı üzerinde bağımsız bir denetime sahip değildir. Azure AD sağlama hizmetinde veri alma, toplama, analiz ve raporlama, varolan kurumsal verilere dayanır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

Azure AD sağlama hizmeti, veri saklama yla ilgili olarak rapor oluşturmaz, analiz yapmaz veya 30 günden fazla öngörü sağlamaz. Bu nedenle, Azure AD sağlama hizmeti 30 günden fazla veri depolamaz, işlemez veya saklamaz. Bu tasarım, GDPR yönetmeliklerine, Microsoft gizlilik uyumluluk yönetmeliklerine ve Azure REKLAM veri saklama ilkeleriyle uyumludur.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
* [İş Günü ve Azure Etkin Dizini arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin](workday-tutorial.md)
* [Diğer SaaS uygulamalarını Azure Etkin Dizini ile nasıl entegre edebilirsiniz öğrenin](tutorial-list.md)
* [Sağlama yapılandırmalarını yönetmek için Microsoft Graph API'lerini nasıl kullanacağınızı öğrenin](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
