---
title: Azure VMware çözümünde Web uygulamalarınızı korumak için Azure Application Gateway kullanma
description: Azure VMware çözümünde çalışan Web uygulamalarınızı güvenli bir şekilde sunmak için Azure Application Gateway 'i yapılandırın.
ms.topic: how-to
ms.date: 10/13/2020
ms.openlocfilehash: 7956ea51421f5cfa893942401c1d9a5871039689
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578485"
---
# <a name="use-azure-application-gateway-to-protect-your-web-apps-on-azure-vmware-solution"></a>Azure VMware çözümünde Web uygulamalarınızı korumak için Azure Application Gateway kullanma

[Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) , Web uygulamalarınıza yönelik trafiği yönetmenizi sağlayan bir katman 7 Web trafiği yük dengeleyicidir. Azure VMware Çözüm v 1.0 ve v 2.0 ile birlikte sunulur. Her iki sürüm de Azure VMware çözümünde çalışan Web Apps ile test edilmiştir.

Özellikleri şunlardır: 
- Tanımlama bilgisi tabanlı oturum benzeşimi
- URL tabanlı yönlendirme
- Web Uygulaması Güvenlik Duvarı (WAF)

Özelliklerin tamamı listesi için bkz. [Azure Application Gateway özellikleri](../application-gateway/features.md). 

Bu makalede, Azure VMware çözümünde çalışan bir Web uygulamasını korumak için bir Web sunucusu grubunun önünde applicate ağ geçidini nasıl kullanacağınız gösterilmektedir. 

## <a name="topology"></a>Topoloji
Diyagram, Azure IaaS sanal makinelerini (VM), Azure sanal makine ölçek kümelerini veya şirket içi sunucuları korumak için Application Gateway nasıl kullanıldığını gösterir. Application Gateway, Azure VMware Çözüm VM 'lerini şirket içi sunucular olarak değerlendirir. 

