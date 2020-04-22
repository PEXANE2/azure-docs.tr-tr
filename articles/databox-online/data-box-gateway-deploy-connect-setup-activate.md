---
title: Azure portalında Azure Veri Kutusu Ağ Geçidine bağlanma, yapılandırma ve etkinleştirme
description: Veri Kutusu Ağ Geçidi'ni dağıtmanın üçüncü öğreticisi, sanal aygıtınızı bağlamanızı, kurmanızı ve etkinleştirmenizi sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: alkohli
ms.openlocfilehash: b3cf4fd958202c28586b7c15932dc88a21d7c60f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686872"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway"></a>Öğretici: Azure Veri Kutusu Ağ Geçidine bağlanma, ayarlama, etkinleştirme

## <a name="introduction"></a>Giriş

This tutorial describes how to connect to, set up, and activate your Data Box Gateway device by using the local web UI. 

Kurulum ve etkinleştirme işleminin tamamlanması yaklaşık 10 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal bir cihaza bağlanma
> * Sanal aygıtı ayarlama ve etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Veri Kutusu Ağ Geçidinizi yapılandırmadan ve ayarlamadan önce şunları yapın:

* Sanal bir aygıt saðladınız ve [hyper-v'deki Veri Kutusu Ağ Geçidi](data-box-gateway-deploy-provision-hyperv.md) hükmünde veya [VMware'de Veri Kutusu Ağ Geçidi Sağlama'da](data-box-gateway-deploy-provision-vmware.md)ayrýntýlý olarak ona baðlý bir URL elde ettiniz.
* Veri Kutusu Ağ Geçidi aygıtlarını yönetmek için oluşturduğunuz Veri Kutusu Ağ Geçidi hizmetinden etkinleştirme anahtarına sahipsiniz. Daha fazla bilgi için [Azure Veri Kutusu Ağ Geçidi'ni dağıtmaya hazırla'ya](data-box-gateway-deploy-prep.md)gidin.


## <a name="connect-to-the-local-web-ui-setup"></a>Connect to the local web UI setup 

1. Bir tarayıcı penceresi açın ve aygıtın yerel web ui adresine şu anda erişin:
   
   https:\//ip-adres-ağ-arayüz
   
   Önceki öğreticide belirtilen bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında bir sorun olduğunu belirten bir hata veya uyarı görürsünüz.

2. **Bu web sayfasına Devam Et'i**seçin. Bu adımlar kullandığınız tarayıcıya bağlı olarak değişebilir.
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

3. Sanal cihazınızın web ui'sinde oturum açın. Varsayılan parola *Password1'dir.* 
   
    ![Yerel web UI'da oturum açın](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

4. İsteyişte aygıt parolasını değiştirin. Yeni parola 8 ile 16 karakter arasında olmalıdır. Aşağıdakilerden 3'ü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.

    ![Cihaz parolasını değiştirme](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

Artık cihazınızın **Panosundasınız.**

## <a name="set-up-and-activate-the-virtual-device"></a>Sanal aygıtı ayarlama ve etkinleştirme
 
Panonuz, sanal aygıtı Veri Kutusu Ağ Geçidi hizmetine yapılandırmak ve kaydetmek için gereken çeşitli ayarları görüntüler. **Aygıt adı,** **Ağ ayarları,** **Web proxy ayarları**ve **Zaman ayarları** isteğe bağlıdır. Gerekli tek ayarlar **Bulut ayarlarıdır.**
   
![Yerel web Web Web UI "Pano" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

1. Sol bölmede **Aygıt adını**seçin ve ardından cihazınız için uygun bir ad girin. Dostu ad 1 ila 15 karakter uzunluğunda ve harf, sayı ve tire içermelidir.

    ![Yerel web kullanıcı arabirimi "Aygıt adı" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

2. (İsteğe bağlı) Sol bölmede **Ağ ayarlarını** seçin ve ardından ayarları yapılandırın. Sanal aygıtınızda, altta yatan sanal makinede kaç tane yapılandırdığınıza bağlı olarak en az bir ağ arabirimi ve daha fazlasını görürsünüz. Bir ağ arabirimi etkin leştirilmiş bir sanal aygıtın **Ağ ayarları** sayfası aşağıda gösterildiği gibi.
    
    ![Yerel web UI "Ağ ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Ağ ayarlarını yapılandırırken şunları unutmayın:

    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP'ler atayabilirsiniz.
    - Ağ arabiriminizi IPv4 olarak yapılandırabilirsiniz.

     >[!NOTE] 
     > Cihaza bağlanacak başka bir IP adresiniz yoksa, ağ arabiriminin yerel IP adresini statikten DHCP'ye değiştirmemenizi öneririz. Tek bir ağ arabirimi kullanırsanız ve DHCP'ye geçerseniz, DHCP adresini belirlemenin bir yolu yoktur. Bir DHCP adresine geçiş yapmak istiyorsanız, aygıt hizmete kaydolana kadar bekleyin ve sonra değiştirin. Daha sonra hizmetiniz için Azure portalındaki **Aygıt özelliklerindeki** tüm bağdaştırıcıların IP'lerini görüntüleyebilirsiniz.

3. (İsteğe bağlı), web ara sunucusunu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca bu sayfada yapılandırabilirsiniz.
   
   ![Yerel web Web UI "Web proxy ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   Web **proxy** sayfasında, aşağıdakileri yapın:
   
   1. Web **proxy URL** kutusuna URL'yi bu `http://&lt;host-IP address or FQDN&gt;:Port number`biçimde girin: . HTTPS URL'leri desteklenmez.
   2. **Kimlik Doğrulama**altında, **Yok** veya **NTLM'yi**seçin.
   3. Kimlik doğrulama kullanıyorsanız, bir **Kullanıcı Adı** ve **Parola**girin.
   4. Yapılandırılan web proxy ayarlarını doğrulamak ve uygulamak için **Uygula'yı**seçin.

   > [!NOTE]
   > Proxy-auto config (PAC) dosyaları desteklenmez. PAC dosyası, web tarayıcılarının ve diğer kullanıcı aracılarının belirli bir URL'yi almak için uygun proxy sunucusunu (erişim yöntemi) otomatik olarak nasıl seçebileceğini tanımlar.
   > Proxy'nin sertifikasına güvenilmediğinden, tüm trafiği durdurmaya ve okumaya çalışan proxy'ler (sonra her şeyi kendi sertifikalarıyla yeniden imzalama) uyumlu değildir.
   > Genellikle saydam yakınlıklar Azure Veri Kutusu Ağ Geçidi ile iyi çalışır.

4. (İsteğe bağlı) Sol bölmede Saat **ayarlarını**seçin ve ardından cihazınızın saat dilimini ve birincil ve ikincil NTP sunucularını yapılandırın. 

    NTP sunucuları gereklidir, çünkü cihazınızın bulut hizmeti sağlayıcılarınızla kimlik doğrulaması yapabilmesi için zamanı eşitlemesi gerekir.
    
    ![Yerel web UI "Saat ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    Saat **ayarları** sayfasında aşağıdakileri yapın:
    
    1. Saat **dilimi** açılır listesinde, aygıtın dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        CihazınızIçin varsayılan saat dilimi PST'dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. Aygıtınız için birincil **ntp sunucusu** belirtin `time.windows.com`veya varsayılan değerini kabul edin.   
        Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.

    3. İsteğe bağlı olarak, **İkincil NTP sunucu** kutusuna cihazınız için ikincil bir sunucu girin.

    4. Yapılandırılan zaman ayarlarını doğrulamak ve uygulamak için **Uygula'yı**seçin.

6. Sol bölmede Bulut **ayarlarını**seçin ve ardından Azure portalındaki Veri Kutusu Ağ Geçidi hizmetiyle cihazınızı etkinleştirin.
    
    1. **Etkinleştirme anahtar** kutusuna, Veri Kutusu Ağ Geçidi için [etkinleştirme anahtarını al'da](data-box-gateway-deploy-prep.md#get-the-activation-key) aldığınız **Etkinleştirme anahtarını** girin.

    2. **Etkinleştir'i**seçin.
       
         ![Yerel web UI "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. Aygıt etkinleştirilir ve varsa kritik güncelleştirmeler otomatik olarak uygulanır. Bu yönde bir bildirim görüyorsunuz. Azure portalı üzerinden güncelleştirme ilerlemesini izleyin.

        ![Yerel web UI "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)
        
        **İletişim kutusunda, güvenli bir konumda kopyalamanız ve kaydetmeniz gereken bir kurtarma anahtarı da vardır. Bu anahtar, aygıtın önyükleme yapamaması durumunda verilerinizi kurtarmak için kullanılır.**


    4. Güncelleştirmenin başarıyla tamamlanması için birkaç dakika beklemeniz gerekebilir. Güncelleştirme tamamlandıktan sonra aygıtta oturum açın. **Bulut ayarları** sayfası, aygıtın başarıyla etkinleştirildiğini belirtmek için güncellenir.

        ![Yerel web UI "Bulut ayarları" sayfası güncellendi](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

Aygıt kurulumu tamamlandı. Artık cihazınıza paylaşımlar ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Sanal bir cihaza bağlanma
> * Sanal aygıtı ayarlama ve etkinleştirme

Veri Kutusu Ağ Geçidinizle nasıl veri aktarılabildiğini öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Veri Kutusu Ağ Geçidi ile veri aktarımı.](./data-box-gateway-deploy-add-shares.md)
