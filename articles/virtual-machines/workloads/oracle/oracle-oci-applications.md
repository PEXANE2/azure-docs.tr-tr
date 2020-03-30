---
title: Azure Sanal Makinelerde Oracle uygulamalarını dağıtmak için mimariler | Microsoft Dokümanlar
description: E-Business Suite, JD Edwards EnterpriseOne ve PeopleSoft gibi Oracle uygulamalarını Azure'da veya Oracle Cloud Infrastructure 'da (OCI) veritabanları bulunan Microsoft Azure sanal makinelerinde dağıtmak için uygulama mimarileri.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: ''
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/18/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: b183a4d4922c89f60ccb19b3e3e978216f33cc9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70100079"
---
# <a name="architectures-to-deploy-oracle-applications-on-azure"></a>Oracle uygulamalarını Azure'a dağıtacak mimariler

Microsoft ve Oracle, müşterilerin Oracle E-Business Suite, JD Edwards EnterpriseOne ve PeopleSoft gibi Oracle uygulamalarını bulutta dağıtmalarını sağlamak için birlikte çalıştı. Microsoft Azure ve Oracle Cloud Infrastructure (OCI) arasındaki önizleme [özel ağ bağlantısının](configure-azure-oci-networking.md) başlatılmasıyla, Oracle uygulamaları artık Azure veya OCI'daki arka uç veritabanlarıyla Azure'da dağıtılabilir. Oracle uygulamaları, kullanıcıların Azure Active Directory (Azure AD) kimlik bilgilerini kullanarak Oracle uygulamasında oturum açabilmeleri için tek oturum açma nızı ayarlamanıza olanak tanıyan Azure Active Directory ile de entegre edilebilir.

OCI, DBaaS, Exadata Cloud Service, Oracle RAC ve Infrastructure-as-a-Service (IaaS) dahil olmak üzere Oracle uygulamaları için birden fazla Oracle veritabanı seçeneği sunar. Şu anda, Özerk Veritabanı Oracle uygulamaları için desteklenen bir arka uç değildir.

Oracle uygulamalarını Azure'da dağıtmak için, yüksek kullanılabilir ve güvenli bir şekilde de dahil olmak üzere [birden çok seçenek](oracle-overview.md) vardır. Azure ayrıca, Oracle uygulamalarınızı tamamen Azure'da çalıştırmayı seçerseniz dağıtabileceğiniz [Oracle veritabanı VM görüntüleri](oracle-vm-solutions.md) de sunar.

Aşağıdaki bölümlerde, Oracle E-Business Suite, JD Edwards EnterpriseOne ve PeopleSoft'u çapraz bulut yapılandırmasında veya tamamen Azure'da dağıtmak için hem Microsoft hem de Oracle'ın mimari önerileri sıralanmaktadır. Microsoft ve Oracle bu uygulamaları test etmiş ve performansın bu uygulamalar için Oracle tarafından belirlenen standartları karşıladığını doğrulamıştır.

## <a name="architecture-considerations"></a>Mimari hususlar

Oracle uygulamaları, Azure'da aynı veya birden çok sanal makinede ve isteğe bağlı olarak OCI'da barındırılabilen birden çok hizmetten oluşur. 

Uygulama örnekleri özel veya genel uç noktalarla ayarlanabilir. Microsoft ve Oracle, uygulamanın yönetimi için ayrı bir alt ağda ortak bir IP adresi içeren bir *burç ana bilgisayar VM* ayarlamanızı önerir. Ardından, veritabanı katmanı da dahil olmak üzere diğer makinelere yalnızca özel IP adresleri atayın. 

Bir uygulamayı bulutlar arası mimaride ayarlarken, Azure sanal ağındaki IP adresi alanının OCI sanal bulut ağındaki özel IP adres alanıyla örtüşmediğinden emin olmak için planlama gereklidir.

