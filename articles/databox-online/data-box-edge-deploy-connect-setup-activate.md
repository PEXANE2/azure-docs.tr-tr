---
title: Azure portal 'de Azure Data Box Edge cihaza bağlanmak, yapılandırmak, etkinleştirmek için öğretici | Microsoft Docs
description: Dağıtım Data Box Edge, fiziksel cihazınızı bağlanmanızı, ayarlamanıza ve etkinleştirmenizi sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 144c59c8bc24e8e10584702ec6cd48f7aa8c15c1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79239059"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge"></a>Öğretici: bağlanma, kurulum ve etkinleştirme Azure Data Box Edge 

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak Azure Data Box Edge cihazınızı nasıl bağlayabileceğinizi, ayarlayabileceğinizi ve etkinleştirebileceğiniz açıklanmaktadır.

Kurulum ve etkinleştirme işleminin tamamlanması 20 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Fiziksel bir cihaza bağlanma
> * Fiziksel cihazı ayarlama ve etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Data Box Edge cihazınızı yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Data Box Edge](data-box-edge-deploy-install.md)ayrıntılı olarak yüklediniz.
* Data Box Edge cihazını yönetmek için oluşturduğunuz Data Box Edge hizmetinden etkinleştirme anahtarınız vardır. Daha fazla bilgi için, [Azure Data Box Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md)bölümüne gidin.

## <a name="connect-to-the-local-web-ui-setup"></a>Yerel Web Kullanıcı arabirimi kurulumuna bağlanma 

1. Bilgisayarınızda Ethernet bağdaştırıcısını, 192.168.100.5 ve alt ağ 255.255.255.0 statik IP adresiyle Data Box Edge cihaza bağlanacak şekilde yapılandırın.

2. Bilgisayarınızı cihazınızda bağlantı noktası 1 ' e bağlayın. Cihazınızda bağlantı noktası 1 ' i tanımlamak için aşağıdaki çizimi kullanın.

    ![Kabloları takılmış bir cihazın arka yüzü](./media/data-box-edge-deploy-install/backplane-cabled.png)


3. Bir tarayıcı penceresi açın ve konumundaki `https://192.168.100.10`cihazın yerel Web Kullanıcı arabirimine erişin.  
    Bu eylem, cihazı etkinleştirdikten sonra birkaç dakika sürebilir. 

    Bir hata veya Web sitesinin güvenlik sertifikasıyla ilgili bir sorun olduğunu belirten bir uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

4. **Bu Web sayfasına devam et**' i seçin.  
    Bu adımlar, kullanmakta olduğunuz tarayıcıya bağlı olarak farklılık gösterebilir.

