---
title: Application Gateway ile tümleştirme
description: Bu uçtan uca gezinmede bir Uygulama Ağ Geçidi ile Bir Uygulama Hizmet Ortamınızda bir uygulamayı nasıl entegre acağınızı öğrenin.
author: ccompy
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.topic: article
ms.date: 03/03/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: dfb6d72b3f8f61e1350101173ecec6134a614edf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687146"
---
# <a name="integrate-your-ilb-app-service-environment-with-the-azure-application-gateway"></a>ILB App Service Ortamınızı Azure Application Gateway ile Tümleştirme #

[Uygulama Hizmeti Ortamı,](./intro.md) Azure Uygulama Hizmeti'nin müşterinin Azure sanal ağının alt ağında bir dağıtımıdır. Uygulama erişimi için genel veya özel bir bitiş noktasıyla dağıtılabilir. Uygulama Hizmet Ortamının özel bir uç noktası (diğer bir şekilde dahili yük dengeleyicisi) ile dağıtılmasına ILB Uygulama Hizmet Ortamı denir.  

Web uygulaması güvenlik duvarları, SQL enjeksiyonlarını, Site arası Komut Dosyalarını, kötü amaçlı yazılım yüklemelerini ve uygulama DDoS'& diğer saldırıları engellemek için gelen web trafiğini inceleyerek web uygulamalarınızın güvenliğini sağlamaya yardımcı olur. Ayrıca, Veri Kaybı Önleme (DLP) için arka uç web sunucularından gelen yanıtları inceler. Azure pazar yerinden bir WAF aygıtı alabilirsiniz veya [Azure Uygulama Ağ Geçidi'ni][appgw]kullanabilirsiniz.

Azure Uygulama Ağ Geçidi, katman 7 yük dengeleme, SSL boşaltma ve web uygulaması güvenlik duvarı (WAF) koruması sağlayan sanal bir cihazdır. Genel bir IP adresinde dinleyebilir ve trafiği uygulama bitiş noktanıza yönlendirebilir. Aşağıdaki bilgiler, WAF tarafından yapılandırılmış bir uygulama ağ geçidinin ILB App Service Environment'daki bir uygulamayla nasıl tümleştirilebildiğini açıklar.  

Uygulama ağ geçidinin ILB App Service Environment ile entegrasyonu uygulama düzeyindedir. Uygulama ağ geçidini ILB App Service Environment'ınızla yapılandırdığınızda, bunu ILB App Service Environment'ınızdaki belirli uygulamalar için yaparsınız. Bu teknik, tek bir ILB App Service Environment'da güvenli çok kiracılı uygulamaların barındırılmasını sağlar.  