Daha fazla güvenlik için, yalnızca belirli bağlantı noktalarında ve IP adreslerinde trafiğe izin verildiğinden emin olmak için ağ güvenlik gruplarını alt ağ düzeyinde ayarlayın. Örneğin, orta katmandaki makineler yalnızca sanal ağ içinden trafik almalıdır. Hiçbir dış trafik orta katman makineleri doğrudan ulaşmamalıdır.

Yüksek kullanılabilirlik için, aynı kullanılabilirlik kümesinde veya farklı kullanılabilirlik bölgelerinde farklı sunucuların gereksiz örneklerini ayarlayabilirsiniz. Kullanılabilirlik bölgeleri %99,99 çalışma süresi SLA elde etmenize olanak sağlarken, kullanılabilirlik kümeleri %99,95 çalışma süresi SLA bölge içinde elde etmenizi sağlar. Bu makalede gösterilen örnek mimariler iki kullanılabilirlik bölgesine dağıtılır.

Bir uygulamayı bulutlar arası bağlantı kullanarak dağıtırken, Azure ortamınızı şirket içi ağınıza bağlamak için varolan bir ExpressRoute devresini kullanmaya devam edebilirsiniz. Ancak, OCI'ye ara bağlantı için şirket içi ağınıza bağlanan devreden daha ayrı bir ExpressRoute devresine ihtiyacınız vardır.

## <a name="e-business-suite"></a>E-İş Paketi

Oracle E-Business Suite (EBS), Tedarik Zinciri Yönetimi (SCM) ve Müşteri İlişkileri Yönetimi (CRM) gibi bir uygulama paketidir. OCI'nin yönetilen veritabanı portföyünden yararlanmak için EBS, Microsoft Azure ve OCI arasındaki çapraz bulut ara bağlantısını kullanarak dağıtılabilir. Bu yapılandırmada, sunu ve uygulama katmanları aşağıdaki mimari diyagramda gösterildiği gibi Azure'da ve OCI'deki veritabanı katmanında çalışır (Şekil 1).

![E-Business Suite çapraz bulut mimarisi](media/oracle-oci-applications/ebs-arch-cross-cloud.png)

*Şekil 1: E-Business Suite çapraz bulut mimarisi* 

Bu mimaride, Azure'daki sanal ağ, oci'deki bir sanal bulut ağına çapraz bulut bağlantısı kullanılarak bağlanır. Uygulama katmanı Azure'da, veritabanı ise OCI'da ayarlanır. Yalnızca belirli bağlantı noktalarındaki belirli alt ağlardan gelen trafiğe izin vermek için her bileşenin ağ güvenlik gruplarıyla birlikte kendi alt ağına dağıtılması önerilir.

Mimari, bir bölgedeki iki kullanılabilirlik bölgesinde Oracle Data Guard kullanılarak yapılandırılan yüksek kullanılabilir Oracle veritabanlarıyla tamamen Azure'da dağıtım için uyarlanabilir. Aşağıdaki diyagram (Şekil 2) bu mimari desenin bir örneğidir:

![E-Business Suite Azure mimarisi](media/oracle-oci-applications/ebs-arch-azure.png)

*Şekil 2: E-Business Suite Azure mimarisi*

Aşağıdaki bölümlerde farklı bileşenler yüksek düzeyde açıklayınız.

[!INCLUDE [virtual-machines-oracle-applications-bastion](../../../../includes/virtual-machines-oracle-applications-bastion.md)]

### <a name="application-middle-tier"></a>Uygulama (orta) katmanı

Uygulama katmanı kendi alt ağına yalıtılır. Hata toleransı ve kolay yama yönetimi için ayarlanmış birden fazla sanal makine vardır. Bu VM'ler, Azure NetApp Files ve Ultra SSD'ler tarafından sunulan paylaşılan depolama alanı yla yedeklenebilir. Bu yapılandırma, kapalı kalma süresi olmadan düzeltme eteklerinin daha kolay dağıtılmasını sağlar. Uygulama katmanındaki makineler, ebs uygulama katmanına yönelik isteklerin bir hata nedeniyle çevrimdışı olsa bile işlenmesi için bir ortak yük dengeleyicitarafından ön lenmelidir.