5. Cihazınızın Web Kullanıcı arabiriminde oturum açın. Varsayılan parola *Parola1*' dir. 
   
    ![Data Box Edge cihaz oturum açma sayfası](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

6. İstem sırasında cihaz yönetici parolasını değiştirin.  
    Yeni parola 8 ile 16 arasında karakter içermelidir. Şu karakterlerden üçünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.

Artık cihazınızın panosundan hazırsınız.

## <a name="set-up-and-activate-the-physical-device"></a>Fiziksel cihazı ayarlama ve etkinleştirme
 
Panonuz fiziksel cihazı Data Box Edge hizmetiyle yapılandırmak ve kaydetmek için gereken çeşitli ayarları görüntüler. **Cihaz adı**, **ağ ayarları**, **Web proxy ayarları**ve **zaman ayarları** isteğe bağlıdır. Yalnızca **bulut ayarları**gereklidir.
   
![Yerel Web Kullanıcı arabirimi "Pano" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Sol bölmede **Cihaz adı**' nı seçin ve ardından cihazınız için kolay bir ad girin.  
    Kolay ad, 1 ile 15 arasında karakter içermeli ve harf, rakam ve kısa çizgi içermelidir.

    ![Yerel Web Kullanıcı arabirimi "Cihaz adı" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. Seçim Sol bölmede **ağ ayarları** ' nı seçin ve ardından ayarları yapılandırın.  
    Fiziksel cihazınızda altı ağ arabirimi vardır. Bağlantı noktası 1 ve bağlantı noktası 2 1 GB/sn ağ arabirimlerdir. Bağlantı noktası 3, bağlantı noktası 4, bağlantı noktası 5 ve bağlantı noktası 6, 10 Gbps ağ arabirimi olarak da kullanılabilen tüm 25 Gbps ağ arabirimlerdir. BAĞLANTı noktası 1 otomatik olarak yalnızca yönetim bağlantı noktası olarak yapılandırılır ve bağlantı noktası 6 ' dan bağlantı noktası 6 ' dan tüm veri bağlantı noktaları bulunur. **Ağ ayarları** sayfası aşağıda gösterildiği gibidir.
    
    ![Yerel Web Kullanıcı arabirimi "ağ ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Ağ ayarlarını yapılandırırken şunları göz önünde bulundurun:

   - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
   - DHCP etkinleştirilmemişse, gerekirse statik IP 'Ler atayabilirsiniz.
   - Ağ arabiriminizi IPv4 olarak yapılandırabilirsiniz.

     >[!NOTE] 
     > Cihaza bağlanmak için başka bir IP adresiniz yoksa, ağ arabiriminin yerel IP adresini static 'ten DCHP 'e geçmenizi öneririz. Bir ağ arabirimi kullanılıyorsa ve DHCP 'ye geçerseniz, DHCP adresini belirlemenin bir yolu yoktur. Bir DHCP adresine geçiş yapmak istiyorsanız, cihaz hizmete kaydolduktan sonra bekleyin ve sonra değiştirin. Daha sonra, hizmetinize yönelik Azure portal **cihaz özelliklerindeki** tüm bağdaştırıcıların IP 'lerini görüntüleyebilirsiniz.

3. Seçim Sol bölmede **Web proxy ayarları**' nı seçin ve ardından Web Proxy sunucunuzu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir Web Proxy kullanıyorsanız, bunu yalnızca bu sayfada yapılandırabilirsiniz.
   
   ![Yerel Web Kullanıcı arabirimi "Web proxy ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   **Web proxy ayarları** sayfasında, aşağıdakileri yapın:
   
   a. **Web proxy URL 'si** kutusuna URL 'yi şu biçimde girin: `http://host-IP address or FQDN:Port number`. HTTPS URL 'Leri desteklenmez.

   b. **Kimlik doğrulaması**altında **hiçbiri** veya **NTLM**' yi seçin.

   c. Kimlik doğrulaması kullanıyorsanız, bir Kullanıcı adı ve parola girin.

   d. Yapılandırılmış Web proxy ayarlarını doğrulamak ve uygulamak için **Ayarları Uygula**' yı seçin.
   
   > [!NOTE]
   > Proxy-otomatik yapılandırma (PAC) dosyaları desteklenmez. PAC dosyası, Web tarayıcılarının ve diğer Kullanıcı aracılarının belirli bir URL 'YI getirmek için uygun proxy sunucusunu (erişim yöntemi) otomatik olarak nasıl seçebileceğini tanımlar.
   > Ara sunucunun sertifikası güvenilir olmadığından, tüm trafiği kesmeye ve okumaya (sonra her şeyi kendi sertifikalarıyla yeniden imzalamaya) yönelik proxy 'ler uyumlu değildir.
   > Genellikle saydam proxy 'ler Azure Data Box Edge iyi çalışır.

4. Seçim Sol bölmede **zaman ayarları**' nı seçin ve ardından cihazınız için saat dilimini ve birincil ve ikincil NTP sunucularını yapılandırın.  
    Cihazınızın bulut hizmeti sağlayıcılarınız ile kimlik doğrulaması yapabilmesi için zaman eşitlemesini gerektiğinden NTP sunucuları gereklidir.
       
    **Zaman ayarları** sayfasında, aşağıdakileri yapın:
    
    1. **Saat dilimi** açılan listesinde, cihazın dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        Cihazınızın varsayılan saat dilimi PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. **BIRINCIL NTP sunucusu** kutusunda, cihazınızın birincil sunucusunu girin veya Time.Windows.com varsayılan değerini kabul edin.  
        Ağınızda NTP trafiğinin veri merkezinizden internet 'e geçmesine izin verdiğinden emin olun.

    3. İsteğe bağlı olarak, **IKINCIL NTP sunucusu** kutusuna cihazınız için bir ikincil sunucu girin.

    4. Yapılandırılan saat ayarlarını doğrulamak ve uygulamak için **Ayarları Uygula**' yı seçin.

        ![Yerel Web Kullanıcı arabirimi "zaman ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. Seçim Cihazınızda depolama dayanıklılığı yapılandırmak için sol bölmede **depolama ayarları** ' nı seçin. Bu özellik şu anda önizleme sürümündedir. Varsayılan olarak, cihazdaki depolama alanı dayanıklı değildir ve cihazda bir veri diski başarısız olursa veri kaybı olur. Dayanıklı seçeneğini etkinleştirdiğinizde, cihazdaki depolama alanı yeniden yapılandırılır ve cihaz veri kaybı olmadan bir veri diskinin hatasını değiştirebilir. Depolamayı dayanıklı olarak yapılandırmak, cihazınızın kullanılabilir kapasitesini azaltır.

    > [!IMPORTANT] 
    > Dayanıklılık yalnızca cihazı etkinleştirmeden önce yapılandırılabilir. 

    ![Yerel Web Kullanıcı arabirimi "depolama ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/storage-settings.png)

6. Sol bölmede **bulut ayarları**' nı seçin ve ardından Azure Portal Data Box Edge hizmetiyle cihazınızı etkinleştirin.
    
    1. **Etkinleştirme anahtarı** kutusunda, Data Box Edge için [etkinleştirme anahtarını al](data-box-edge-deploy-prep.md#get-the-activation-key) bölümünde aldığınız etkinleştirme anahtarını girin.
    2. **Uygula**’yı seçin.
       
        ![Yerel Web Kullanıcı arabirimi "bulut ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. İlk olarak cihaz etkinleştirilir. Daha sonra bu cihaz kritik güncelleştirmeler için taranır ve varsa güncelleştirmeler otomatik olarak uygulanır. Bu etkiye yönelik bir bildirim görürsünüz.

        İletişim kutusunda Ayrıca, kopyalayıp güvenli bir konuma kaydetmeniz gereken bir kurtarma anahtarı bulunur. Bu anahtar, cihazın önyüklenebildiği olaydaki verilerinizi kurtarmak için kullanılır.

        ![Yerel Web Kullanıcı arabirimi "bulut ayarları" sayfası güncelleştirildi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. Güncelleştirme başarıyla tamamlandıktan sonra birkaç dakika beklemeniz gerekebilir. Sayfa, cihazın başarıyla etkinleştirildiğini belirtecek şekilde güncelleştirilir.

        ![Yerel Web Kullanıcı arabirimi "bulut ayarları" sayfası güncelleştirildi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-8.png)

Cihaz kurulumu tamamlanmıştır. Artık cihazınıza paylaşımlar ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Fiziksel bir cihaza bağlanma
> * Fiziksel cihazı ayarlama ve etkinleştirme

Data Box Edge cihazınızla verilerin nasıl aktarılacağı hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Data Box Edge verileri aktarın](./data-box-edge-deploy-add-shares.md).
