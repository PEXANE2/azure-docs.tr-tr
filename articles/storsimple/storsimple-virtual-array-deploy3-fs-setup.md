---
title: StorSimple Virtual Array'i dosya sunucusu olarak ayarlama | Microsoft Dokümanlar
description: StorSimple Virtual Array dağıtımındaki bu üçüncü öğretici, sanal bir aygıtı dosya sunucusu olarak ayarlamanızı sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 16a5e0bb3e50e3a90951572e8d2847d379c1b114
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297638"
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>StorSimple Virtual Array'i dağıt - Azure portalı üzerinden dosya sunucusu olarak ayarlama
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Giriş

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu makalede, ilk kurulumun nasıl gerçekleştiriliş, StorSimple dosya sunucunuzun kaydedilmesi, aygıt kurulumunun nasıl tamamlandığı ve Kobİ paylaşımlarını nasıl oluşturup bağlanıştırılacak açıklanmaktadır. Bu, sanal dizinizi bir dosya sunucusu veya iSCSI sunucusu olarak tamamen dağıtmak için gereken dağıtım öğreticileri serisinin son makalesidir.

Kurulum ve yapılandırma işleminin tamamlanması yaklaşık 10 dakika sürebilir. Bu makaledeki bilgiler yalnızca StorSimple Sanal Dizi'nin dağıtımı için geçerlidir. StorSimple 8000 serisi cihazların dağıtımı için şuna gidin: [Update 2 çalıştıran StorSimple 8000 serisi cihazınızı dağıtın.](storsimple-deployment-walkthrough-u2.md)

## <a name="setup-prerequisites"></a>Kurulum önkoşulları
StorSimple Virtual Array'inizi yapılandırmadan ve kurmadan önce şunları yapın:

* Sanal bir dizi sağlamış ve [Hyper-V'deki StorSimple Virtual Array'de](storsimple-virtual-array-deploy2-provision-hyperv.md) ayrıntılı olarak belirtildiği gibi ona bağlısınız veya [VMware'de StorSimple Virtual Array'i sağlamışsınızdır.](storsimple-virtual-array-deploy2-provision-vmware.md)
* StorSimple Virtual Arrays'i yönetmek için oluşturduğunuz StorSimple Device Manager hizmetinden hizmet kayıt anahtarına sahipsiniz. Daha fazla bilgi için Bkz. Adım 2: StorSimple Virtual Array için [hizmet kayıt anahtarını alın.](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key)
* Bu, varolan bir StorSimple Device Manager hizmetine kaydettiğiniz ikinci veya sonraki sanal diziyse, hizmet veri şifreleme anahtarına sahip olmanız gerekir. Bu anahtar, ilk aygıt bu hizmete başarıyla kaydedildiğinde oluşturuldu. Bu anahtarı kaybettiyseniz, storsimple Virtual [Array'inizin hizmet veri şifreleme anahtarını alın'](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a bakın.

## <a name="step-by-step-setup"></a>Adım adım kurulum
StorSimple Virtual Array'inizi kurmak ve yapılandırmak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Step 1: Complete the local web UI setup and register your device
#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlamak ve cihazı kaydetmek için
1. Bir tarayıcı penceresi açın ve yerel web arabirimi'ne bağlanın. Şunu yazın:
   
   `https://<ip-address of network interface>`
   
   Önceki adımda belirtilen bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında bir sorun olduğunu belirten bir hata görürsünüz. **Bu web sayfasına Devam et'i**tıklatın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. **StorSimpleAdmin**olarak sanal dizinizin web Kullanıcı Arama Sürümü'nde oturum açın. Adım 3'te değiştirdiğiniz aygıt yöneticisi parolasını girin: [Hyper-V'de StorSimple Virtual Array'de](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [VMware'de StorSimple Virtual Array'de](storsimple-virtual-array-deploy2-provision-vmware.md)sanal diziyi başlatın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. **Ana** sayfaya götürülür. Bu sayfa, sanal diziyi StorSimple Device Manager hizmetine yapılandırmak ve kaydetmek için gereken çeşitli ayarları açıklar. **Ağ ayarları,** **Web proxy ayarları**ve Zaman **ayarları** isteğe bağlıdır. Gerekli tek ayarlar **Aygıt ayarları** ve Bulut **ayarlarıdır.**
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. **Ağ arabirimleri**altındaki **Ağ ayarları** sayfasında, DATA 0 sizin için otomatik olarak yapılandırılacaktır. Her ağ arabirimi, IP adresini otomatik olarak (DHCP) almak için varsayılan olarak ayarlanır. Bu nedenle, bir IP adresi, alt ağ ağı ve ağ geçidi otomatik olarak atanır (hem IPv4 hem de IPv6 için).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Aygıtın sağlanması sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Unutmayın, ağ arabiriminizi yalnızca IPv4 olarak veya hem IPv4 hem de IPv6 olarak yapılandırabilirsiniz. IPv6 yalnızca yapılandırmaları desteklenmez.
5. DNS sunucuları, aygıtınız bulut depolama hizmeti sağlayıcılarınızla iletişim kurmaya veya dosya sunucusu olarak yapılandırıldığında aygıtınızı ada göre çözmeye çalıştığında kullanıldığından gereklidir. **DNS sunucularının**altındaki **Ağ ayarları** sayfasında:
   
   1. Birincil ve ikincil Bir DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmayı seçerseniz, DNS sunucularını belirtebilirsiniz. Yüksek kullanılabilirlik için, birincil ve ikincil bir DNS sunucusu yapılandırmanızı öneririz.
   2. Ağ ayarlarını uygulamak ve doğrulamak için **Uygula'yı** tıklatın.
6. Cihaz **ayarları** sayfasında:
   
   1. Cihazınıza benzersiz bir **Ad** atayın. Bu ad 1-15 karakter olabilir ve harf, sayı ve tire içerebilir.
   2. Oluşturduğunuz aygıt ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) **türü** için Dosya **sunucusu** simgesini tıklatın. Dosya sunucusu paylaşılan klasörler oluşturmanıza olanak sağlar.
   3. Aygıtınız bir dosya sunucusu olduğundan, aygıtı bir etki alanına katılmanız gerekir. Alan **adı**girin.
   4. **Uygula**’ya tıklayın.