### <a name="load-balancer"></a>Yük dengeleyici

Azure yük dengeleyicisi, yüksek kullanılabilirlik sağlamak için trafiği iş yükünüzün birden çok örneğine dağıtmanıza olanak tanır. Bu durumda, kullanıcıların WEB üzerinden EBS uygulamasına erişmesine izin verildiği için bir ortak yük dengeleyicisi ayarlanır. Yük dengeleyici, yükü orta katmandaki her iki makineye dağıtır. Daha fazla güvenlik için, siteden siteye VPN veya ExpressRoute ve ağ güvenlik gruplarını kullanarak yalnızca şirket ağınızdan sisteme erişen kullanıcılardan gelen trafiğe izin verin.

### <a name="database-tier"></a>Veritabanı katmanı

Bu katman Oracle veritabanını barındırur ve kendi alt ağına ayrılır. Oracle'a özel veritabanı bağlantı noktası 1521'deki veritabanı katmanına yalnızca uygulama katmanından gelen trafiğe izin veren ağ güvenlik grupları eklenmesi önerilir.

Microsoft ve Oracle yüksek kullanılabilirlik kurulumu önerir. Azure'da yüksek kullanılabilirlik, Oracle Data Guard ile iki kullanılabilirlik bölgesinde iki Oracle veritabanı ayarlayarak veya OCI'da Oracle Database Exadata Cloud Service kullanılarak elde edilebilir. Oracle Database Exadata Cloud Service kullanırken, veritabanınız iki alt ağda dağıtılır. Oracle Data Guard ile iki kullanılabilirlik etki alanında OCI'daki VM'lerde Oracle Veritabanı'nı da kurabilirsiniz.


### <a name="identity-tier"></a>Kimlik katmanı

Kimlik katmanı EBS Asserter VM içerir. EBS Asserter, Oracle Identity Cloud Service (IDCS) ve Azure AD'deki kimlikleri eşitlemenize olanak tanır. EBS SAML 2.0 veya OpenID Connect gibi tek oturum açma protokollerini desteklemediği için EBS Asserter gereklidir. EBS Asserter OpenID bağlantı belirteci (IDCS tarafından oluşturulan) tüketir, doğrular ve sonra EBS kullanıcı için bir oturum oluşturur. 