![Application Gateway Azure VMware Çözüm VM 'lerini korur.](media/protect-azure-vmware-solution-with-application-gateway/app-gateway-protects.png)

> [!IMPORTANT]
> Azure Application Gateway Şu anda Azure VMware Çözüm VM 'lerinde çalışan Web uygulamalarını kullanıma sunmak için desteklenen tek yöntemdir.

Diyagramda, Azure VMware Çözüm Web uygulamalarıyla Application Gateway doğrulamak için kullanılan test senaryosu gösterilmektedir.

:::image type="content" source="media/hub-spoke/azure-vmware-solution-second-level-traffic-segmentation.png" alt-text="Web uygulamalarını çalıştıran Azure VMware çözümüyle Application Gateway tümleştirme" border="false":::

Application Gateway örnek, Hub üzerinde ayrılmış bir alt ağda dağıtılır. Azure genel IP adresine sahiptir. Sanal ağ için standart DDoS korumasının etkinleştirilmesi önerilir. Web sunucusu, NSX T0 ve T1 yönlendiricilerinin arkasındaki bir Azure VMware çözümü özel bulutu üzerinde barındırılır. Azure VMware çözümü, hub ve şirket içi sistemlerle iletişimi etkinleştirmek için [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) kullanır.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı.
- Azure VMware çözümü özel bulutu dağıtıldı ve çalışıyor.

## <a name="deployment-and-configuration"></a>Dağıtım ve yapılandırma

1. Azure portal, **Application Gateway** araması yapın ve **uygulama ağ geçidi oluştur** ' u seçin.

2. Aşağıdaki şekilde temel ayrıntıları sağlayın; ardından Ileri ' yi seçin **: ön uçlar>**. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/create-app-gateway.png" alt-text="Application Gateway oluşturma":::

3. Ön uç IP adresi türünü seçin. Ortak için, mevcut bir genel IP adresi seçin veya yeni bir tane oluşturun. **İleri ' yi seçin: backends>**.

    > [!NOTE]
    > Özel ön uçlar için yalnızca standart ve Web uygulaması güvenlik duvarı (WAF) SKU 'Ları desteklenir.

4. Azure VMware Çözüm altyapısında çalışan VM 'Ler için bir arka uç havuzu ekleyin. Azure VMware çözümü özel bulutu üzerinde çalışan Web sunucularının ayrıntılarını sağlayın ve **Ekle** ' yi seçin.  Ardından Ileri ' yi seçin **: yapılandırma>**.

1. **Yapılandırma** sekmesinde, **yönlendirme kuralı ekle** ' yi seçin.

6. **Dinleyici** sekmesinde, dinleyicinin ayrıntılarını sağlayın. HTTPS seçilirse, bir PFX dosyasından veya var olan bir Azure Key Vault sertifikasından bir sertifika sağlamanız gerekir. 

7. **Arka uç hedefleri** sekmesini seçin ve daha önce oluşturulan arka uç havuzunu seçin. **Http ayarları** alanı Için **Yeni Ekle** ' yi seçin.

8. HTTP ayarlarının parametrelerini yapılandırın. **Ekle** ’yi seçin.

9. Yol tabanlı kuralları yapılandırmak istiyorsanız, **yol tabanlı bir kural oluşturmak için birden çok hedef Ekle** ' yi seçin. 

10. Yol tabanlı bir kural ekleyin ve **Ekle** ' yi seçin. Başka yol tabanlı kurallar eklemek için tekrarlayın. 

11. Yol tabanlı kurallar eklemeyi bitirdiğinizde yeniden **Ekle** ' yi seçin. ardından Ileri ' yi seçin **: etiketler>**. 

12. Etiketler ekleyin ve ardından Ileri ' yi seçin **: gözden geçir + oluştur>**.

13. Application Gateway bir doğrulama çalışacaktır; başarılı olursa, dağıtmak için **Oluştur** ' u seçin.

## <a name="configuration-examples"></a>Yapılandırma örnekleri

Bu bölümde, bu kullanım durumları için arka uç havuzları olarak Azure VMware Çözüm VM 'Leri ile Application Gateway yapılandırmayı öğreneceksiniz: 

- [Birden çok site barındırma](#hosting-multiple-sites)
- [URL 'ye göre yönlendirme](#routing-by-url)

### <a name="hosting-multiple-sites"></a>Birden çok site barındırma

Bu yordamda, mevcut bir uygulama ağ geçidinde Azure VMware çözümü özel bulutu üzerinde çalışan VM 'Ler kullanılarak arka uç adres havuzlarının nasıl tanımlanacağı gösterilmektedir. 

>[!NOTE]
>Bu yordamda birden çok etki alanı olduğu varsayılır, bu nedenle www.contoso.com ve www.fabrikam.com örneklerini kullanacağız.


1. Özel bulutunuzda iki farklı VM havuzu oluşturun. Biri contoso ve ikinci fabrikam 'ı temsil eder. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs.png" alt-text="VM 'Ler oluşturun.":::

    Bu öğreticiyi göstermek için Internet Information Services (IIS) rolü yüklü Windows Server 2016 ' i kullandık. VM 'Ler yüklendikten sonra, VM 'lerin her birinde IIS 'yi yapılandırmak için aşağıdaki PowerShell komutlarını çalıştırın. 

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

2. Mevcut bir Application Gateway örneğinde sol menüden **arka uç havuzları** ' nı seçin,  **Ekle** ' yi seçin ve yeni havuzların ayrıntılarını girin. Sağ bölmede **Ekle** ' yi seçin.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-02.png" alt-text="Arka uç havuzlarını ekleyin." lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-02.png":::

3. **Dinleyiciler** bölümünde her Web sitesi için yeni bir dinleyici oluşturun. Her bir dinleyicinin ayrıntılarını girin ve **Ekle** ' yi seçin.

4. Sol tarafta **http ayarları** ' nı seçin ve sol bölmedeki **Ekle** ' yi seçin. Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve **Kaydet** ' i seçin.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-03.png" alt-text="Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve Kaydet ' i seçin." lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-03.png":::

5. Sol menünün **kurallar** bölümünde kuralları oluşturun. Her kuralı ilgili dinleyiciyle ilişkilendirin. **Ekle** ’yi seçin.

6. Karşılık gelen arka uç havuzunu ve HTTP ayarlarını yapılandırın. **Ekle** ’yi seçin.

7. Bağlantıyı test edin. Tercih ettiğiniz tarayıcınızı açın ve Azure VMware Çözüm ortamınızda barındırılan farklı Web sitelerine gidin (örneğin,) http://www.fabrikam.com .

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-07.png" alt-text="Bağlantıyı test edin.":::

### <a name="routing-by-url"></a>URL 'ye göre yönlendirme

Bu yordamda, mevcut bir uygulama ağ geçidinde Azure VMware çözümü özel bulutu üzerinde çalışan VM 'Ler kullanılarak arka uç adres havuzlarının nasıl tanımlanacağı gösterilmektedir. Daha sonra, Web trafiğinin havuzlardaki uygun sunuculara ulaştığınızdan emin olmak için yönlendirme kuralları oluşturursunuz.

1. Özel bulutunuzda, Web grubunu temsil eden bir sanal makine havuzu oluşturun. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs.png" alt-text="Azure VMware çözümünde bir VM havuzu oluşturun.":::

    Bu öğreticiyi göstermek için IIS rolü yüklü olan Windows Server 2016 kullanılmıştır. VM 'Ler yüklendikten sonra, IIS 'yi her bir VM öğreticisi için yapılandırmak üzere aşağıdaki PowerShell komutlarını çalıştırın. 

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

2. Mevcut bir Application Gateway örneğine üç yeni arka uç havuzu ekleyin. 

   1. Sol menüden **arka uç havuzları** ' nı seçin. 
   1. **Ekle** ' yi seçin ve ilk havuzun ( **contoso-Web** ) ayrıntılarını girin. 
   1. Hedef olarak bir VM ekleyin. 
   1. **Ekle** ’yi seçin. 
   1. Hedef olarak benzersiz bir VM ekleyerek **contoso görüntüleri** ve **contoso-video** için bu işlemi tekrarlayın. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-02.png" alt-text="Üç yeni arka uç havuzu ekleyin." lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-02.png":::

3. **Dinleyiciler** bölümünde, 8080 numaralı bağlantı noktasını kullanarak temel türünde yeni bir dinleyici oluşturun.

4. Sol gezinti çubuğunda **http ayarları** ' nı seçin ve sol bölmeden **Ekle** ' yi seçin. Ayrıntıları doldurarak yeni bir HTTP ayarı oluşturun ve **Kaydet** ' i seçin.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-04.png" alt-text="HTP ayarları yapılandırması.":::

5. Sol menünün **kurallar** bölümünde kuralları oluşturun. Her kuralı önceden oluşturulmuş dinleyiciyle ilişkilendirin. Ardından ana arka uç havuzunu ve HTTP ayarlarını yapılandırın. **Ekle** ’yi seçin.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-07.png" alt-text="Sol menünün kurallar bölümünde kuralları oluşturun.":::

6. Yapılandırmayı test edin. Azure portal uygulama ağ geçidine erişin ve genel IP adresini **genel bakış** bölümüne kopyalayın. 

   1. Yeni bir tarayıcı penceresi açın ve URL 'YI girin `http://<app-gw-ip-address>:8080` . 

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-08.png" alt-text="Azure portal yapılandırmayı test edin.":::

   1. URL'yi `http://<app-gw-ip-address>:8080/images/test.htm` olarak değiştirin.

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-09.png" alt-text="URL 'YI değiştirin.":::

   1. URL 'YI tekrar olarak değiştirin `http://<app-gw-ip-address>:8080/video/test.htm` .

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-10.png" alt-text="URL 'YI yeniden değiştirin.":::

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla yapılandırma örneği için [Azure Application Gateway belgelerini](../application-gateway/index.yml) gözden geçirin.