7. Bir iletişim kutusu görüntülenir. Etki alanı kimlik bilgilerinizi belirtilen biçimde girin. Denetim simgesini tıklatın. Etki alanı kimlik bilgileri doğrulanır. Kimlik bilgileri yanlışsa bir hata iletisi görürsünüz.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. **Uygula**’ya tıklayın. Bu, aygıt ayarlarını uygular ve doğrular.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Sanal dizinizin Active Directory için kendi kuruluş biriminde (OU) olduğundan ve ona grup ilkesi nesnelerinin (GPO) uygulanmadığından veya devralındığından emin olun. Grup ilkesi, StorSimple Virtual Array'e virüsten koruma yazılımı gibi uygulamalar yükleyebilir. Ek yazılım yüklemek desteklenmez ve veri bozulmasına yol açabilir. 
   > 
   > 
9. (İsteğe bağlı olarak) web proxy sunucunuzu yapılandırın. Web ara sunucusunun yapılandırması isteğe bağlı olsa bir web ara sunucu kullanıyorsanız, burada yalnızca bunu yapılandırabileceğinizi unutmayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   Web **proxy** sayfasında:
   
   1. Web **proxy URL'sini** bu biçimde sağlama: *http://&lt;ana&gt;bilgisayar-IP adresi veya FQDN :Port numarası.* HTTPS URL'lerinin desteklenmediğini unutmayın.
   2. **Kimlik Doğrulamayı** **Temel** veya **Yok**olarak belirtin.
   3. Kimlik doğrulamasını kullanıyorsanız, bir **Kullanıcı Adı** ve **Parola**da sağlamanız gerekir.
   4. **Uygula**’ya tıklayın. Bu, yapılandırılan web proxy ayarlarını doğrular ve uygular.
10. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınızın saat ayarlarını yapılandırın. NTP sunucuları gereklidir, çünkü cihazınızın bulut hizmeti sağlayıcılarınızla kimlik doğrulaması yapabilmesi için zamanı eşitlemesi gerekir.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    Saat **ayarları** sayfasında:
    
    1. Açılan listeden, aygıtın dağıtıldığı coğrafi konuma göre **Saat Dilimini** seçin. CihazınızIçin varsayılan saat dilimi PST'dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Aygıtınız için birincil **ntp sunucusu** belirtin veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak aygıtınız için ikincil bir **NTP sunucusu** belirtin.
    4. **Uygula**’ya tıklayın. Bu, yapılandırılan zaman ayarlarını doğrular ve uygular.