Bu mimarii IDCS tümleştirmesi gösterirken, Azure AD birleşik erişimi ve tek oturum açma özelliği Oracle Internet Directory veya Oracle Unified Directory ile Oracle Access Manager ile de etkinleştirilebilir. Daha fazla bilgi için, [IDCS Entegrasyonu ile Oracle EBS'yi dağıtma](https://cloud.oracle.com/iaas/whitepapers/deploy_ebusiness_suite_across_oci_azure_sso_idcs.pdf) veya [OAM Tümleştirmesi ile Oracle EBS'yi dağıtma hakkındaki](https://cloud.oracle.com/iaas/whitepapers/deploy_ebusiness_suite_across_oci_azure_sso_oam.pdf)teknik incelemelere bakın.

Yüksek kullanılabilirlik için, önünde bir yük dengeleyicisi bulunan birden çok kullanılabilirlik bölgesi ne zaman EBS Asserter'ın gereksiz sunucularını dağıtmanız önerilir.

Altyapınız kurulduktan sonra, Oracle tarafından sağlanan yükleme kılavuzunu izleyerek E-Business Suite yüklenebilir.

## <a name="jd-edwards-enterpriseone"></a>JD Edwards EnterpriseOne

Oracle'ın JD Edwards EnterpriseOne kapsamlı kurumsal kaynak planlama yazılımı entegre bir uygulama paketidir. Oracle veya SQL Server veritabanı arka ucuyla kurulabilen çok katmanlı bir uygulamadır. Bu bölümde, JD Edwards EnterpriseOne'ı OCI'da veya Azure'da bir Oracle veritabanı arka ucuyla dağıtmayla ilgili ayrıntılar açıklanmaktadır.

Önerilen aşağıdaki mimaride (Şekil 3), yönetim, sunu ve orta katmanlar Azure'daki sanal ağa dağıtılır. Veritabanı OCI'deki bir sanal bulut ağında dağıtılır.

E-Business Suite'te olduğu gibi, güvenli yönetim amaçları için isteğe bağlı bir burç katmanı ayarlayabilirsiniz. Uygulama ve veritabanı örneklerine erişmek için sıçrama sunucusu olarak burç VM ana bilgisayarını kullanın.

![JD Edwards EnterpriseOne çapraz bulut mimarisi](media/oracle-oci-applications/jdedwards-arch-cross-cloud.png)

*Şekil 3: JD Edwards EnterpriseOne çapraz bulut mimarisi*

Bu mimaride, Azure'daki sanal ağ oci'deki sanal bulut ağına çapraz bulut bağlantısı kullanılarak bağlanır. Uygulama katmanı Azure'da, veritabanı ise OCI'da ayarlanır. Yalnızca belirli bağlantı noktalarındaki belirli alt ağlardan gelen trafiğe izin vermek için her bileşenin ağ güvenlik gruplarıyla birlikte kendi alt ağına dağıtılması önerilir.

Mimari, bir bölgedeki iki kullanılabilirlik bölgesinde Oracle Data Guard kullanılarak yapılandırılan yüksek kullanılabilir Oracle veritabanlarıyla tamamen Azure'da dağıtım için uyarlanabilir. Aşağıdaki diyagram (Şekil 4) bu mimari desenin bir örneğidir:

![JD Edwards EnterpriseOne Azure mimarisi](media/oracle-oci-applications/jdedwards-arch-azure.png)

*Şekil 4: JD Edwards EnterpriseOne Azure mimarisi*

Aşağıdaki bölümlerde farklı bileşenler yüksek düzeyde açıklayınız.

[!INCLUDE [virtual-machines-oracle-applications-bastion](../../../../includes/virtual-machines-oracle-applications-bastion.md)]

### <a name="administrative-tier"></a>İdari katman

Adından da anlaşılacağı gibi, bu katman yönetim görevleri için kullanılır. Yönetim katmanı için ayrı bir alt ağ atabilirsiniz. Bu katmandaki hizmetler ve sunucular öncelikle uygulamanın yüklenmesi ve yönetilmesi için kullanılır. Bu nedenle, bu sunucuların tek örnekleri yeterlidir. Uygulamanızın yüksek kullanılabilirliği için gereksiz örnekler gerekmez.

Bu katmanın bileşenleri aşağıdaki gibidir:
    
 - **Sağlama sunucusu** - Bu sunucu, uygulamanın farklı bileşenlerinin uçtan uca dağıtımı için kullanılır. Bağlantı noktası 22 üzerinden veritabanı katmanındaki örnekler de dahil olmak üzere diğer katmanlarda ki örneklerle iletişim kurar. JD Edwards EnterpriseOne için Server Manager Konsolu'nu barındıran konsol.
 - **Dağıtım sunucusu** - Bu sunucu öncelikle JD Edwards EnterpriseOne kurulumu için gereklidir. Yükleme işlemi sırasında, bu sunucu gerekli dosyalar ve yükleme paketleri için merkezi depo görevi görür. Yazılım, bu sunucudan diğer sunuculara ve istemcilere dağıtılır veya dağıtılır.
 - **Geliştirme istemcisi** - Bu sunucu, yerel uygulamaların yanı sıra bir web tarayıcısında çalışan bileşenler içerir.

### <a name="presentation-tier"></a>Sunum katmanı

Bu katman, Uygulama Arabirimi Hizmetleri (AIS), Uygulama Geliştirme Çerçevesi (ADF) ve Java Uygulama Sunucuları (JAS) gibi çeşitli bileşenleri içerir. Bu katmandaki sunucular orta katmandaki sunucularla iletişim kurar. Bunlar, trafiğin alındığı bağlantı noktası numarası ve URL'sine göre trafiği gerekli sunucuya gönderen bir yük dengeleyicisi tarafından ön plana alınır. Yüksek kullanılabilirlik için her sunucu türünden birden çok örnek dağıtmanız önerilir.

Bu katmandaki bileşenler şunlardır:
    
- **Uygulama Arayüz Hizmetleri (AIS)** - AIS sunucusu JD Edwards EnterpriseOne mobil kurumsal uygulamalar ve JD Edwards EnterpriseOne arasında iletişim arayüzü sağlar.
- **Java Application Server (JAS)** - JAS yük dengeleyicisinden istekalır ve karmaşık görevleri yürütmek için orta katmana geçirir. JAS basit iş mantığı yürütmek için yeteneğine sahiptir.
- **BI Publisher Server (BIP)** - Bu sunucu, JD Edwards EnterpriseOne uygulaması tarafından toplanan verilere dayalı raporlar sunar. Raporun verileri farklı şablonlara göre nasıl sunduğunu tasarlayabilir ve denetleyebilirsiniz.
- **Business Services Server (BSS)** - BSS, diğer Oracle uygulamalarıyla bilgi alışverişi ve birlikte çalışabilirlik sağlar.
- **Gerçek Zamanlı Olaylar Sunucusu (RTE)** - RTE Server, JDE EnterpriseOne sisteminde gerçekleşen işlemlerle ilgili dış sistemlere bildirimler ayarlamanızı sağlar. Bir abone modeli kullanır ve üçüncü taraf sistemlerinin olaylara abone olmasını sağlar. Bakiye isteklerini her iki RTE sunucusuna yüklemek için sunucuların kümede olduğundan emin olun.
- **Uygulama Geliştirme Çerçevesi (ADF) Server** - ADF sunucusu Oracle ADF ile geliştirilen JD Edwards EnterpriseOne uygulamalarını çalıştırmak için kullanılır. Bu, ADF çalışma süresine sahip bir Oracle WebLogic sunucusunda dağıtılır.

### <a name="middle-tier"></a>Orta katman

Orta katman, mantık sunucusu ve toplu iş sunucusu içerir. Bu durumda, her iki sunucu da aynı sanal makineye yüklenir. Ancak, üretim senaryoları için, mantık sunucusu ve toplu sunucuyu ayrı sunucularda dağıtmanız önerilir. Daha yüksek kullanılabilirlik için iki kullanılabilirlik bölgesi arasında orta katmanda birden çok sunucu dağıtılır. Bir Azure yük dengeleyicisi oluşturulmalı ve bu sunucular, her iki sunucunun da etkin olduğundan ve istekleri işlemeye yönelik olduğundan emin olmak için arka uç havuzuna yerleştirilmelidir.

Orta katmandaki sunucular, sunu katmanındaki sunuculardan ve yalnızca genel yük bakiyesinden istek alır. Ağ güvenliği grubu kuralları, sunu katmanı alt ağı ve yük dengeleyicisi dışındaki herhangi bir adresten gelen trafiği reddetmek için ayarlanmalıdır. Yönetim amacıyla burç ana kuruluşundan 22. Orta katmandaki VM'ler arasındaki bakiye isteklerini yüklemek için ortak yük dengeleyicisini kullanabilirsiniz.

Aşağıdaki iki bileşen orta katmandadır:

- **Mantık sunucusu** - İş mantığını veya iş fonksiyonlarını içerir.
- **Toplu sunucu** - Toplu işlem için kullanılır

[!INCLUDE [virtual-machines-oracle-applications-database](../../../../includes/virtual-machines-oracle-applications-database.md)]

[!INCLUDE [virtual-machines-oracle-applications-identity](../../../../includes/virtual-machines-oracle-applications-identity.md)]

## <a name="peoplesoft"></a>Peoplesoft

Oracle'ın PeopleSoft uygulama paketi insan kaynakları ve finansal yönetim için yazılım içerir. Uygulama paketi çok katmanlıdır ve uygulamalar arasında insan kaynakları yönetim sistemleri (HRMS), müşteri ilişkileri yönetimi (CRM), finans ve tedarik zinciri yönetimi (FSCM) ve kurumsal performans yönetimi (EPM) bulunmaktadır.

Yazılım paketinin her katmanının kendi alt ağına dağıtılması önerilir. Uygulama için arka uç veritabanı olarak bir Oracle veritabanı veya Microsoft SQL Server gereklidir. Bu bölümde, PeopleSoft'u Oracle veritabanı arka ucuyla dağıtma yla ilgili ayrıntılar açıklanmıştır.

Aşağıda, PeopleSoft uygulama paketini bulutlar arası mimaride dağıtmak için kanonik bir mimari ve (Şekil 5).

![PeopleSoft çapraz bulut mimarisi](media/oracle-oci-applications/peoplesoft-arch-cross-cloud.png)

*Şekil 5: PeopleSoft çapraz bulut mimarisi*

Bu örnek mimaride, Azure'daki sanal ağ OCI'daki sanal bulut ağına çapraz bulut bağlantısı kullanılarak bağlanır. Uygulama katmanı Azure'da, veritabanı ise OCI'da ayarlanır. Yalnızca belirli bağlantı noktalarındaki belirli alt ağlardan gelen trafiğe izin vermek için her bileşenin ağ güvenlik gruplarıyla birlikte kendi alt ağına dağıtılması önerilir.

Mimari, bir bölgedeki iki kullanılabilirlik bölgesinde Oracle Data Guard kullanılarak yapılandırılan yüksek kullanılabilir Oracle veritabanlarıyla tamamen Azure'da dağıtım için uyarlanabilir. Aşağıdaki diyagram (Şekil 6) bu mimari desenin bir örneğidir:

![PeopleSoft Azure mimarisi](media/oracle-oci-applications/peoplesoft-arch-azure.png)

*Şekil 6: PeopleSoft Azure mimarisi*

Aşağıdaki bölümlerde farklı bileşenler yüksek düzeyde açıklayınız.

[!INCLUDE [virtual-machines-oracle-applications-bastion](../../../../includes/virtual-machines-oracle-applications-bastion.md)]

### <a name="application-tier"></a>Uygulama katmanı

Uygulama katmanı, PeopleSoft uygulama sunucuları, PeopleSoft web sunucuları, elastik arama ve PeopleSoft İşlem Zamanlayıcısı örneklerini içerir. Azure yük dengeleyicisi, uygulama katmanındaki uygun sunucuya yönlendirilen kullanıcılardan gelen istekleri kabul etmek üzere ayarlanmıştır.

Yüksek kullanılabilirlik için, uygulama katmanındaki her sunucunun gereksiz örneklerini farklı kullanılabilirlik bölgeleri arasında ayarlamayı düşünün. Azure yük dengeleyicisi, her isteği doğru sunucuya yönlendirmek için birden çok arka uç havuzuyla ayarlanabilir.

### <a name="peopletools-client"></a>PeopleTools İstemci

PeopleTools İstemci geliştirme, geçiş ve yükseltme gibi yönetim etkinlikleri gerçekleştirmek için kullanılır. PeopleTools İstemci uygulamanızın yüksek kullanılabilirlik elde etmek için gerekli olmadığından, PeopleTools Client gereksiz sunucular gerekli değildir.

[!INCLUDE [virtual-machines-oracle-applications-database](../../../../includes/virtual-machines-oracle-applications-database.md)]

[!INCLUDE [virtual-machines-oracle-applications-identity](../../../../includes/virtual-machines-oracle-applications-identity.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure'da Oracle uygulamaları ayarlamak ve OCI ile çapraz bulut bağlantısı kurmak için [Terraform komut dosyalarını](https://github.com/microsoft/azure-oracle) kullanın.

OCI hakkında daha fazla bilgi ve teknik incelemeler için [Oracle Cloud](https://docs.cloud.oracle.com/iaas/Content/home.htm) belgelerine bakın.
