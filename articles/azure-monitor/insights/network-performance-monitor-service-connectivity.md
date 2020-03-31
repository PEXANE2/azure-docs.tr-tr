---
title: Ağ Performans Monitörü çözüm Hizmeti Bağlantısı - Azure Günlük Analizi
description: Ağ bağlantısının açık bir TCP bağlantı noktası olan herhangi bir uç noktaya bağlantısını izlemek için Ağ Performans Monitörü'ndeki Hizmet Bağlantısı İzleme özelliğini kullanın.
ms.subservice: logs
ms.topic: conceptual
author: abshamsft
ms.author: absha
ms.date: 02/20/2018
ms.openlocfilehash: 93f3820b7cf1db85b9ff4cd514fe22efc75a90d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249249"
---
# <a name="service-connectivity-monitor"></a>Hizmet Bağlantısı İzleyicisi

Ağ Performans İzleyicisi'ndeki [Network Performance Monitor](network-performance-monitor.md) Hizmet Bağlantısı İzleme özelliğini, açık TCP bağlantı noktası olan herhangi bir uç noktaya ağ bağlantısını izlemek için kullanabilirsiniz. Bu uç noktalar web sitelerini, SaaS uygulamalarını, PaaS uygulamalarını ve SQL veritabanlarını içerir. 

Service Connectivity Monitor ile aşağıdaki işlevleri gerçekleştirebilirsiniz: 

- Birden fazla şube veya konumdan uygulamalarınıza ve ağ hizmetlerinize ağ bağlantısını izleyin. Uygulamalar ve ağ hizmetleri Office 365, Dynamics CRM, şirket içi iş satırı uygulamaları ve SQL veritabanlarını içerir.
- Office 365 ve Dynamics 365 uç noktalarına ağ bağlantısını izlemek için yerleşik testleri kullanın. 
- Yanıt süresini, ağ gecikmesini ve bitiş noktasına bağlanırken karşılaşılan paket kaybını belirleyin.
- Kötü uygulama performansının ağdan mı yoksa uygulama sağlayıcısının ucundaki bazı sorundan mı olduğunu belirleyin.
- Bir topoloji haritasında her atlama tarafından katkıda bulunulan gecikme süresi görüntüleyerek kötü uygulama performansına neden olabilir ağ üzerinde sıcak noktalar belirleyin.


![Hizmet Bağlantısı İzleyicisi](media/network-performance-monitor-service-endpoint/service-endpoint-intro.png)


## <a name="configuration"></a>Yapılandırma 
Ağ Performans Monitörü için yapılandırmayı açmak [için, Ağ Performans Monitörü çözümünü](network-performance-monitor.md) açın ve **Yapılandır'ı**seçin.

