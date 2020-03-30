---
title: Azure CDN uç noktaları - 404 durum kodu sorun giderme
description: Azure CDN uç noktalarıyla 404 yanıt kodlarını sorun giderin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c332c6712cdf057491e3039854aa1a29bd54196f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083126"
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>404 durum kodu döndüren Azure CDN uç noktalarıyla sorun giderme
Bu makale, 404 HTTP yanıt durum kodlarını döndüren Azure İçerik Teslim Ağı (CDN) uç noktalarıyla ilgili sorunları gidermenize olanak tanır.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

## <a name="symptom"></a>Belirti
Bir CDN profili ve bir bitiş noktası oluşturdunuz, ancak içeriğiniz CDN'de kullanılamıyor gibi görünüyor. CDN URL üzerinden içeriğinize erişmeye çalışan kullanıcılara bir HTTP 404 durum kodu alırsınız. 

## <a name="cause"></a>Nedeni
Çeşitli olası nedenleri vardır, dahil:

* Dosyanın kaynağı CDN tarafından görülemez.
* Bitiş noktası yanlış yapılandırılmıştır ve CDN'nin yanlış yerde görünmesine neden olur.
* Ana bilgisayar, CDN'den ana bilgisayar üstbilgisini reddediyor.
* Bitiş noktası CDN boyunca yaymak için zaman olmadı.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!IMPORTANT]
> CdN bitiş noktası oluşturduktan sonra, kaydın CDN'de yayılması zaman aldığından, hemen kullanılamaz:
> - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle on dakikada tamamlanır. 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır. 
> 
> Bu belgedeki adımları tamamlarsanız ve hala 404 yanıt alıyorsanız, destek biletini açmadan önce tekrar kontrol etmek için birkaç saat beklemeyi düşünün.
> 
> 

### <a name="check-the-origin-file"></a>Kaynak dosyasını denetleme
İlk olarak, önbelleğe açılan dosyanın kaynak sunucuda kullanılabildiğini ve internetüzerinden herkese açık olduğunu doğrulayın. Bunu yapmanın en hızlı yolu, özel veya gizli oturumda bir tarayıcı açmak ve doğrudan dosyaya göz atmaktır. URL'yi adres kutusuna yazın veya yapıştırın ve bunun beklediğiniz dosyayla sonuçlanıp sonuçverdiğini doğrulayın. Örneğin, bir Azure Depolama hesabında https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt adresinden erişilebilen bir dosyanız olduğunu varsayalım. Bu dosyanın içeriğini başarıyla yükleyebiliyorsanız, testi geçer.

![Başarılı!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Bu, dosyanızın genel kullanıma açık olduğunu doğrulamanın en hızlı ve en kolay yolu olsa da, kuruluşunuzdaki bazı ağ yapılandırmaları, bir dosyanın yalnızca ağınızın kullanıcıları tarafından kullanılabilir olduğunda (barındırılan dosyada olsa bile) genel kullanıma sunulduğunu ortaya çıkartabilir Azure). Böyle olmadığından emin olmak için, dosyayı kuruluşunuzun ağına bağlı olmayan bir mobil aygıt veya Azure'daki sanal makine gibi harici bir tarayıcıyla sınayın.
> 
> 

