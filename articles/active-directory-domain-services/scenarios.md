---
title: Azure AD Etki Alanı Hizmetleri için yaygın dağıtım senaryoları | Microsoft Dokümanlar
description: Değer sağlamak ve iş gereksinimlerini karşılamak için Azure Active Directory Domain Services'in yaygın senaryoları ve kullanım örnekleri hakkında bilgi edinin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: ac67ef64ca4850c6e805b5314ace856114d889a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77917239"
---
# <a name="common-use-cases-and-scenarios-for-azure-active-directory-domain-services"></a>Azure Etkin Dizin Etki Alanı Hizmetleri için yaygın kullanım örnekleri ve senaryolar

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), etki alanı birleştirme, grup ilkesi, hafif dizin erişim protokolü (LDAP) ve Kerberos / NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Azure AD DS, kullanıcıların mevcut kimlik bilgilerini kullanarak oturum açmalarını mümkün kılan mevcut Azure AD kiracınızla tümleşir. Bu etki alanı hizmetlerini, şirket içi kaynakların Azure'a daha sorunsuz bir şekilde kaldırılmasını ve kaydırmasını sağlayan bulutta etki alanı denetleyicilerini dağıtmaya, yönetmeye ve düzeltmeye gerek kalmadan kullanırsınız.

Bu makalede, Azure AD DS'nin değer sağladığı ve bu gereksinimleri karşıladığı bazı yaygın iş senaryoları özetlenir.

## <a name="secure-administration-of-azure-virtual-machines"></a>Azure sanal makinelerin güvenli yönetimi

