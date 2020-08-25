---
title: Azure VMware çözümünde Web uygulamalarınızı korumak için Azure Application Gateway kullanma
description: Azure VMware çözümünde çalışan Web uygulamalarınızı güvenli bir şekilde sunmak için Azure Application Gateway 'i yapılandırın.
ms.topic: how-to
ms.date: 07/31/2020
ms.openlocfilehash: d4e193c58c5eccb29f603c3b4d56a09d26686975
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750608"
---
# <a name="use-azure-application-gateway-to-protect-your-web-apps-on-azure-vmware-solution"></a>Azure VMware çözümünde Web uygulamalarınızı korumak için Azure Application Gateway kullanma

[Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) , Web uygulamalarınıza yönelik trafiği yönetmenizi sağlayan bir katman 7 Web trafiği yük dengeleyicidir. Birçok özellik sunar: tanımlama bilgisi tabanlı oturum benzeşimi, URL tabanlı Yönlendirme ve Web uygulaması güvenlik duvarı (WAF) birkaç kez adı. (Özelliklerin tüm listesi için bkz. [Azure Application Gateway özellikleri](../application-gateway/features.md).) İki sürüm olan v1 ve v2 olarak sunulur. Her ikisi de Azure VMware çözümünde çalışan Web Apps ile test edilmiştir.

Bu makalede, Azure VMware çözümünde çalışan bir Web uygulamasını korumaya yönelik bir dizi yapılandırma ve öneriyle bir Web sunucusu grubunun önünde Application Gateway kullanarak genel bir senaryoya yol göstereceğiz. 

## <a name="topology"></a>Topoloji
Aşağıdaki şekilde gösterildiği gibi, Azure IaaS sanal makinelerini, Azure sanal makine ölçek kümelerini veya şirket içi sunucuları korumak için Application Gateway kullanılabilir. Azure VMware çözümü sanal makineleri, Application Gateway tarafından şirket içi sunucular olarak kabul edilir.

