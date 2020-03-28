---
title: Azure portalında Azure Veri Kutusu Kenarı aygıtına bağlanma, yapılandırma ve etkinleştirme için öğretici | Microsoft Dokümanlar
description: Data Box Edge dağıtmak için öğretici bağlanmak için talimat, kurmak ve fiziksel cihazınızı etkinleştirmek.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 144c59c8bc24e8e10584702ec6cd48f7aa8c15c1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239059"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge"></a>Öğretici: Azure Veri Kutusu Kenarı'nı bağlayın, ayarlayın ve etkinleştirin 

Bu öğretici, yerel web ui'sini kullanarak Azure Veri Kutusu Edge aygıtınıza nasıl bağlanabileceğinizi, ayarlayabileceğinizi ve etkinleştirebileceğinizi açıklar.

Kurulum ve etkinleştirme işleminin tamamlanması yaklaşık 20 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Fiziksel bir cihaza bağlanma
> * Fiziksel aygıtı ayarlama ve etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Veri Kutusu Kenar aygıtınızı yapılandırmadan ve ayarlamadan önce şunları yapın:

* Fiziksel aygıtı [Veri Kutusu Kenarını Yükle'de](data-box-edge-deploy-install.md)ayrıntılı olarak yükledin.
* Veri Kutusu Kenarı aygıtını yönetmek için oluşturduğunuz Veri Kutusu Kenarı hizmetinden etkinleştirme anahtarına sahipsiniz. Daha fazla bilgi için [Azure Veri Kutusu Kenarı dağıtmaya hazırla'ya](data-box-edge-deploy-prep.md)gidin.

## <a name="connect-to-the-local-web-ui-setup"></a>Connect to the local web UI setup 

1. Bilgisayarınızdaki Ethernet bağdaştırıcısını, 192.168.100.5 statik IP adresi ve 255.255.255.0 alt ağıyla Veri Kutusu Edge aygıtına bağlanacak şekilde yapılandırın.

2. Aygıtınızdaki bilgisayarı PORT 1'e bağlayın. Cihazınızdaki PORT 1'i tanımlamak için aşağıdaki çizimi kullanın.

    ![Kabloları takılmış bir cihazın arka yüzü](./media/data-box-edge-deploy-install/backplane-cabled.png)


3. Bir tarayıcı penceresi açın ve cihazın yerel web `https://192.168.100.10`ui adresine erişin.  
    Bu eylem, aygıtı açtıktan sonra birkaç dakika sürebilir. 

    Web sitesinin güvenlik sertifikasında bir sorun olduğunu belirten bir hata veya uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

4. **Bu web sayfasına Devam Et'i**seçin.  
    Bu adımlar kullandığınız tarayıcıya bağlı olarak değişebilir.

5. Cihazınızın web arama sürümünde oturum açın. Varsayılan parola *Password1'dir.* 
   
    ![Veri Kutusu Kenarı cihaz oturum açma sayfası](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

6. İsteyicide aygıt yöneticisi parolasını değiştirin.  
    Yeni parola 8 ile 16 karakter arasında olmalıdır. Aşağıdaki karakterlerden üçünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.

Artık cihazınızın panosundasınız.

## <a name="set-up-and-activate-the-physical-device"></a>Fiziksel aygıtı ayarlama ve etkinleştirme
 
Panonuz, fiziksel aygıtı Veri Kutusu Kenarı hizmetiyle yapılandırmak ve kaydetmek için gereken çeşitli ayarları görüntüler. **Aygıt adı,** **Ağ ayarları,** **Web proxy ayarları**ve **Zaman ayarları** isteğe bağlıdır. Gerekli tek ayarlar **Bulut ayarlarıdır.**
   
![Yerel web Web Web UI "Pano" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Sol bölmede Aygıt **adını**seçin ve ardından cihazınız için uygun bir ad girin.  
    Dost adı 1 ila 15 karakter içermelidir ve harfler, sayılar ve tireler olmalıdır.

    ![Yerel web kullanıcı arabirimi "Aygıt adı" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. (İsteğe bağlı) Sol bölmede Ağ **ayarlarını** seçin ve ardından ayarları yapılandırın.  
    Fiziksel aygıtınızda altı ağ arabirimi vardır. PORT 1 ve PORT 2 1-Gbps ağ arabirimleridir. PORT 3, PORT 4, PORT 5 ve PORT 6, 10 Gbps ağ arabirimi olarak da hizmet verebilen 25 Gbps ağ arabirimleridir. PORT 1 otomatik olarak yalnızca yönetim bağlantı noktası olarak yapılandırılır ve PORT 2 ile PORT 6 tüm veri bağlantı noktalarıdır. **Ağ ayarları** sayfası aşağıda gösterildiği gibi.
    
    ![Yerel web UI "Ağ ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Ağ ayarlarını yapılandırırken şunları unutmayın:

   - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
   - DHCP etkin değilse, gerekirse statik IP'ler atayabilirsiniz.
   - Ağ arabiriminizi IPv4 olarak yapılandırabilirsiniz.

     >[!NOTE] 
     > Cihaza bağlanacak başka bir IP adresiniz yoksa, ağ arabiriminin yerel IP adresini statikten DCHP'ye değiştirmemenizi öneririz. Tek bir ağ arabirimi kullanırsanız ve DHCP'ye geçerseniz, DHCP adresini belirlemenin bir yolu yoktur. Bir DHCP adresine geçiş yapmak istiyorsanız, aygıt hizmete kaydolana kadar bekleyin ve sonra değiştirin. Daha sonra hizmetiniz için Azure portalındaki **Aygıt özelliklerindeki** tüm bağdaştırıcıların IP'lerini görüntüleyebilirsiniz.

3. (İsteğe bağlı) Sol **bölmede, Web proxy ayarlarını**seçin ve ardından web proxy sunucunuzu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca bu sayfada yapılandırabilirsiniz.
   
   ![Yerel web Web UI "Web proxy ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Web **proxy ayarları** sayfasında aşağıdakileri yapın:
   
   a. Web **proxy URL** kutusuna URL'yi bu `http://host-IP address or FQDN:Port number`biçimde girin: . HTTPS URL'leri desteklenmez.

   b. **Kimlik Doğrulama**altında, **Yok** veya **NTLM'yi**seçin.

   c. Kimlik doğrulaması kullanıyorsanız, bir kullanıcı adı ve parola girin.

   d. Yapılandırılan web proxy ayarlarını doğrulamak ve uygulamak **için Ayarları Uygula'yı**seçin.
   
   > [!NOTE]
   > Proxy-auto config (PAC) dosyaları desteklenmez. PAC dosyası, web tarayıcılarının ve diğer kullanıcı aracılarının belirli bir URL'yi almak için uygun proxy sunucusunu (erişim yöntemi) otomatik olarak nasıl seçebileceğini tanımlar.
   > Proxy'nin sertifikasına güvenilmediğinden, tüm trafiği durdurmaya ve okumaya çalışan proxy'ler (sonra her şeyi kendi sertifikalarıyla yeniden imzalama) uyumlu değildir.
   > Genellikle saydam yakınlıklar Azure Veri Kutusu Kenarı ile iyi çalışır.

4. (İsteğe bağlı) Sol bölmede Saat **ayarlarını**seçin ve ardından cihazınızın saat dilimini ve birincil ve ikincil NTP sunucularını yapılandırın.  
    NTP sunucuları gereklidir, çünkü cihazınızın bulut hizmeti sağlayıcılarınızla kimlik doğrulaması yapabilmesi için zamanı eşitlemesi gerekir.
       
    Saat **ayarları** sayfasında aşağıdakileri yapın:
    
    1. Saat **dilimi** açılır listesinde, aygıtın dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        CihazınızIçin varsayılan saat dilimi PST'dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. Birincil **NTP sunucu** kutusuna, aygıtınızın birincil sunucusunu girin veya time.windows.com varsayılan değerini kabul edin.  
        Ağınızın NTP trafiğinin veri merkezinizden internete geçmesine izin verdiğinden emin olun.

    3. İsteğe bağlı olarak, **İkincil NTP sunucu** kutusuna cihazınız için ikincil bir sunucu girin.

    4. Yapılandırılan zaman ayarlarını doğrulamak ve uygulamak **için Ayarları Uygula'yı**seçin.

        ![Yerel web UI "Saat ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. (İsteğe bağlı) Sol bölmede, aygıtınızdaki depolama esnekliğini yapılandırmak için **Depolama ayarlarını** seçin. Bu özellik şu anda önizleme sürümündedir. Varsayılan olarak, aygıttaki depolama esnek değildir ve aygıtta bir veri diski arızalanırsa veri kaybı olur. Esnek seçeneğini etkinleştirdiğinizde, aygıttaki depolama alanı yeniden yapılandırılır ve aygıt veri kaybı olmadan bir veri diskinin arızasına dayanabilir. Depolamayı esnek olarak yapılandırılsın, cihazınızın kullanılabilir kapasitesini azaltır.

    > [!IMPORTANT] 
    > Esneklik yalnızca aygıtı etkinleştirmeden önce yapılandırılabilir. 

    ![Yerel web Kullanıcı Birası "Depolama ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/storage-settings.png)

6. Sol bölmede Bulut **ayarlarını**seçin ve ardından Azure portalındaki Veri Kutusu Kenarı hizmetiyle cihazınızı etkinleştirin.
    
    1. **Etkinleştirme anahtar** kutusuna, Veri Kutusu Kenarı için [etkinleştirme anahtarını al'da](data-box-edge-deploy-prep.md#get-the-activation-key) aldığınız etkinleştirme anahtarını girin.
    2. **Uygula**’yı seçin.
       
        ![Yerel web UI "Bulut ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. Önce cihaz etkinleştirilir. Aygıt daha sonra kritik güncelleştirmeler için taranır ve varsa güncelleştirmeler otomatik olarak uygulanır. Bu yönde bir bildirim görüyorsunuz.

        İletişim kutusunda ayrıca kopyalamanız ve güvenli bir yere kaydetmeniz gereken bir kurtarma anahtarı da vardır. Bu anahtar, aygıtın önyükleme yapamaması durumunda verilerinizi kurtarmak için kullanılır.

        ![Yerel web UI "Bulut ayarları" sayfası güncellendi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. Güncelleştirme başarıyla tamamlandıktan sonra birkaç dakika beklemeniz gerekebilir. Sayfa, aygıtın başarıyla etkinleştirildiğini belirtmek için güncellenir.

        ![Yerel web UI "Bulut ayarları" sayfası güncellendi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-8.png)

Aygıt kurulumu tamamlandı. Artık cihazınıza paylaşımlar ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Fiziksel bir cihaza bağlanma
> * Fiziksel aygıtı ayarlama ve etkinleştirme

Veri Kutusu Edge aygıtınızla nasıl veri aktarılatırınızı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Veri Kutusu Kenarı ile veri aktarımı.](./data-box-edge-deploy-add-shares.md)
