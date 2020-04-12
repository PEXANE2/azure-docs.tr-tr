---
title: ARM ile ILB ASE oluşturma
description: Azure Kaynak Yöneticisi şablonlarını kullanarak dahili yük dengeleyicisi (ILB ASE) ile Bir Uygulama Hizmeti ortamını nasıl oluşturabilirsiniz öğrenin. Uygulamalarınızı internetten tamamen yalıtın.
author: ccompy
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.topic: quickstart
ms.date: 08/05/2019
ms.author: ccompy
ms.custom: mvc, seodec18
ms.openlocfilehash: b7fa447e8564fcbf77702f1d3d474cceb48705c5
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114631"
---
# <a name="create-and-use-an-internal-load-balancer-app-service-environment"></a>Dahili Yük Dengeleyici Uygulaması Servis Ortamı Oluşturma ve Kullanma 

Azure Uygulama Hizmet Ortamı, Azure Uygulama Hizmeti'nin bir Azure sanal ağında (VNet) bir alt ağa dağıtımıdır. Bir App Service Ortamı (ASE) iki şekilde dağıtılabilir: 

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- Genellikle iç uç nokta bir iç yük dengeleyici (ILB) olduğu için ILB ASE olarak adlandırılan durumda, bir iç IP adresi üzerindeki VIP ile. 

Bu makale bir ILB ASE oluşturma işlemini gösterir. ASE’ye genel bakış için bkz. [App Service Ortamlarına giriş][Intro]. Dış ASE oluşturma hakkında bilgi almak için bkz. [Dış ASE Oluşturma][MakeExternalASE].

## <a name="overview"></a>Genel Bakış 

Bir ASE’yi İnternet’ten erişilebilen bir uç nokta ile ya da sanal ağınızdaki bir IP adresi ile dağıtabilirsiniz. IP adresini bir sanal ağ adresine ayarlamak için, ASE’nin ILB ile dağıtılması gerekir. ASE'nizi bir ILB ile dağıttığınızda, ASE'nizin adını sağlamanız gerekir. ASE'nizin adı, ASE'nizdeki uygulamaların etki alanı sonekinde kullanılır.  ILB ASE'nizin etki alanı &lt;soneki ASE adıdır&gt;.appserviceenvironment.net. ILB ASE'de yapılan uygulamalar genel DNS'ye konulmaz. 

ILB ASE'nin önceki sürümlerinde, https bağlantıları için bir etki alanı soneki ve varsayılan bir sertifika sağlamanız gerekiyordu. Etki alanı soneki artık ILB ASE oluşturmada toplanmaz ve varsayılan sertifika da artık toplanmaz. Şimdi bir ILB ASE oluşturduğunuzda, varsayılan sertifika Microsoft tarafından sağlanır ve tarayıcı tarafından güvenilir. ASE'nizdeki uygulamalarda özel alan adları belirleyebilir ve bu özel alan adları üzerinde sertifikalar ayarlayabilirsiniz. 

Bir ILB ASE ile şunları yapabilirsiniz:

-   Siteden siteye veya ExpressRoute üzerinden erişebildiğiniz intranet uygulamalarını bulutta güvenli bir şekilde barındırın.
-   Uygulamaları WAF cihazıyla koruyun
-   Genel DNS sunucularında listelenmeyen uygulamaları bulutta barındırma.
-   Ön uç uygulamalarınızın güvenli bir şekilde tümleştirilebileceği, İnternet’ten yalıtılmış arka uç uygulamaları oluşturma.

### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler ###

ILB ASE’yi kullanırken bazı işlemleri yapamazsınız:

-   IP tabanlı SSL kullanma.
-   Belirli uygulamalara IP adresleri atama.
-   Azure portalı üzerinden bir uygulama ile sertifika satın alma ve kullanma. Sertifikaları doğrudan bir sertifika yetkilisinden alabilir ve uygulamalarınızla kullanabilirsiniz. Sertifikaları Azure portalı üzerinden alamazsınız.

## <a name="create-an-ilb-ase"></a>ILB ASE oluşturma ##

ILB ASE oluşturmak için:

1. Azure portalında > kaynak**Web** > **Uygulaması Hizmet Ortamı** **Oluştur'u**seçin.