![Application Gateway Azure VMware Çözüm VM 'lerini korur.](media/protect-avs-web-apps-with-app-gw/app-gateway-protects.png)

> [!IMPORTANT]
> Azure Application Gateway Şu anda Azure VMware Çözüm sanal makinelerinde çalışan Web uygulamalarını kullanıma sunmak için desteklenen tek yöntemdir.

Aşağıdaki diyagramda Azure VMware Çözüm Web uygulamalarıyla Application Gateway doğrulamak için kullanılan test senaryosu gösterilmektedir.

![Web Apps çalıştıran Azure VMware çözümü ile tümleştirme Application Gateway.](media/protect-avs-web-apps-with-app-gw/app-gateway-avs-scenario.png)

Application Gateway örnek, Hub üzerinde ayrılmış bir alt ağda dağıtılır. Azure genel IP adresine sahiptir; sanal ağ için standart DDoS korumasının etkinleştirilmesi önerilir. Web sunucusu, NSX T0 ve T1 yönlendiricilerinin arkasındaki bir Azure VMware çözümü özel bulutu üzerinde barındırılır. Azure VMware çözümü, hub ve şirket içi sistemlerle iletişimi etkinleştirmek için [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) kullanır.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı.
- Azure VMware çözümü özel bulutu dağıtıldı ve çalışıyor.

## <a name="deployment-and-configuration"></a>Dağıtım ve yapılandırma

1. Azure portal, **Application Gateway** araması yapın ve **uygulama ağ geçidi oluştur**' u seçin.

2. Aşağıdaki şekilde temel ayrıntıları sağlayın; ardından Ileri ' yi seçin **: ön uçlar>**. 

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/create-app-gateway.png" alt-text="Application Gateway oluşturma":::

3. Ön uç IP adresi türünü seçin. Ortak için, mevcut bir genel IP adresi seçin veya yeni bir tane oluşturun. **İleri ' yi seçin: backends>**.

    > [!NOTE]
    > Özel ön uçlar için yalnızca standart ve Web uygulaması güvenlik duvarı (WAF) SKU 'Ları desteklenir.

4. Sonra, uygulamanın veya hizmetin parçası olan bir örnek kümesini açıklayan bir arka uç havuzu ekleyin (Bu durumda, Azure VMware Çözüm altyapısında çalışan sanal makineler). Azure VMware çözümü özel bulutu üzerinde çalışan Web sunucularının ayrıntılarını sağlayın ve **Ekle**' yi seçin. ardından Ileri ' yi seçin **: yapılandırma>**.

1. **Yapılandırma** sekmesinde, **yönlendirme kuralı ekle**' yi seçin.

6. **Dinleyici** sekmesinde, dinleyicinin ayrıntılarını sağlayın. HTTPS seçilirse, bir PFX dosyasından veya Azure Key Vault bir sertifika ile bir sertifikanın sağlanması gerekir. 

7. **Arka uç hedefleri** sekmesini seçin ve daha önce oluşturulan arka uç havuzunu seçin. **Http ayarları** alanı Için **Yeni Ekle**' yi seçin.

8. HTTP ayarlarının parametrelerini yapılandırın. **Ekle**’yi seçin.

9. Yol tabanlı kuralları yapılandırmak istiyorsanız, **yol tabanlı bir kural oluşturmak için birden çok hedef Ekle**' yi seçin. 

10. Yol tabanlı bir kural ekleyin ve **Ekle**' yi seçin. Başka yol tabanlı kurallar eklemek için tekrarlayın. 

11. Yol tabanlı kurallar eklemeyi bitirdiğinizde yeniden **Ekle** ' yi seçin. ardından Ileri ' yi seçin **: etiketler>**. 

12. İstediğiniz etiketleri ekleyin. **İleri ' yi seçin: gözden geçir + oluştur>**.

13. Application Gateway bir doğrulama çalışacaktır; başarılı olursa, dağıtmak için **Oluştur** ' u seçin.

## <a name="configuration-examples"></a>Yapılandırma örnekleri

Bu bölümde, Azure VMware Çözüm sanal makinelerini Application Gateway aşağıdaki kullanım örnekleri için arka uç havuzları olarak nasıl yapılandıracağınızı öğreneceksiniz: 

- [Birden çok site barındırma](#hosting-multiple-sites)
- [URL 'ye göre yönlendirme](#routing-by-url)

### <a name="hosting-multiple-sites"></a>Birden çok site barındırma

Uygulama ağ geçidi oluştururken birden çok Web sitesini barındırmayı yapılandırmak için Azure portal kullanabilirsiniz. Bu öğreticide, mevcut bir uygulama ağ geçidinde Azure VMware çözümü özel bulutu üzerinde çalışan sanal makineleri kullanarak arka uç adres havuzları tanımlarsınız. Uygulama ağ geçidi, [Azure VMware çözümünü bir hub ve bağlı bileşen mimarisinde tümleştirme](concepts-avs-hub-and-spoke-integration.md)bölümünde açıklandığı gibi bir hub sanal ağının parçasıdır. Bu öğreticide, birden çok etki alanı olduğu varsayılır ve www.contoso.com ve www.fabrikam.com örnekleri kullanılmaktadır.

1. Sanal makineleri oluşturun. Azure VMware çözümü özel bulutu üzerinde iki farklı sanal makine havuzu oluşturun; tek bir contoso ve ikinci fabrikam 'ı temsil eder. 

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs.png" alt-text="Sanal makineleri oluşturun.":::

    Bu öğreticiyi anlamak için Windows Server 2016 Internet Information Services (IIS) rolünün yüklü olduğunu kullandık. Sanal makineler yüklendikten sonra, VM 'lerin her birinde IIS 'yi yapılandırmak için aşağıdaki PowerShell komutlarını çalıştırın. 

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

2. Arka uç havuzlarını ekleyin. Mevcut bir Application Gateway örneğinde sol menüden **arka uç havuzları** ' nı seçin,  **Ekle**' yi seçin ve yeni havuzların ayrıntılarını girin. Sağ bölmede **Ekle** ' yi seçin.

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs-02.png" alt-text="Arka uç havuzlarını ekleyin." lightbox="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs-02.png":::

3. **Dinleyiciler** bölümünde her Web sitesi için yeni bir dinleyici oluşturun. Her bir dinleyicinin ayrıntılarını girin ve **Ekle**' yi seçin.

4. Sol gezinti çubuğunda **http ayarları** ' nı seçin ve sol bölmeden **Ekle** ' yi seçin. Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve **Kaydet**' i seçin.

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs-03.png" alt-text="Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve Kaydet ' i seçin." lightbox="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs-03.png":::

5. Sol menünün **kurallar** bölümünde kuralları oluşturun. Her kuralı ilgili dinleyiciyle ilişkilendirin. **Ekle**’yi seçin.

6. Karşılık gelen arka uç havuzunu ve HTTP ayarlarını yapılandırın. **Ekle**’yi seçin.

7. Bağlantıyı test edin. Tercih ettiğiniz tarayıcınızı açın ve Azure VMware Çözüm ortamınızda barındırılan farklı Web sitelerine gidin (örneğin,) http://www.fabrikam.com .

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-multi-backend-pool-avs-07.png" alt-text="Bağlantıyı test edin.":::

### <a name="routing-by-url"></a>URL 'ye göre yönlendirme

Azure Application Gateway, URL yolu tabanlı yönlendirme kurallarını yapılandırmak için kullanabilirsiniz. Bu öğreticide, mevcut bir uygulama ağ geçidinde Azure VMware çözümü özel bulutu üzerinde çalışan sanal makineleri kullanarak arka uç adres havuzları tanımlarsınız. Uygulama ağ geçidi, [Azure VMware çözümü Azure yerel tümleştirme belgelerinde](concepts-avs-hub-and-spoke-integration.md)açıklandığı gibi bir hub sanal ağının parçasıdır. Daha sonra, Web trafiğinin havuzlardaki uygun sunuculara ulaştığınızdan emin olmak için yönlendirme kuralları oluşturursunuz.

1. Sanal makineleri oluşturun. Azure VMware Çözüm özel bulutu 'nda, Web grubunu temsil eden bir sanal makine havuzu oluşturun. 

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs.png" alt-text="Azure VMware çözümünde bir sanal makine havuzu oluşturun.":::

    Bu öğreticiyi göstermek için IIS rolü yüklü olan Windows Server 2016 kullanılmıştır. Sanal makineler yüklendikten sonra, IIS 'yi her VM 'deki öğreticide yapılandırmak için aşağıdaki PowerShell komutlarını çalıştırın. 

    İlk sanal makine olan contoso-Web-01, ana Web sitesini barındıracaktır.

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

    Contoso-Web-02 ikinci sanal makine, görüntüler sitesini barındıracaktır.
 
    ```powershell
    Install-WindowsFeature -Name Web-Server
    New-Item -Path "C:\inetpub\wwwroot\" -Name "images" -ItemType "directory"
    Add-Content -Path C:\inetpub\wwwroot\images\test.htm -Value $($env:computername)
    ```

    Contoso-Web-03 olan üçüncü sanal makine, video sitesini barındıracaktır.

    ```powershell
    Install-WindowsFeature -Name Web-Server
    New-Item -Path "C:\inetpub\wwwroot\" -Name "video" -ItemType "directory"
    Add-Content -Path C:\inetpub\wwwroot\video\test.htm -Value $($env:computername)
    ```

2. Arka uç havuzlarını ekleyin. Mevcut bir Application Gateway örneğine üç yeni arka uç havuzu eklemeniz gerekir. Sol menüden **arka uç havuzları** ' nı seçin. **Ekle** ' yi seçin ve ilk havuzun ( **contoso-Web**) ayrıntılarını girin. Hedef olarak bir VM ekleyin. **Ekle**’yi seçin. Her birine hedef olarak benzersiz bir VM ekleyerek **contoso görüntüleri** ve **contoso-video**için bu işlemi tekrarlayın. 

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-02.png" alt-text="Üç yeni arka uç havuzu ekleyin." lightbox="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-02.png":::

3. **Dinleyiciler** bölümünde, 8080 numaralı bağlantı noktasını kullanarak temel türünde yeni bir dinleyici oluşturun.

4. Sol gezinti çubuğunda **http ayarları** ' nı seçin ve sol bölmeden **Ekle** ' yi seçin. Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve **Kaydet**' i seçin.

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-04.png" alt-text="HTP ayarları yapılandırması.":::

5. Sol menünün **kurallar** bölümünde kuralları oluşturun. Her kuralı önceden oluşturulmuş dinleyiciyle ilişkilendirin. Ardından ana arka uç havuzunu ve HTTP ayarlarını yapılandırın. **Ekle**’yi seçin.

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-07.png" alt-text="Sol menünün kurallar bölümünde kuralları oluşturun.":::

6. Yapılandırmayı test edin. Azure portal uygulama ağ geçidine erişin ve **genel bakış** bölümünde genel IP adresini kopyalayın. Sonra yeni bir tarayıcı penceresi açın ve URL 'YI girin `http://<app-gw-ip-address>:8080` . 

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-08.png" alt-text="Azure portal yapılandırmayı test edin.":::

    URL'yi `http://<app-gw-ip-address>:8080/images/test.htm` olarak değiştirin.

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-09.png" alt-text="URL 'YI değiştirin.":::

    URL 'YI tekrar olarak değiştirin `http://<app-gw-ip-address>:8080/video/test.htm` .

    :::image type="content" source="media/protect-avs-web-apps-with-app-gw/app-gateway-url-route-backend-pool-avs-10.png" alt-text="URL 'YI yeniden değiştirin.":::

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla yapılandırma örneği için [Azure Application Gateway belgelerini](https://docs.microsoft.com/azure/application-gateway/) gözden geçirin.
