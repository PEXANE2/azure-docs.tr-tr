---
title: Sorun giderme oturumu afinite sorunları
titleSuffix: Azure Application Gateway
description: Bu makalede, Azure Uygulama Ağ Geçidi'nde oturum afinite sorunlarının nasıl giderilene ilişkin bilgiler verilmektedir
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: aa3617b30fe1ef9b4d4a6c5fe5aac51bff95bb92
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866685"
---
# <a name="troubleshoot-azure-application-gateway-session-affinity-issues"></a>Azure Uygulama Ağ Geçidi oturumu afinite sorunları

Azure Application Gateway ile oturum afinite sorunlarını nasıl tanılayıp çözeceğinizi öğrenin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel bakış

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

## <a name="possible-problem-causes"></a>Olası sorun nedenleri

Çerez tabanlı oturum afinitesini koruma sorunu aşağıdaki ana nedenlerden dolayı oluşabilir:

- "Çerez tabanlı Affinity" ayarı etkinleştirildi
- Uygulamanız çerez tabanlı yakınlığı kaldıramaz
- Uygulama çerez tabanlı yakınlık kullanıyor, ancak istekler hala arka uç sunucular arasında sıçrayan

### <a name="check-whether-the-cookie-based-affinity-setting-is-enabled"></a>"Çerez tabanlı Affinity" ayarının etkin olup olmadığını kontrol edin

Bazen "Çerez tabanlı yakınlık" ayarını etkinleştirmeyi unuttuğunızda oturum yakınlığı sorunları oluşabilir. Azure portalındaki HTTP Ayarlar sekmesindeki "Çerez tabanlı yakınlık" ayarını etkinleştirip etkinleştirmediğinizi belirlemek için yönergeleri izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Sol **gezinti** bölmesinde Tüm **kaynaklar'ı**tıklatın. Tüm kaynaklar bıçak uygulama ağ geçidi adını tıklatın. Seçtiğiniz aboneliğin içinde zaten birkaç kaynak varsa, Filtre'ye uygulama ağ geçidi adını **ada göre girebilirsiniz...** girebilirsiniz.

3. **AYARLAR'ın**altındaki **HTTP ayarları** sekmesini seçin.

   ![sorun giderme-oturum-yakınlık-sorunlar-1](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-1.png)

4. Sağ taraftaki **AppGatewayBackendAyarlar'ı** tıklayarak Çerez için **Etkin** tabanlı yakınlığı seçip seçmediğinizi kontrol edin.

   ![sorun giderme-oturum-yakınlık-sorunlar-2](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-2.jpg)



Ayrıca aşağıdaki yöntemlerden birini kullanarak "**BackendHttpSettingsCollection**" altında *Etkin*olarak ayarlanmış olan "**CookieBasedAffinity**" değerini de kontrol edebilirsiniz:

- [PowerShell'de Get-AzApplicationGatewayBackend'i](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhttpsetting) Çalıştır
- Azure Kaynak Yöneticisi şablonu kullanarak JSON dosyasına bakın

```
"cookieBasedAffinity": "Enabled", 
```

### <a name="the-application-cannot-handle-cookie-based-affinity"></a>Uygulama çerez tabanlı yakınlık işleyemez

#### <a name="cause"></a>Nedeni

Uygulama ağ geçidi yalnızca bir çerez kullanarak oturum tabanlı yakınlık gerçekleştirebilir.

#### <a name="workaround"></a>Geçici çözüm

Uygulama çerez tabanlı yakınlığı işleyemiyorsa, harici veya dahili azure yük dengeleyicisi veya başka bir üçüncü taraf çözümü kullanmanız gerekir.

### <a name="application-is-using-cookie-based-affinity-but-requests-still-bouncing-between-back-end-servers"></a>Uygulama çerez tabanlı yakınlık kullanıyor, ancak istekler hala arka uç sunucular arasında sıçrayan

#### <a name="symptom"></a>Belirti

Örneğin, Internet Explorer'da kısa bir ad URL'si kullanarak Uygulama Ağ Geçidi'ne erişdiğinizde Çerez tabanlı Affinity ayarını etkinleştirdiniz: [http://website](http://website/) , istek hala arka uç sunucular arasında sıçrayıyor.

Bu sorunu tanımlamak için yönergeleri izleyin:

1. Uygulama Ağ Geçidi'nin arkasındaki uygulamaya bağlanan "İstemci" üzerinde bir web hata ayıklayıcı izini alın (Bu örnekte Fiddler kullanıyoruz).
    **İpucu** Fiddler'ı nasıl kullanacağınızı bilmiyorsanız, alttaki **"Ağ trafiğini toplamak ve web hata ayıklayıcısını kullanarak analiz etmek istiyorum"** seçeneğini işaretleyin.

2. İstemci tarafından sağlanan tanımlama bilgilerinin ARRAffinity ayrıntılarına sahip olup olmadığını belirlemek için oturum günlüklerini kontrol edin ve analiz edin. Çerez kümesiiçinde "**ARRAffinity=** *ARRAffinityValue*" gibi ARRAffinity ayrıntılarını bulamazsanız, istemci Ninder çerezi ile yanıtlamadığı anlamına gelir, bu uygulama ağ geçidi tarafından sağlanan.
    Örneğin:

    ![sorun giderme-oturum-yakınlık-sorunlar-3](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-3.png)

    ![sorun giderme-oturum-yakınlık-sorunlar-4](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-4.png)