### <a name="check-the-origin-settings"></a>Kaynak ayarlarını kontrol edin
Dosyanın internette herkese açık olduğunu doğruladıktan sonra, başlangıç ayarlarınızı doğrulayın. Azure [Portalı'nda](https://portal.azure.com)CDN profilinize göz atın ve sorun giderdiğiniz bitiş noktasını seçin. Ortaya çıkan **Bitiş Noktası** sayfasından kaynağı seçin.  

![Kaynağı vurgulanan bitiş noktası sayfası](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Origin** sayfası görüntülenir. 

![Kaynak sayfası](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Kaynak türü ve ana bilgisayar adı
Başlangıç türü ve **Başlangıç** **ana bilgisayar adının** değerlerinin doğru olduğunu doğrulayın. Bu örnekte,\/https: /cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, URL'nin ana bilgisayar adı bölümü *cdndocdemo.blob.core.windows.net*, doğrudur. Azure Depolama, Web Uygulaması ve Bulut Hizmeti kökenliler, Origin **ana bilgisayar adı** alanı için açılır liste değeri kullandığından, yanlış yazımlar sorun değildir. Ancak, özel bir kaynak kullanıyorsanız, ana bilgisayar adınızın doğru yazıldığından emin olun.

#### <a name="http-and-https-ports"></a>HTTP ve HTTPS bağlantı noktaları
**HTTP** ve **HTTPS bağlantı noktalarınızı**kontrol edin. Çoğu durumda, 80 ve 443 doğrudur ve hiçbir değişiklik gerektirmez.  Ancak, kaynak sunucusu farklı bir bağlantı noktasında dinliyorsa, burada temsil edilmesi gerekir. Emin değilseniz, kaynak dosyanızın URL'sini görüntüleyin. HTTP ve HTTPS belirtimleri varsayılan olarak 80 ve 443 bağlantı noktalarını kullanır. Örnek URL'de,\/https: /cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, bir bağlantı noktası belirtilmez, bu nedenle varsayılan 443 varsayılır ve ayarlar doğrudur.  

Ancak, daha önce sınadığınız kaynak dosyasının URL'sinin http olduğunu varsayalım:\//www.contoso.com:8080/file.txt. Ana bilgisayar kesiminin sonundaki *:8080* bölümüne dikkat edin. Bu numara tarayıcıya www\.contoso.com'daki web sunucusuna bağlanmak için 8080 numaralı bağlantı noktasını kullanmasını bildirir, bu nedenle HTTP bağlantı **noktası** alanına *8080* girmeniz gerekir. Bu bağlantı noktası ayarlarının yalnızca bitiş noktasının kaynaktan bilgi almak için kullandığı bağlantı noktasını etkilediğini unutmayın.

> [!NOTE]
> **Akamai uç noktalarından Azure CDN Standardı,** başlangıçlar için tam TCP bağlantı noktası aralığına izin vermez.  İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](/previous-versions/azure/mt757337(v=azure.100)).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Bitiş noktası ayarlarını kontrol edin
Bitiş **Noktası** **sayfasında, Yapıla** düğmesini seçin.

![Yapılandırma düğmesi vurgulanmış bitiş noktası sayfası](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

CDN bitiş noktası **yapılandırma** sayfası görüntülenir.

![Sayfayı yapılandırma](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoller
Protokoller için, **istemciler**tarafından kullanılan protokolün seçildiğini doğrulayın. İstemci tarafından kullanılan aynı protokol kaynağına erişmek için kullanıldığından, önceki bölümde kaynak bağlantı noktalarının doğru şekilde yapılandırılması önemlidir. CDN bitiş noktası, kaynak bağlantı noktalarından bağımsız olarak yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarında (80 ve 443) dinler.

Http:\//www.contoso.com:8080/file.txt ile varsayımsal örneğimize dönelim.  Hatırlayacağınız gibi, Contoso *8080'i* HTTP bağlantı noktası olarak belirtmiştir, ancak https bağlantı noktası olarak *44300'ü* belirttiklerini varsayalım.  *Onlar contoso*adlı bir bitiş noktası oluşturduysanız, cdn uç nokta ana bilgisayar adı *contoso.azureedge.net*olacaktır.  http için bir\/istek: /contoso.azureedge.net/file.txt bir HTTP isteğidir, bu nedenle bitiş noktası http bağlantı noktası 8080'i kullanarak orijinden alır.  HTTPS, https:\//contoso.azureedge.net/file.txt üzerinden güvenli bir istek, dosyanın kaynağından alırken 44300 bağlantı noktasında HTTPS'nin kullanılmasına neden olur.

#### <a name="origin-host-header"></a>Kaynak barındırma üst bilgisi
**Origin ana bilgisayar üstbilgisi,** her istekle birlikte menşee gönderilen ana bilgisayar üstbilgi değeridir.  Çoğu durumda, bu daha önce doğruladığımız **Origin ana bilgisayar adı** ile aynı olmalıdır.  Bu alandaki yanlış bir değer genellikle 404 duruma neden olmaz, ancak kaynağın ne beklediğine bağlı olarak diğer 4xx durumlarına neden olabilir.

#### <a name="origin-path"></a>Kaynak yolu
Son olarak, Bizim **Origin yolu**doğrulamak gerekir.  Varsayılan olarak bu boştur.  Bu alanı yalnızca CDN'de kullanılabilir hale getirmek istediğiniz kaynak barındırılan kaynakların kapsamını daraltmak istiyorsanız kullanmalısınız.  

Örnek bitiş noktasında, depolama hesabındaki tüm kaynakların kullanılabilir olmasını istedik, bu nedenle **Başlangıç yolu** boş bırakıldı.  Bu, https için bir\/istek anlamına gelir: /cdndocdemo.azureedge.net/publicblob/lorem.txt bir bağlantı ile sonuçlanır bitiş noktasından cdndocdemo.core.windows.net bu istekleri */publicblob/lorem.txt*.  Aynı şekilde, https için\/bir istek: /cdndocdemo.azureedge.net/donotcache/status.png uç noktası */donotcache/status.png* kaynaktan isteyen sonuçlanır.

Peki ya CDN'yi kökeninizdeki her yol için kullanmak istemiyorsanız?  Sadece *publicblob* yolunu ortaya çıkarmak istediğini söyle.  **Origin yol** alanına */publicblob* girersek, menşeine yapılan her istek ten önce bitiş noktasının */publicblob'u* eklemesine neden olur.  Bu, https için istek\/anlamına gelir: /cdndocdemo.azureedge.net/publicblob/lorem.txt şimdi aslında URL istek kısmını alacak, */ publicblob/lorem.txt*, ve baş */ publicblob* ekleyecek. Bu da */publicblob/publicblob/lorem.txt* için bir istekle sonuçlanır.  Bu yol gerçek bir dosyaya çözülmezse, kaynak 404 durumunu döndürür.  Bu örnekte lorem.txt almak için doğru URL\/aslında https olacaktır: /cdndocdemo.azureedge.net/lorem.txt.  URL'nin istek bölümü */lorem.txt* olduğundan ve bitiş noktası */publicblob*eklenerek */publicblob/lorem.txt* isteğinin menşeine geçtiği için */publicblob* yolunu hiç eklemediğimizi unutmayın.