Tek bir AD kimlik bilgileri kümesi kullanmanıza izin vermek için Azure sanal makineleri (VM' ler) Azure AD DS yönetilen bir etki alanına katılabilir. Bu yaklaşım, her VM'de yerel yönetici hesapları nın veya ortamlar arasındaki ayrı hesaplar ve parolaların korunması gibi kimlik bilgisi yönetimi sorunlarını azaltır.

Azure AD DS yönetilen etki alanına katılan VM'ler grup ilkesi kullanılarak da yönetilebilir ve güvenli hale alınabilir. Kurumsal güvenlik yönergelerine uygun olarak kilitlemek için VM'lere gerekli güvenlik taban çizgileri uygulanabilir. Örneğin, VM'de başlatılabilen uygulama türlerini kısıtlamak için grup ilkesi yönetimi yeteneklerini kullanabilirsiniz.

![Azure sanal makinelerin ini kolaylaştırılmış yönetimi](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Ortak bir örnek senaryoya bakalım. Sunucular ve diğer altyapı ömrü sona ererken, Contoso şu anda şirket içinde barındırılan uygulamaları buluta taşımak istiyor. Mevcut BT standartları, kurumsal uygulamaları barındıran sunucuların grup ilkesi kullanılarak etki alanına katılmasını ve yönetilmesini zorunlu kılmaktadır. Contoso'nun BT yöneticisi, kullanıcıların kurumsal kimlik bilgilerini kullanarak oturum açabilmeleri için yönetimi kolaylaştırmak için Azure'da dağıtılan VM'lere katılmayı tercih eder. Etki alanı birleştirildiğinde, VM'ler grup ilkesi nesneleri (GPO' lar) kullanılarak gerekli güvenlik taban çizgilerine uyacak şekilde de yapılandırılabilir. Contoso, Azure'da kendi etki alanı denetleyicilerini dağıtmamayı, izlememeyi ve yönetmemeyi tercih eder.

Azure AD DS, bu kullanım örneği için mükemmel bir uyum sağlar. Azure AD DS yönetilen etki alanı, etki alanına katılmanızı, tek bir kimlik bilgileri kümesi kullanmanıza ve grup ilkesi uygulamanıza olanak tanır. Yönetilen bir etki alanı olarak, etki alanı denetleyicilerini kendiniz yapılandırmanız ve korumanız gerekmemektedir.

### <a name="deployment-notes"></a>Dağıtım notları

Aşağıdaki dağıtım konuları bu örnek kullanım örneği için geçerlidir:

* Azure AD DS yönetilen etki alanları varsayılan olarak tek, düz Kuruluş Birimi (OU) yapısını kullanır. Etki alanına katılan tüm VM'ler tek bir OU'dadır. İstenirse, özel OS B'ler oluşturabilirsiniz.
* Azure AD DS, kullanıcılar ve bilgisayar kapsayıcıları için yerleşik bir GPO kullanır. Ek denetim için, özel GPO'lar oluşturabilir ve bunları özel İş'lere göre hedefleyebilirsiniz.
* Azure AD DS, temel AD bilgisayar nesnesi şemasını destekler. Bilgisayar nesnesinin şemasını genişletemezsiniz.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-bind-authentication"></a>LDAP bağlama kimlik doğrulaması kullanan şirket içi kaldırma ve kaydırma uygulamaları

Örnek bir senaryo olarak Contoso'nun yıllar önce bir ISV'den satın alınan şirket içi bir uygulaması vardır. Uygulama şu anda ISV tarafından bakım modunda ve uygulama değişiklikleri isteyen son derece pahalıdır. Bu uygulama, bir web formu kullanarak kullanıcı kimlik bilgilerini toplayan ve daha sonra şirket içi AD DS ortamına bir LDAP bağlama gerçekleştirerek kullanıcıların kimlik doğrulamasını sağlayan bir web tabanlı ön uç vardır.

![LDAP bağlama](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso bu uygulamayı Azure'a geçirmek ister. Uygulama, hiçbir değişiklik gerekmeden olduğu gibi çalışmaya devam etmelidir. Ayrıca, kullanıcılar mevcut kurumsal kimlik bilgilerini kullanarak ve ek eğitim olmadan kimlik doğrulaması yapabilmelidir. Uygulamanın çalıştığı son kullanıcılariçin saydam olmalıdır.

Bu senaryo için Azure AD DS, uygulamaların kimlik doğrulama işleminin bir parçası olarak LDAP bağlamaları gerçekleştirmesini sağlar. Eski şirket içi uygulamalar Azure'a geçiş yapabilir ve yapılandırma veya kullanıcı deneyiminde herhangi bir değişiklik yapmadan kullanıcıların kimliğini sorunsuz bir şekilde doğrulamaya devam edebilir.

### <a name="deployment-notes"></a>Dağıtım notları

Aşağıdaki dağıtım konuları bu örnek kullanım örneği için geçerlidir:

* Uygulamanın dizini değiştirmesi/yazması gerekmediğinden emin olun. LDAP yazma erişimi Azure AD DS yönetilen etki alanı desteklenmez.
* Parolaları doğrudan Azure AD DS yönetilen bir etki alanına göre değiştiremezsiniz. Son kullanıcılar parolalarını Azure AD'nin self servis parola değiştirme mekanizmasını kullanarak veya şirket içi dizine göre değiştirebilir. Bu değişiklikler daha sonra otomatik olarak senkronize edilir ve Azure AD DS yönetilen etki alanında kullanılabilir.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-read-to-access-the-directory"></a>LDAP kullanan şirket içi kaldırma ve kaydırma uygulamaları dizine erişmek için okunur

Önceki örnek senaryoda olduğu gibi, Contoso'nun da neredeyse on yıl önce geliştirilen şirket içi iş yeri (LOB) uygulaması olduğunu varsayalım. Bu uygulama dizin farkında dır ve AD DS kullanıcıları hakkında bilgi / öznitelikleri okumak için LDAP kullanmak için tasarlanmıştır. Uygulama öznitelikleri değiştirmez veya dizine başka bir şekilde yazmaz.

Contoso bu uygulamayı Azure'a geçirmek ve şu anda bu uygulamayı barındıran eskiyen şirket içi donanımı emekliye geçirmek istiyor. Uygulama, REST tabanlı Microsoft Graph API'si gibi modern dizin API'lerini kullanmak için yeniden yazılamaz. Kodu değiştirmeden veya uygulamayı yeniden yazmadan, uygulamanın bulutta çalıştırılmak üzere geçirilebileceği bir kaldırma ve kaydırma seçeneği istenir.

Bu senaryoya yardımcı olmak için Azure AD DS, uygulamaların ldap okumalarını, ihtiyaç duyduğu öznitelik bilgilerini almak için yönetilen etki alanına karşı gerçekleştirmesine olanak tanır. Uygulamanın yeniden yazılması gerekmez, bu nedenle Azure'a geçiş, kullanıcıların çalıştığı yerde bir değişiklik olduğunu fark etmeden uygulamayı kullanmaya devam etmesini sağlar.

### <a name="deployment-notes"></a>Dağıtım notları

Aşağıdaki dağıtım konuları bu örnek kullanım örneği için geçerlidir:

* Uygulamanın dizini değiştirmesi/yazması gerekmediğinden emin olun. LDAP yazma erişimi Azure AD DS yönetilen etki alanı desteklenmez.
* Uygulamanın özel/genişletilmiş Active Directory şemasına gerek olmadığından emin olun. Şema uzantıları Azure AD DS'de desteklenmez.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure"></a>Şirket içi bir hizmeti veya daemon uygulamasını Azure'a geçirme

Bazı uygulamalar, katmanlardan birinin veritabanı gibi bir arka uç katmanına kimlik doğrulaması çağrıları gerçekleştirmesi gereken birden çok katman içerir. AD hizmet hesapları genellikle bu senaryolarda kullanılır. Azure AD DS, uygulamaları Azure'a taşıyıp kaydırdığınızda, hizmet hesaplarını aynı şekilde kullanmaya devam etmenizi sağlar. Şirket içi dizininizden Azure AD'ye senkronize edilen aynı hizmet hesabını kullanmayı veya özel bir OU oluşturmayı ve ardından bu OU'da ayrı bir hizmet hesabı oluşturmayı seçebilirsiniz. Her iki yaklaşımla da, uygulamalar diğer katmanlara ve hizmetlere kimlik doğrulamalı aramalar yapmak için aynı şekilde çalışmaya devam eder.

![WIA kullanarak servis hesabı](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Bu örnek senaryoda, Contoso'nun web ön ucu, bir SQL sunucusu ve arka uç FTP sunucusu içeren özel olarak oluşturulmuş bir yazılım kasası uygulaması vardır. Hizmet hesaplarını kullanarak Windows ile tümleşik kimlik doğrulaması, web ön ucunu FTP sunucusuna doğrular. Web ön uç bir hizmet hesabı olarak çalışacak şekilde ayarlanır. Arka uç sunucusu, web ön uç için hizmet hesabından erişim yetkisi vermek üzere yapılandırılmıştır. Contoso, bu uygulamayı Azure'a taşımak için bulutta kendi etki alanı denetleyiciSi VM'lerini dağıtmak ve yönetmek istemiyor.

Bu senaryo için, web ön ucunu, SQL sunucusunu ve FTP sunucusunu barındıran sunucular Azure VM'lerine geçirilebilir ve Azure AD DS yönetilen etki alanına katılabilir. Daha sonra VM'ler, Azure AD Connect kullanılarak Azure AD aracılığıyla senkronize edilen uygulamanın kimlik doğrulama amaçları için şirket içi dizinde aynı hizmet hesabını kullanabilir.

### <a name="deployment-notes"></a>Dağıtım notları

Aşağıdaki dağıtım konuları bu örnek kullanım örneği için geçerlidir:

* Uygulamaların kimlik doğrulaması için bir kullanıcı adı ve parola kullandığından emin olun. Sertifika veya akıllı kart tabanlı kimlik doğrulaması Azure AD DS tarafından desteklenmez.
* Parolaları doğrudan Azure AD DS yönetilen bir etki alanına göre değiştiremezsiniz. Son kullanıcılar parolalarını Azure AD'nin self servis parola değiştirme mekanizmasını kullanarak veya şirket içi dizine göre değiştirebilir. Bu değişiklikler daha sonra otomatik olarak senkronize edilir ve Azure AD DS yönetilen etki alanında kullanılabilir.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Azure'da Windows Server uzak masaüstü hizmetleri dağıtımları

Azure'da dağıtılan uzak masaüstü sunucularına yönetilen etki alanı hizmetleri sağlamak için Azure AD DS'yi kullanabilirsiniz. Bu dağıtım senaryosu hakkında daha fazla bilgi için [Azure AD Etki Alanı Hizmetlerini RDS dağıtımınızla nasıl tümleştirilene][windows-rds]bakın.

## <a name="domain-joined-hdinsight-clusters"></a>Etki alanı birleştirilmiş HDInsight kümeleri

Apache Ranger etkinleştirilmiş bir Azure AD DS yönetilen etki alanına birleştirilmiş bir Azure HDInsight kümesi ayarlayabilirsiniz. Apache Ranger aracılığıyla Hive ilkeleri oluşturabilir ve uygulayabilir ve veri bilimciler gibi kullanıcıların Excel veya Tableau gibi ODBC tabanlı araçları kullanarak Hive'a bağlanmasına izin verebilirsiniz. Etki alanına katılan HDInsight'a HBase, Spark ve Storm gibi diğer iş yüklerini eklemek için çalışmaya devam ediyoruz.

Bu dağıtım senaryosu hakkında daha fazla bilgi için [etki alanına birleştirilmiş HDInsight kümelerini nasıl yapılandırılabildiğini][hdinsight] öğrenin

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırma][tutorial-create-instance]

<!-- INTERNAL LINKS -->
[hdinsight]: ../hdinsight/domain-joined/apache-domain-joined-configure.md
[tutorial-create-instance]: tutorial-create-instance.md

<!-- EXTERNAL LINKS -->
[windows-rds]: /windows-server/remote/remote-desktop-services/rds-azure-adds