Uygulama, yanıt alana kadar her istek te çerezi ayarlamaya devam eder.

#### <a name="cause"></a>Nedeni

Bu sorun, Internet Explorer ve diğer tarayıcıların çerezi kısa bir ad URL'si olan çerezi depolayamayabileceği veya kullanamayabileceğinden oluşur.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için Application Gateway'e FQDN kullanarak erişmeniz gerekir. Örneğin, kullanın [http://website.com](https://website.com/) [http://appgw.website.com](http://website.com/) veya .

## <a name="additional-logs-to-troubleshoot"></a>Sorun gidermek için ek günlükler

Çerez tabanlı oturum afinitesini gidermek için ek günlükler toplayabilir ve bunları analiz edebilirsiniz

### <a name="analyze-application-gateway-logs"></a>Uygulama Ağ Geçidi günlüklerini analiz et

Uygulama Ağ Geçidi günlüklerini toplamak için yönergeleri izleyin:

Azure portaldan günlüğe kaydetmeyi etkinleştirme

1. Azure [portalında,](https://portal.azure.com/)kaynağınızı bulun ve **tanıgünlüklerini**tıklatın.

   Uygulama Ağ Geçidi için üç günlük mevcuttur: Erişim günlüğü, Performans günlüğü, Güvenlik Duvarı günlüğü

2. Veri toplamaya başlamak için **tanılamayı aç'ı**tıklatın.

   ![sorun giderme-oturum-yakınlık-sorunlar-5](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-5.png)

3. **Tanılama ayarları** blade tanılama günlükleri için ayarları sağlar. Bu örnekte, Log Analytics günlükleri depolar. Çalışma alanınızı ayarlamak için **Günlük Analitiği** altında **Yapılandır'ı** tıklatın. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.

   ![sorun giderme-oturum-yakınlık-sorunlar-6](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-6.png)

4. Ayarları onaylayın ve sonra **Kaydet'i**tıklatın.

   ![sorun giderme-oturum-yakınlık-sorunlar-7](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-7.png)

#### <a name="view-and-analyze-the-application-gateway-access-logs"></a>Uygulama Ağ Geçidi erişim günlüklerini görüntüleme ve çözümleme

1. Uygulama Ağ Geçidi kaynak görünümü altındaki Azure portalında, **İzleme** bölümünde **Tanılama günlüklerini** seçin.

   ![sorun giderme-oturum-yakınlık-sorunlar-8](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-8.png)

2. Sağ tarafta, **Log kategorileri** altındaki açılır listede "**ApplicationGatewayAccessLog**" seçeneğini belirleyin.  

   ![sorun giderme-oturum-yakınlık-sorunlar-9](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-9.png)

3. Uygulama Ağ Geçidi Erişim Günlüğü listesinde, çözümlemek ve dışa aktarmak istediğiniz günlüğü tıklatın ve ardından JSON dosyasını dışa aktarın.

4. Adım 3'te dışa aktardığınız JSON dosyasını CSV dosyasına dönüştürün ve bunları Excel, Power BI veya başka bir veri görselleştirme aracında görüntüleyin.

5. Aşağıdaki verileri kontrol edin:

- **ClientIP**– Bu, bağlanan istemcinin istemci IP adresidir.
- **ClientPort** - Bu, istek için bağlantı istemcisinden gelen kaynak bağlantı noktasıdır.
- **RequestQuery** – Bu, isteğin alındığı hedef sunucuyu gösterir.
- **Server-Routed**: İsteğin alındığı arka uç havuzu örneği.
- **X-AzureApplicationGateway-LOG-ID**: İstek için kullanılan Korelasyon Kimliği. Arka uç sunucularında trafik sorunlarını gidermek için kullanılabilir. Örneğin: X-AzureApplicationAğ Geçidi-CACHE-HIT=0&SERVER-ROUTED=10.0.2.4.

  - **SERVER-STATUS**: Application Gateway'in arka uçtan aldığı HTTP yanıt kodu.

  ![sorun giderme-oturum-yakınlık-sorunlar-11](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-11.png)

Aynı ClientIP ve Client Port'tan iki öğenin geldiğini görürseniz ve aynı arka uç sunucusuna gönderilirlerse, bu Uygulama Ağ Geçidi'nin doğru şekilde yapılandırıldığı anlamına gelir.

Aynı ClientIP ve Client Port'tan iki öğenin geldiğini ve farklı arka uç sunucularına gönderildiğini görürseniz, bu isteğin arka uç sunucuları arasında sıçraydığı anlamına gelir, "Uygulama çerez tabanlı yakınlık kullanıyor, ancak sorun gidermek için alttaki**arka uç sunucular arasında zıplayan istekler**" seçeneğini belirleyin.

### <a name="use-web-debugger-to-capture-and-analyze-the-http-or-https-traffics"></a>HTTP veya HTTPS trafiğini yakalamak ve analiz etmek için web hata ayıklayıcısını kullanın

Fiddler gibi web hata ayıklama araçları, Internet ve test bilgisayarları arasındaki ağ trafiğini yakalayarak web uygulamalarını hata ayıklamanıza yardımcı olabilir. Bu araçlar, tarayıcı bunları alır/gönderirken gelen ve giden verileri incelemenize olanak tanır. Bu örnekte Fiddler, özellikle kimlik doğrulama türünde bir sorun için, web uygulamalarıyla ilgili istemci tarafındaki sorunları gidermenize yardımcı olabilecek HTTP yeniden oynatma seçeneğine sahiptir.

Seçtiğiniz web hata ayıklama kullanın. Bu örnekte, http veya https trafiğini yakalamak ve analiz etmek için Fiddler'ı kullanacağız, talimatları uygulayacağız:

1. Fiddler aracını <https://www.telerik.com/download/fiddler>buradan indirin.

    > [!NOTE]
    > Yakalama bilgisayarı .NET 4 yüklüyse Fiddler4'u seçin. Aksi takdirde, Fiddler2'yi seçin.

2. Çalıştırılabilir kurulumsağ tıklatın ve yüklemek için yönetici olarak çalıştırın.

    ![sorun giderme-oturum-yakınlık-sorunlar-12](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-12.png)

3. Fiddler'ı açtığınızda, otomatik olarak trafiği yakalamaya başlamalıdır (sol alt köşedeki Yakalama'ya dikkat edin). Trafik yakalamayı başlatmak veya durdurmak için F12 tuşuna basın.

    ![sorun giderme-oturum-yakınlık-sorunlar-13](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-13.png)

4. Büyük olasılıkla, şifresi çözülmüş HTTPS trafiğiyle ilgileneceksiniz ve **Araçlar** > **Fiddler Seçenekleri'ni**seçerek HTTPS şifresini çözmeyi etkinleştirebilir ve " **Şifreyi Çöz HTTPS trafiğini**çöz" kutusunu işaretleyebilirsiniz.

    ![sorun giderme-oturum-yakınlık-sorunlar-14](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-14.png)

5. **X** (simge) > **Tümlerini** Ekran Görüntüsü olarak Kaldır'ı tıklatarak sorunu çoğaltmadan önce önceki ilgisiz oturumları kaldırabilirsiniz: 

    ![sorun giderme-oturum-yakınlık-sorunlar-15](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-15.png)

6. Sorunu yeniden çoğalttınktan sonra, **Dosya** > Tüm Oturumları**Kaydet'i** > seçerek dosyayı gözden geçirilmeniz için kaydedin.**All Sessions..** 

    ![sorun giderme-oturum-yakınlık-sorunlar-16](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-16.png)

7. Sorunun ne olduğunu belirlemek için oturum günlüklerini denetleyin ve çözümleyin.

    Örnekler için:

- **Örnek A:** İsteğin istemciden gönderildiğibir oturum günlüğü bulursunuz ve bu günlük Uygulama Ağ Geçidi'nin genel IP adresine gider, ayrıntıları görüntülemek için bu günlüğü tıklatın.  Sağ tarafta, alt kutudaki veriler Uygulama Ağ Geçidi istemciye geri dönüyor. "RAW" sekmesini seçin ve istemcinin "**Set-Cookie: ARRAffinity=** *ARRAffinityValue"* alıp almadığını belirleyin. Çerez yoksa, oturum yakınlığı ayarlı değildir veya Uygulama Ağ Geçidi istemciye çerezi geri uygulamıyor.

   > [!NOTE]
   > Bu ARRAffinity değeri, uygulama ağ geçidinin istemcinin belirli bir arka uç sunucusuna gönderilmesini ayarlayan çerez kimliğidir.

   ![sorun giderme-oturum-yakınlık-sorunlar-17](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-17.png)

- **Örnek B:** Bir öncekinin izlediği sonraki oturum günlüğü, ARRAAFFINITY'yi ayarlayan Uygulama Ağ Geçidi'ne yanıt veren istemcidir. ARRAffinity çerez kimliği eşleşirse, paket daha önce kullanılan aynı arka uç sunucusuna gönderilmelidir. Müşterinin ARRAffinity çerezinin değişip değişmediğini görmek için sonraki birkaç http iletişim satırını kontrol edin.

   ![sorun giderme-oturum-yakınlık-sorunlar-18](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-18.png)

> [!NOTE]
> Aynı iletişim oturumu için çerez değişmemelidir. Sağ taraftaki üst kutuyu işaretleyin, istemcinin çerezi kullanıp kullanmadığını ve Uygulama Ağ Geçidi'ne geri gönderip göndermediğini görmek için "Çerezler" sekmesini seçin. Değilse, istemci tarayıcı sıyrık ve konuşmalar için çerez kullanmaz. Bazen, müşteri yalan söyleyebilir.

 

## <a name="next-steps"></a>Sonraki adımlar

Önceki adımlar sorunu çözmezse, bir [destek bileti](https://azure.microsoft.com/support/options/)açın.