2. Aboneliğinizi seçin.

3. Kaynak grubunu seçin veya oluşturun.

4. Uygulama Hizmet Ortamınızın adını girin.

5. İç sanal IP türünü seçin.

    ![ASE oluşturma](media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase.png)

> [!NOTE]
> App Service Environment adı en fazla 37 karakter olmalıdır.

6. Ağ'ı Seçin

7. Sanal Ağ seçin veya oluşturun. Burada yeni bir VNet oluşturursanız, 192.168.250.0/23 adres aralığı ile tanımlanır. Farklı bir adres aralığına veya ASE'den farklı bir kaynak grubunda bir VNet oluşturmak için Azure Sanal Ağ oluşturma portalını kullanın. 

8. Boş bir alt ağ seçin veya oluşturun. Bir alt ağ seçmek istiyorsanız, boş olmalı ve devralınmamalıdır. ASE oluşturulduktan sonra alt ağ boyutu değiştirilemez. 256 adres içeren ve en büyük boyutlu ASE’yi işleyebilen ve ölçeklendirme ihtiyaçlarını karşılayabilen `/24` dosya boyutu önerilir. 

    ![ASE ağ][1]

7. **Gözden Geçir'i ve Oluştur'u** seçin ve ardından **Oluştur'u**seçin.


## <a name="create-an-app-in-an-ilb-ase"></a>ILB ASE'de uygulama oluşturma ##

ILB ASE'de uygulama oluşturma işlemi, normalde bir ASE’de uygulama oluşturma işlemiyle aynıdır.

1. Azure portalında > kaynak**Web Web** > **Uygulaması** **Oluştur'u**seçin.

1. Uygulamanın adını girin.

1. Aboneliği seçin.

1. Kaynak grubunu seçin veya oluşturun.

1. Yayımla, Çalışma Zamanı Yığını ve İşletim Sisteminizi seçin.

1. Konumun varolan bir ILB ASE olduğu bir konum seçin.  Ayrıca, yalıtılmış bir Uygulama Hizmeti planı seçerek uygulama oluşturma sırasında yeni bir ASE oluşturabilirsiniz. Yeni bir ASE oluşturmak istiyorsanız, ASE'nin oluşturulmasını istediğiniz bölgeyi seçin.

1. Bir App Service planı seçin ya da oluşturun. 

1. **Gözden Geçir'i ve Oluştur'u** seçin ve hazır olduğunuzda **Oluştur'u** seçin.

### <a name="web-jobs-functions-and-the-ilb-ase"></a>Web işleri, İşlevler ve ILB ASE 

Hem İşlevler hem de web işleri ILB ASE’de desteklenir, ancak portalın bunlarla çalışabilmesi için SCM sitesine ağ erişiminiz olmalıdır.  Başka bir deyişle, tarayıcınız sanal ağ içinde veya sanal ağa bağlı bir konakta olmalıdır. ILB ASE'nizin *appserviceenvironment.net*bitmeyen bir etki alanı adı varsa, tarayıcınızın scm siteniz tarafından kullanılan HTTPS sertifikasına güvenmesi gerekir.

## <a name="dns-configuration"></a>DNS yapılandırması 

Dış VIP kullandığınızda DNS, Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. ILB ASE ile kullanılan etki alanı soneki ASE'nin adına bağlıdır. Etki alanı soneki * &lt;ASE adı&gt;.appserviceenvironment.net.* ILB'nizin IP adresi IP **adreslerinin**altındaki portaldadır. 

DNS'nizi yapılandırmak için:

- * &lt;ASE adı&gt;için bir bölge oluşturma .appserviceenvironment.net*
- iLB IP adresine * işaret eden o bölgede bir A kaydı oluşturmak
- iLB IP adresine @ işaret eden o bölgede bir A kaydı oluşturmak
- *ASE adında&gt;bir bölge oluşturmak .appserviceenvironment.net adlı scm &lt;*
- ILB IP adresine * işaret eden scm bölgesinde bir A kaydı oluşturmak

## <a name="publish-with-an-ilb-ase"></a>ILB ASE ile yayımlama