11. CihazınızIçin bulut ayarlarını yapılandırın. Bu adımda, yerel aygıt yapılandırmasını tamamlar ve aygıtı StorSimple Device Manager hizmetinize kaydedebilirsiniz.
    
    1. Adım 2'de aldığınız **Hizmet kayıt anahtarını** girin: StorSimple Virtual Array için [servis kayıt anahtarını alın.](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key)
    2. Bu hizmete kaydolan ilk aygıtınızsa, Size **Hizmet veri şifreleme anahtarı**sunulur. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. Bu anahtar, StorSimple Device Manager hizmetine ek aygıtlar kaydetmek için servis kayıt anahtarı ile birlikte gereklidir. 
       
       Bu hizmete kaydettiğiniz ilk aygıt bu değilse, hizmet veri şifreleme anahtarını sağlamanız gerekir. Daha fazla bilgi için yerel web kullanıcı arabiriminizdeki [hizmet veri şifreleme anahtarını](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) almak için başvurun.
    3. **Kaydol'u**tıklatın. Bu, aygıtı yeniden başlatacak. Aygıtın başarıyla kaydedilmesi için 2-3 dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına götürülür.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Azure portalına dönün. Tüm **kaynaklara**gidin, StorSimple Device Manager hizmetinizi arayın.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. Filtre uygulanmış listede, StorSimple Device Manager hizmetinizi seçin ve ardından **Yönetim > Aygıtları'na**gidin. **Cihazlar** bıçağında, aygıtın hizmete başarıyla bağladığını ve **ayarlanacak**duruma hazır olduğunu doğrulayın.
    
    ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-the-device-as-file-server"></a>Adım 2: Aygıtı dosya sunucusu olarak yapılandırma