![Ağ Performansı İzleyicisi’ni Yapılandırma](media/network-performance-monitor-service-endpoint/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>İzleme için Log Analytics aracılarını yapılandırın
Çözümün düğümlerinizden hizmet bitiş noktasına kadar topolojiyi keşfedebilmesi için izleme için kullanılan düğümlerde aşağıdaki güvenlik duvarı kurallarını etkinleştirin: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Hizmet Bağlantısı İzleme testleri oluşturun 

Hizmet uç noktalarına ağ bağlantısını izlemek için testlerinizi oluşturmaya başlayın.

1. Hizmet **Bağlantısı İzleme** sekmesini seçin.
2. **Test Ekle'yi**seçin ve test adını ve açıklamasını girin. Çalışma alanı başına en fazla 450 test oluşturabilirsiniz. 
3. Test türünü seçin:<br>

    * outlook.office365.com veya bing.com gibi HTTP/S isteklerine yanıt veren bir hizmete bağlantıyı izlemek için **Web'i** seçin.<br>
    * TCP isteklerine yanıt veren ancak SQL sunucusu, FTP sunucusu veya SSH bağlantı noktası gibi HTTP/S isteklerine yanıt vermeyen bir hizmete bağlantıyı izlemek için **Ağ'ı** seçin. 
    * Örneğin: Blob depolama hesabına bir web testi oluşturmak için **Web'i** seçin ve hedefin *depo hesabınız*olarak girin .blob.core.windows.net. Benzer [şekilde, bu bağlantıyı](https://docs.microsoft.com/azure/storage/common/storage-account-overview#storage-account-endpoints) kullanarak diğer tablo depolama, sıra depolama ve Azure Dosyaları için testler oluşturabilirsiniz.
4. Ağ gecikmesi, paket kaybı ve topoloji bulma gibi ağ ölçümleri gerçekleştirmek istemiyorsanız, **ağ ölçümlerini gerçekleştir** onay kutusunu temizleyin. Yeteneğinden maksimum fayda elde etmek için seçili tutun. 
5. **Hedef'te,** ağ bağlantısını izlemek istediğiniz URL/FQDN/IP adresini girin.
6. **Bağlantı noktası numarasında,** hedef hizmetin bağlantı noktası numarasını girin. 
7. **Test**Sıklığı'nda, testin ne sıklıkta çalışmasını istediğinize göre bir değer girin. 
8. Hizmete ağ bağlantısını izlemek istediğiniz düğümleri seçin. Test başına eklenen aracı sayısının 150'den az olduğundan emin olun. Herhangi bir aracı maksimum 150 uç nokta/aracıyı test edebilir.

    >[!NOTE]
    > Windows sunucu tabanlı düğümler için, bu özellik ağ ölçümlerini gerçekleştirmek için TCP tabanlı istekleri kullanır. Windows istemci tabanlı düğümler için, bu özellik ağ ölçümlerini gerçekleştirmek için ICMP tabanlı istekleri kullanır. Bazı durumlarda, düğümler Windows istemci tabanlı olduğunda hedef uygulama icmp tabanlı isteklerini engeller. Çözüm ağ ölçümleri gerçekleştiremiyor. Bu gibi durumlarda Windows sunucu tabanlı düğümler kullanmanızı öneririz. 

9. Seçtiğiniz öğeler için sistem durumu olayları oluşturmak istemiyorsanız, **bu testin kapsadığı hedeflerde Sistem Durumu İzlemeyi Etkinleştir'i**temizleyin. 
10. İzleme koşullarını seçin. Eşik değerlerini girerek sistem durumu oluşturma için özel eşikler ayarlayabilirsiniz. Koşulun değeri seçili ağ veya alt ağ çifti için seçili eşiğinin üzerine çıktığında, bir sistem durumu olayı oluşturulur. 
11. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. 

    ![Servis Bağlantısı Monitörü test yapılandırmaları](media/network-performance-monitor-service-endpoint/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Kılavuz 

Ağ Performans İzleme panosu görünümüne gidin. Oluşturduğunuz farklı testlerin durumunun bir özetini almak için **Hizmet Bağlantısı İzleme** sayfasına bakın. 

![Servis Bağlantı İzleme sayfası](media/network-performance-monitor-service-endpoint/service-endpoint-blade.png)

**Testler** sayfasındaki testlerin ayrıntılarını görüntülemek için döşemeyi seçin. Soldaki tabloda, tüm testler için hizmet yanıt süresinin, ağ gecikmesinin ve paket kaybının zaman içinde sağlık durumunu ve değerini görüntüleyebilirsiniz. Ağ anlık görüntüsünü geçmişte başka bir zamanda görüntülemek için Ağ Durumu Kaydedici denetimini kullanın. Araştırmak istediğiniz tablodaki testi seçin. Sağdaki bölmedeki grafiklerde, kaybın, gecikme süresinin ve yanıt süresi değerlerinin tarihsel eğilimini görüntüleyebilirsiniz. Her düğümden performansı görüntülemek için **Test Ayrıntıları** bağlantısını seçin.

![Servis Bağlantı Monitörü testleri](media/network-performance-monitor-service-endpoint/service-endpoint-tests.png)

Test **Düğümleri** görünümünde, her düğümden ağ bağlantısını gözlemleyebilirsiniz. Performans bozulması olan düğümü seçin. Bu, uygulamanın yavaş çalıştığı gözlenen düğümdür.

Uygulamanın yanıt süresi ile ağ gecikmesi arasındaki ilişkiyi gözlemleyerek, kötü uygulama performansının ağdan mı yoksa uygulama sağlayıcısının sonundaki bir sorundan mı bağlı olduğunu belirleyin. 

* **Uygulama sorunu:** Yanıt süresinde bir artış ancak ağ gecikmesindeki tutarlılık, ağın iyi çalıştığını ve sorunun uygulama sonundaki bir sorundan kaynaklanabileceğini gösterir. 

    ![Hizmet Bağlantı Monitörü uygulama sorunu](media/network-performance-monitor-service-endpoint/service-endpoint-application-issue.png)

* **Ağ sorunu:** Yanıt süresindeki ani artış, ağ gecikmesindeki artışa eşlik eder, yanıt süresindeki artışın ağ gecikmesindeki artışa bağlı olabileceğini düşündürmektedir. 

    ![Hizmet Bağlantısı İzleme ağ sorunu](media/network-performance-monitor-service-endpoint/service-endpoint-network-issue.png)

Sorunun ağ dan dolayı olduğunu belirledikten sonra, topoloji haritasındaki zahmetli atlamayı belirlemek için **Topoloji** görünüm bağlantısını seçin. Aşağıdaki resimde bir örnek gösterilmiştir. Düğüm ve uygulama bitiş noktası arasındaki 105 ms toplam gecikme süresinin 96 ms'i kırmızı ile işaretlenmiş atlamadan dolayıdır. Zahmetli atlamayı tanımladıktan sonra düzeltici eylemde olabilirsiniz. 

![Servis Bağlantı Monitörü testleri](media/network-performance-monitor-service-endpoint/service-endpoint-topology.png)

## <a name="diagnostics"></a>Tanılama 

Bir anormallik gözlemlerseniz, aşağıdaki adımları izleyin:

* Hizmet yanıt süresi, ağ kaybı ve gecikme süresi NA olarak gösterilirse, aşağıdaki nedenlerden biri veya birkaçı neden olabilir:

    - Uygulama düştü.
    - Hizmete ağ bağlantısını denetlemek için kullanılan düğüm indi.
    - Test yapılandırmasına girilen hedef yanlıştır.
    - Düğümde ağ bağlantısı yok.

* Geçerli bir hizmet yanıt süresi gösterilir, ancak ağ kaybı ve gecikme süresi NA olarak gösterilirse, aşağıdaki nedenlerden biri veya birkaçı neden olabilir:

    - Hizmete ağ bağlantısını denetlemek için kullanılan düğüm bir Windows istemci salı makinesiyse, hedef hizmet ICMP isteklerini engelliyor veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteklerini engelliyor.
    - **Ağ ölçülerini gerçekleştir** onay kutusu test yapılandırmasında boştur. 

* Hizmet yanıt süresi NA ise, ağ kaybı ve gecikme süresi de geçerliyse, hedef hizmet bir web uygulaması olmayabilir. Test yapılandırmasını düzenleme ve **Web**yerine **Ağ** olarak test türünü seçin. 

* Uygulama yavaş çalışıyorsa, kötü uygulama performansının ağdan mı yoksa uygulama sağlayıcısının ucundaki bir sorundan mı bağlı olduğunu belirleyin.

## <a name="gcc-office-urls-for-us-government-customers"></a>ABD Hükümeti müşterileri için GCC Ofis URL'leri
ABD Hükümeti Virginia bölgesi için, yalnızca DOD URL'leri yerleşik NPM'dir. GCC URL'lerini kullanan müşterilerin özel testler oluşturması ve her URL'yi ayrı ayrı eklemesi gerekir.

| Alan | GCC |
|:---   |:--- |
| Office 365 Portal ve paylaşılan | portal.apps.mil |
| Office 365 auth ve kimlik | * login.microsoftonline.us <br> * api.login.microsoftonline.com <br> * clientconfig.microsoftonline-p.net <br> * login.microsoftonline.com <br> * login.microsoftonline-p.com <br> * login.windows.net <br> * loginex.microsoftonline.com <br> * login-us.microsoftonline.com <br> * nexus.microsoftonline-p.com <br> * mscrl.microsoft.com <br> * secure.aadcdn.microsoftonline-p.com |
| Office Online | * adminwebservice.gov.us.microsoftonline.com <br>  * adminwebservice-s1-bn1a.microsoftonline.com <br> * adminwebservice-s1-dm2a.microsoftonline.com <br> * becws.gov.us.microsoftonline.com <br> * provisioningapi.gov.us.microsoftonline.com <br> * officehome.msocdn.us <br> * prod.msocdn.us <br> * portal.office365.us <br> * webshell.suite.office365.us <br> * www .office365.us <br> * activation.sls.microsoft.com <br> * crl.microsoft.com <br> * go.microsoft.com <br> * insertmedia.bing.office.net <br> * ocsa.officeapps.live.com <br> * ocsredir.officeapps.live.com <br> * ocws.officeapps.live.com <br> * office15client.microsoft.com <br>* officecdn.microsoft.com <br> * officecdn.microsoft.com.edgesuite.net <br> * officepreviewredir.microsoft.com <br> * officeredir.microsoft.com <br> * ols.officeapps.live.com  <br> * r.office.microsoft.com <br> * cdn.odc.officeapps.live.com <br> * odc.officeapps.live.com <br> * officeclient.microsoft.com |
| Exchange Online | * outlook.office365.us <br> * attachments.office365-net.us <br> * autodiscover-s.office365.us <br> * manage.office365.us <br> * scc.office365.us |
| MS Takımları | gov.teams.microsoft.us | 

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı ağ performansı veri kayıtlarını görüntülemek için [günlükleri arayın.](../../azure-monitor/log-query/log-query-overview.md)