![ILB App Service Environment'daki uygulamaya işaret eden uygulama ağ geçidi][1]

Bu bölümde şunları yapacaksınız:

* Azure Uygulama Ağ Geçidi oluşturun.
* Uygulama Ağ Geçidini ILB App Service Environment'ınızdaki bir uygulamayı işaret etmek için yapılandırın.
* Özel alan adını onurlandıracak şekilde uygulamanızı yapılandırın.
* Uygulama ağ geçidinize işaret eden ortak DNS ana bilgisayar adını düzenleyin.

## <a name="prerequisites"></a>Ön koşullar

Uygulama Ağ Geçidinizi ILB App Service Environment ile entegre etmek için şunları yapmanız gerekir:

* ILB uygulama servis ortamı.
* ILB App Service Environment'da çalışan bir uygulama.
* ILB App Service Environment'da uygulamanızla birlikte kullanılacak bir internet routable alan adı.
* ILB App Service Environment'ınızın kullandığı ILB adresi. Bu bilgiler **Ayarlar** > **IP Adresleri**altında App Servis Ortamı portalında yer alıyor:

    ![ILB App Service Environment tarafından kullanılan IP adreslerinin örnek listesi][9]
    
* Daha sonra Uygulama Ağ Geçidinizi işaret etmek için kullanılan genel bir DNS adı. 

ILB Uygulama Hizmet Ortamı nın nasıl oluşturulabildiğini hakkında ayrıntılı bilgi için, [Bir ILB Uygulama Hizmet Ortamı Oluşturma ve Kullanma][ilbase]hakkında bilgi.

Bu makalede, Uygulama Hizmet Ortamının dağıtıldığı aynı Azure sanal ağında bir Uygulama Ağ Geçidi istediğinizi varsayar. Uygulama Ağ Geçidi'ni oluşturmaya başlamadan önce, ağ geçidini barındırmak için kullanacağınız bir alt ağ seçin veya oluşturun. 

GatewaySubnet adında olmayan bir alt ağ kullanmalısınız. Uygulama Ağ Geçidi'ni GatewaySubnet'e koyarsanız, daha sonra sanal ağ ağ geçidi oluşturamazsınız. 

Ayrıca, ILB App Service Environment'ınızın kullandığı alt ağa ağ geçidini koyamazsınız. App Service Environment bu alt ağda olabilecek tek şeydir.

## <a name="configuration-steps"></a>Yapılandırma adımları ##

1. Azure portalında, **Yeni** > **Ağ** > **Uygulama Ağ Geçidi'ne**gidin.

2. Temel **bilgiler** alanında:

   a. **Ad**için, Uygulama Ağ Geçidi'nin adını girin.

   b. **Tier**için **WAF'ı**seçin.

   c. **Abonelik**için, App Service Environment sanal ağın kullandığı aboneliği seçin.

   d. **Kaynak grubu**için kaynak grubu oluşturun veya seçin.

   e. **Konum**için, App Service Environment sanal ağının konumunu seçin.

   ![Yeni Uygulama Ağ Geçidi oluşturma temelleri][2]

3. **Ayarlar** alanında:

   a. **Sanal ağ**için, App Service Environment sanal ağı seçin.

   b. **Subnet**için, Uygulama Ağ Geçidi'nin dağıtılması gereken alt ağı seçin. GatewaySubnet'i kullanmayın, çünkü VPN ağ geçitlerinin oluşturulmasını engeller.

   c. **IP adresi türü için** **Genel'i**seçin.

   d. **Herkese Açık IP adresi**için herkese açık bir IP adresi seçin. Eğer yoksa, şimdi bir tane oluşturun.

   e. **Protokol**için **HTTP** veya **HTTPS'yi**seçin. HTTPS için yapılandırma yapacaksanız, bir PFX sertifikası sağlamanız gerekir.

   f. **Web uygulaması güvenlik duvarı**için, güvenlik duvarını etkinleştirebilir ve uygun gördüğünüz şekilde **Algılama** veya **Önleme** için ayarlayabilirsiniz.

   ![Yeni Uygulama Ağ Geçidi oluşturma ayarları][3]
    
4. **Özet** bölümünde, ayarları gözden geçirin ve **Tamam'ı**seçin. Uygulama Ağ Geçidinizin kurulumu tamamlaması 30 dakikadan biraz daha uzun sürebilir.  

5. Uygulama Ağ Geçidiniz kurulumu tamamladıktan sonra, Uygulama Ağ Geçidi portalınıza gidin. **Arka uç havuzu'ni**seçin. ILB App Service Environment için ILB adresini ekleyin.

   ![Arka uç havuzunı yapılandırma][4]

6. Arka uç havuzunuzu yapılandırma işlemi tamamlandıktan sonra, **Sağlık sondalarını**seçin. Uygulamanız için kullanmak istediğiniz alan adı için bir sistem durumu sondası oluşturun. 

   ![Sistem durumu araştırmalarını yapılandırma][5]
    
7. Sistem durumu sondalarınızı yapılandırma işlemi tamamlandıktan sonra **HTTP ayarlarını**seçin. Varolan ayarları edin, **Özel sonda yı kullanın'ı**seçin ve yapılandırdığınız sondayı seçin.

   ![HTTP ayarlarını yapılandırma][6]
    
8. Uygulama Ağ Geçidi'nin **Genel Bakış** bölümüne gidin ve Uygulama Ağ Geçidinizin kullandığı genel IP adresini kopyalayın. Bu IP adresini uygulama etki alanı adınız için bir kayıt olarak ayarlayın veya cname kaydında bu adresin DNS adını kullanın. Uygulama Ağ Geçidi'nin **Genel Bakış** bölümündeki bağlantıdan kopyalamak yerine herkese açık IP adresini seçmek ve genel IP adresinin Kullanıcı Arama Bilgisi'nden kopyalamak daha kolaydır. 

   ![Uygulama Ağ Geçidi portalı][7]

9. ILB App Service Environment'ınızda uygulamanızın özel alan adını ayarlayın. Portalda uygulamanıza gidin ve **Ayarlar**altında Özel **etki alanlarını**seçin.

   ![Uygulamada özel alan adı ayarlama][8]

Web uygulamanız için [özel alan adlarını ayarlama][custom-domain]makalesinde web uygulamalarınız için özel alan adları ayarlama hakkında bilgiler bulunmaktadır. Ancak ILB App Service Environment'daki bir uygulama için alan adı üzerinde herhangi bir doğrulama yoktur. Uygulama bitiş noktalarını yöneten DNS'ye sahip olduğunuz için, istediğinizher şeyi oraya koyabilirsiniz. Bu durumda eklediğiniz özel alan adının DNS'nizde olması gerekmez, ancak yine de uygulamanızla birlikte yapılandırılması gerekir. 

Kurulum tamamlandıktan ve DNS değişikliklerinizin yayılması için kısa bir süre izin verdikten sonra, oluşturduğunuz özel etki alanı adını kullanarak uygulamanıza erişebilirsiniz. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