Gerekli aygıt kurulumını tamamlamak için [Azure portalında](https://portal.azure.com/) aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-the-device-as-file-server"></a>Aygıtı dosya sunucusu olarak yapılandırmak için
1. StorSimple Device Manager hizmetinize gidin ve ardından **Yönetim > Cihazları'na**gidin. **Cihazlar** bıçağında, yeni oluşturduğunuz aygıtı seçin. Bu **aygıt, ayarlamaya hazır**olarak gösteriş yapacak.
   
   ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Aygıtı tıklattığınızda, aygıtın kuruluma hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Komut çubuğunda **Yapıla'yı** tıklatın. Bu, **Configure** bıçağını açar. **Yapılandırma** bıçağında aşağıdakileri yapın:
   
   1. Dosya sunucusu adı otomatik olarak doldurulur.
    
   2. Bulut depolama şifrelemesinin **Etkin**olarak ayarlandıklarına emin olun. Bu, buluta gönderilen tüm verileri şifreler. 
    
   3. Şifreleme için kullanıcı tanımlı anahtarla birlikte 256 bit AES anahtarı kullanılır. 32 karakterli bir anahtar belirtin ve onaylamak için anahtarı yeniden girin. Anahtarı ileride başvurmak için önemli bir yönetim uygulamasına kaydedin.
    
   4. Cihazınızda kullanılacak depolama hesabı kimlik bilgilerini belirtmek için **gerekli ayarları Yapılandır'ı** tıklatın. Yapılandırılan depolama hesabı kimlik bilgileri yoksa **yeni ekle'yi** tıklatın. **Kullandığınız depolama hesabının blok blobs'u desteklediğinden emin olun. Sayfa lekeleri desteklenmez.** [Bloklar blobs ve sayfa lekeleri](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)hakkında daha fazla bilgi .
   
      ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. Depolama **hesabı kimlik bilgileri ekle** bıçağında aşağıdakileri yapın: 

    1. Depolama hesabı hizmetle aynı abonelikteyse geçerli aboneliği seçin. Diğer belirtin depolama hesabı hizmet aboneliği dışında olmasıdır. 
    
    2. Açılan listeden, varolan bir depolama hesabı seçin. 
    
    3. Konum, belirtilen depolama hesabına göre otomatik olarak doldurulur. 
    
    4. Aygıt ve bulut arasında güvenli bir ağ iletişim kanalı sağlamak için TLS'yi etkinleştirin.
    
    5. Bu depolama hesabı kimlik bilgisini eklemek için **Ekle'yi** tıklatın. 
   
        ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Depolama hesabı kimlik bilgileri başarıyla oluşturulduktan sonra, **Yapılandırma** bıçağı belirtilen depolama hesabı kimlik bilgilerini görüntülemek için güncelleştirilir. **Yapılandır**'a tıklayın.
   
   ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Bir dosya sunucusu oluşturulduğunu görürsünüz. Dosya sunucusu başarıyla oluşturulduktan sonra, size bildirilir.
   
   ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Cihaz durumu da **Online**olarak değişecektir.
   
   ![Dosya sunucusuyapılandırma](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   Paylaşım eklemeye devam edebilirsiniz.

## <a name="step-3-add-a-share"></a>Adım 3: Paylaşım ekleme
Paylaşım oluşturmak için [Azure portalında](https://portal.azure.com/) aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-share"></a>Paylaşım oluşturmak için
1. Önceki adımda yapılandırdığınız dosya sunucusu aygıtını seçin ve **...** (veya sağ tıklatın) düğmesine tıklayın. Bağlam menüsünde **paylaş ekle'yi**seçin. Alternatif olarak, aygıt komut çubuğunda **+ Paylaş Ekle'yi** tıklatabilirsiniz.
   
   ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Aşağıdaki paylaşım ayarlarını belirtin:

   1. Payınız için eşsiz bir isim. Ad, 3 ila 127 karakter içeren bir dize olmalıdır.
    
   2. Paylaşım için isteğe bağlı **bir Açıklama.** Açıklama, paylaşım sahiplerinin tanımlanmasına yardımcı olur.
    
   3. Paylaşım için bir **tür.** Tür **Katmanlı** veya **Yerel olarak sabitlenmiş**olabilir , katmanlı varsayılan olarak. Yerel garantiler, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için **Yerel olarak sabitlenmiş** bir pay seçin. Diğer tüm veriler için **Katmanlı** paylaşım seçin.
      Yerel olarak sabitlenmiş bir pay kalın bir şekilde karşılanır ve hissedeki birincil verilerin aygıtta yerel kalmasını ve buluta dökülmemesini sağlar. Diğer taraftan katmanlı bir pay ince olarak karşılanır. Katmanlı bir pay oluşturduğunuzda, alanın %10'u yerel katmanda, %90'ı ise bulutta karşılanır. Örneğin, 1 TB birim sağlarsanız, 100 GB yerel alanda kalır ve veri katmanları bulutta 900 GB kullanılır. Bu da, aygıttaki tüm yerel alanın tükenirse, katmanlı bir pay sağlayamayacağınız anlamına gelir.
   
   4. Varsayılan **alanı ayarla,,** izinleri kullanıcıya veya bu paylaşıma erişen gruba atayın. *John\@contoso.com* biçiminde kullanıcının veya kullanıcı grubunun adını belirtin. Yönetici ayrıcalıklarının bu paylaşımlara erişmesine izin vermek için bir kullanıcı grubu (tek bir kullanıcı yerine) kullanmanızı öneririz. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.
   
   5. Paylaşımı oluşturmak için **Ekle'yi** tıklatın. 
    
       ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
       Paylaşım oluşturma işleminin devam ettiği size bildirilir.
   
       ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
      Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** bıçağı yeni paylaşımı yansıtacak şekilde güncellenir. Varsayılan olarak, izleme ve yedekleme paylaşım için etkinleştirilir.
   
      ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-to-the-share"></a>Adım 4: Paylaşıma bağlanın
Artık önceki adımda oluşturduğunuz bir veya daha fazla paylaşıma bağlanmanız gerekir. Bu adımları StorSimple Virtual Array'inize bağlı Windows Server ana bilgisayarınızda gerçekleştirin.

#### <a name="to-connect-to-the-share"></a>Paylaşıma bağlanmak için
1. + ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) R tuşuna basın. Çalıştır penceresinde, *dosya sunucusu adını* dosya sunucunuza atadığınız aygıt adıyla değiştirerek yol olarak *&#92;&#92;&lt;dosya sunucusu adını&gt; * belirtin. **Tamam**'a tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Bu Dosya Gezgini açılır. Artık oluşturduğunuz paylaşımları klasörler olarak görebiliyor olmalısınız. İçeriğini görmek için paylaşımı (klasörü) seçin ve çift tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Artık bu paylaşımlara dosya ekleyebilir ve yedek alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Virtual Array'inizi yönetmek](storsimple-ova-web-ui-admin.md)için yerel web kullanıcı arama ayını nasıl kullanacağınızı öğrenin.