Oluşturulan her uygulama için iki uç nokta vardır. ILB ASE'de * &lt;uygulama adınız&gt;vardır.&lt; ILB ASE&gt; Etki alanı* ve * &lt;uygulama adı&gt;.scm.&lt; ILB ASE&gt;Etki Alanı*. 

SCM site adı sizi Azure portalı içinde **Gelişmiş portal** olarak adlandırılan Kudu konsoluna götürür. Kudu konsolunu kullanarak ortam değişkenlerini görüntüleyebilir, diski keşfedebilir, bir konsolu kullanabilir ve daha fazlasını yapabilirsiniz. Daha fazla bilgi için bkz. [Azure App Service için Kudu konsolu][Kudu]. 

GitHub ve Azure DevOps gibi İnternet tabanlı CI sistemleri, derleme aracısına İnternet’ten erişilebiliyorsa ve aracı ILB ASE ile aynı ağdaysa ILB ASE ile çalışmaya devam eder. Bu nedenle, Azure DevOps örneğinde derleme aracısı ILB ASE ile aynı sanal ağda (alt ağın farklı olması sorun yaratmaz) oluşturulursa Azure DevOps git’ten kod çekip ILB ASE’ye dağıtabilir. Kendi derleme aracınızı oluşturmak istemiyorsanız çekme modeli kullanan bir CI sistemi (Dropbox gibi) kullanmanız gerekir.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Bu etki alanı uygulamanın yayın profilinde ve uygulamanın portal bıçak **(Genel Bakış** > **Essentials** ve aynı zamanda **Özellikler)** görünür. Etki alanı soneki * &lt;ASE adı&gt;.appserviceenvironment.net*ile bir ILB ASE ve *mytest*adlı bir uygulama varsa, *mytest kullanın.&lt; FTP&gt;için ASE adı .appserviceenvironment.net* ve web dağıtımı için *mytest.scm.contoso.net.*

## <a name="configure-an-ilb-ase-with-a-waf-device"></a>Bir Waf cihazıyla ILB ASE'yi yapılandırma ##

Bir web uygulaması güvenlik duvarı (WAF) cihazını ILB ASE'nizle birleştirerek yalnızca internete istediğiniz uygulamaları ortaya çıkarabilir ve geri kalanını yalnızca VNet'ten erişebilirsiniz. Bu, diğer şeylerin yanı sıra güvenli çok katmanlı uygulamalar oluşturmanıza olanak tanır.

ILB ASE'nizi bir WAF aygıtıyla nasıl yapılandırılabildiğiniz hakkında daha fazla bilgi edinmek için, [Uygulama Hizmeti ortamınızla bir web uygulama güvenlik duvarını yapılandır'a][ASEWAF]bakın. Bu makalede, bir Barracuda sanal gerecinin ASE’nizle nasıl kullanılacağı gösterilir. Bir diğer seçenek ise Azure Application Gateway’in kullanılmasıdır. Application Gateway, arkasına yerleştirilmiş uygulamaların güvenliğini sağlamak için OWASP temel kurallarını kullanır. Application Gateway hakkında daha fazla bilgi için bkz. [Azure web uygulaması güvenlik duvarına giriş][AppGW].

## <a name="ilb-ases-made-before-may-2019"></a>Mayıs 2019'dan önce yapılan ILB'ler

Mayıs 2019'dan önce yapılan ILB'ler, ASE oluşturma sırasında etki alanı sonekini ayarlamanızı gerektiriyordu. Ayrıca, bu etki alanı sonekine dayalı bir varsayılan sertifika yüklemenizi de istediler. Ayrıca, eski bir ILB ASE ile Bu ILB ASE uygulamaları ile Kudu konsoluna tek oturum açamaz. DNS'yi eski bir ILB ASE için yapılandırırken, etki alanı nız sonekiyle eşleşen bir bölgede Joker A kaydını ayarlamanız gerekir. 

## <a name="get-started"></a>başlarken ##

* ASE’leri kullanmaya başlamak için bkz. [App Service ortamlarına giriş][Intro]. 

<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[customdomain]: ../app-service-web-tutorial-custom-domain.md
[linuxapp]: ../containers/app-service-linux-intro.md
