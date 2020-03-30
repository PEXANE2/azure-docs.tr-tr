---
title: Microsoft Azure StorBasit Sanal Dizi iSCSI sunucu kurulumu | Microsoft Dokümanlar
description: İlk kurulumun nasıl gerçekleştirilip, StorSimple iSCSI sunucunuzu nasıl kaydedin ve aygıt kurulumunu nasıl tamamlayacağız.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 4560ca2b07826e2a071f515f147dfab8cbec3624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254501"
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>StorSimple Virtual Array'i dağıt – Azure portalı üzerinden iSCSI sunucusu olarak ayarlama

![iscsi kurulum süreci akışı](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu dağıtım öğreticisi Microsoft Azure StorBasit Sanal Dizi için geçerlidir. Bu öğretici, ilk kurulumu nasıl gerçekleştireceğimizi, StorSimple iSCSI sunucunuzu nasıl kaydedin, aygıt kurulumunu tamamlayın ve ardından iSCSI sunucusu olarak yapılandırılan StorSimple Virtual Array'unuzda birimleri nasıl oluşturup, monte edin, başlatılır ve biçimlendirin. 

Burada açıklanan prosedürlerin tamamlanması yaklaşık 30 dakika ile 1 saat sürer. Bu makalede yayınlanan bilgiler yalnızca StorSimple Virtual Arrays için geçerlidir.

## <a name="setup-prerequisites"></a>Kurulum önkoşulları

StorSimple Virtual Array'inizi yapılandırmadan ve kurmadan önce şunları yapın:

* Sen sanal bir dizi sağlanan ve bağlı olarak Deploy StorSimple Sanal Dizi açıklandığı gibi [- Hyper-V veya](storsimple-ova-deploy2-provision-hyperv.md) Deploy StorSimple Sanal Dizi bir sanal dizi sağlama [- VMware bir sanal dizi sağlama](storsimple-virtual-array-deploy2-provision-vmware.md).
* StorSimple Virtual Arrays'inizi yönetmek için oluşturduğunuz StorSimple Device Manager hizmetinden servis kayıt anahtarına sahipsiniz. Daha fazla bilgi için **bkz: Adım 2:** [StorSimple Virtual Array'de](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key)servis kayıt anahtarını alın - Portalı hazırlayın.
* Bu, varolan bir StorSimple Device Manager hizmetine kaydettiğiniz ikinci veya sonraki sanal diziyse, hizmet veri şifreleme anahtarına sahip olmanız gerekir. Bu anahtar, ilk aygıt bu hizmete başarıyla kaydedildiğinde oluşturuldu. Bu anahtarı kaybettiyseniz, [StorSimple Virtual Array'inizi yönetmek için Web Kullanıcı İyii'sini Kullan'daki](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) **hizmet veri şifreleme anahtarını alın'** a bakın.

## <a name="step-by-step-setup"></a>Adım adım kurulum

StorSimple Virtual Array'inizi ayarlamak ve yapılandırmak için aşağıdaki adım adım yönergeleri kullanın:

* [Step 1: Complete the local web UI setup and register your device](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* Adım 2: Gerekli aygıt kurulumını tamamlama
* [Adım 3: Ses düzeyi ekleme](#step-3-add-a-volume)
* [Adım 4: Bir birimi monte etme, başlatma ve biçimlendirme](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Step 1: Complete the local web UI setup and register your device

#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlamak ve cihazı kaydetmek için

1. Tarayıcı penceresini açın. Web Web Web Birimi türüne bağlanmak için:
   
    `https://<ip-address of network interface>`
   
    Önceki adımda belirtilen bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında bir sorun olduğunu bildiren bir hata görürsünüz. **Bu web sayfasına Devam et'i**tıklatın.
   
    ![güvenlik sertifikası hatası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. **StorSimpleAdmin**olarak sanal cihazınızın web Kullanıcı Arama Sürümü'nde oturum açın. Adım 3'te değiştirdiğiniz aygıt yöneticisi parolasını girin: Depolama Alanı Sanal Dizisini Dağıt'ta sanal aygıtı başlatın [- Hyper-V'de sanal bir cihaz sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [StorBasit Sanal Dizi'yi dağıtma - VMware'de sanal bir cihaz sağlama](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Oturum açma sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. **Ana** sayfaya götürüleceksiniz. Bu sayfada, sanal aygıtı StorSimple Device Manager hizmetine yapılandırmak ve kaydetmek için gereken çeşitli ayarlar açıklanmaktadır. **Ağ ayarları,** **Web proxy ayarları**ve Zaman **ayarlarının** isteğe bağlı olduğunu unutmayın. Gerekli tek ayarlar **Aygıt ayarları** ve Bulut **ayarlarıdır.**
   
    ![Giriş sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. **Ağ arabirimleri**altındaki **Ağ ayarları** sayfasında, DATA 0 sizin için otomatik olarak yapılandırılacaktır. Her ağ arabirimi varsayılan olarak bir IP adresi almak için ayarlanır (DHCP). Bu nedenle, bir IP adresi, alt ağ ve ağ geçidi otomatik olarak atanır (hem IPv4 hem de IPv6 için).
   
    Cihazınızı bir iSCSI sunucusu olarak dağıtmayı planlarken (sağlama bloğu depolamasına), **IP adresini otomatik olarak al** seçeneğini devre dışı bırakıp statik IP adreslerini yapılandırmanızı öneririz.
   
    ![Ağ ayarları sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Aygıtın sağlanması sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Unutmayın, ağ arabiriminizi yalnızca IPv4 olarak veya hem IPv4 hem de IPv6 olarak yapılandırabilirsiniz. IPv6 yalnızca yapılandırmaları desteklenmez.
5. DNS sunucuları, aygıtınız bulut depolama hizmeti sağlayıcılarınızla iletişim kurmaya veya dosya sunucusu olarak yapılandırıldıysa aygıtınızı ada göre çözmeye çalıştığında kullanıldığından gereklidir. **DNS sunucuları**altında **Ağ ayarları** sayfasında:
   
   1. Birincil ve ikincil Bir DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmayı seçerseniz, DNS sunucularını belirtebilirsiniz. Yüksek kullanılabilirlik için, birincil ve ikincil bir DNS sunucusu yapılandırmanızı öneririz.
   2. **Uygula**’ya tıklayın. Bu, ağ ayarlarını uygular ve doğrular.
6. Cihaz **ayarları** sayfasında:
   
   1. Cihazınıza benzersiz bir **Ad** atayın. Bu ad 1-15 karakter olabilir ve harf, sayı ve tire içerebilir.
   2. Oluşturduğunuz aygıt **türü** ![için iSCSI](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) sunucu simgesi **iSCSI** sunucu simgesini tıklatın. Bir iSCSI sunucusu blok depolama sağlamanızı sağlar.
   3. Bu aygıtın etki alanına katılmasını isteyip istemediğinizi belirtin. Aygıtınız bir iSCSI sunucusuysa, etki alanına katılmak isteğe bağlıdır. iSCSI sunucunuzu bir etki alanına katılmamaya karar verirseniz, **Uygula'yı**tıklatın, ayarların uygulanmasını bekleyin ve ardından bir sonraki adıma geçin.
      
       Aygıtı bir etki alanına katılmak istiyorsanız. Bir **Etki Alanı adı**girin ve sonra **Uygula'yı**tıklatın.
      
      > [!NOTE]
      > iSCSI sunucunuzu bir etki alanına katılıyorsanız, sanal dizinizin Microsoft Azure Etkin Dizini için kendi kuruluş biriminde (OU) olduğundan ve bu sunucuya grup ilkesi nesnelerinin (GPO) uygulanmadığından emin olun.
      > 
      > 
   4. Bir iletişim kutusu görüntülenir. Etki alanı kimlik bilgilerinizi belirtilen biçimde girin. Onay simgesine ![onay simgesi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). Etki alanı kimlik bilgileri doğrulanacaktır. Kimlik bilgileri yanlışsa bir hata iletisi görürsünüz.
      
       ![Kimlik bilgileri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. **Uygula**’ya tıklayın. Bu, aygıt ayarlarını uygular ve doğrular.
7. (İsteğe bağlı olarak) web proxy sunucunuzu yapılandırın. Web ara sunucusunun yapılandırması isteğe bağlı olsa bir web ara sunucu kullanıyorsanız, burada yalnızca bunu yapılandırabileceğinizi unutmayın.
   
    ![web proxy yapılandırmak](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Web **proxy** sayfasında:
   
   1. Web **proxy URL'sini** bu biçimde tedarik edin: *http:\//host-IP adresi* veya *FQDN:Bağlantı noktası numarası.* HTTPS URL'lerinin desteklenmediğini unutmayın.
   2. **Kimlik Doğrulamayı** **Temel** veya **Yok**olarak belirtin.
   3. Kimlik doğrulamakullanıyorsanız, bir **Kullanıcı Adı** ve **Parola**da sağlamanız gerekir.
   4. **Uygula**’ya tıklayın. Bu, yapılandırılan web proxy ayarlarını doğrular ve uygular.
8. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınızın saat ayarlarını yapılandırın. NTP sunucuları gereklidir, çünkü cihazınızın bulut hizmeti sağlayıcılarınızla kimlik doğrulaması yapabilmesi için zamanı eşitlemesi gerekir.
   
    ![Saat ayarları](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Saat **ayarları** sayfasında:
   
   1. Açılan listeden, aygıtın dağıtıldığı coğrafi konuma göre **Saat Dilimini** seçin. CihazınızIçin varsayılan saat dilimi PST'dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
   2. Aygıtınız için birincil **ntp sunucusu** belirtin veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
   3. İsteğe bağlı olarak aygıtınız için ikincil bir **NTP sunucusu** belirtin.
   4. **Uygula**’ya tıklayın. Bu, yapılandırılan zaman ayarlarını doğrular ve uygular.
9. CihazınızIçin bulut ayarlarını yapılandırın. Bu adımda, yerel aygıt yapılandırmasını tamamlar ve aygıtı StorSimple Device Manager hizmetinize kaydedebilirsiniz.
   
   1. Adım 2'de aldığınız **Hizmet kayıt anahtarını** girin: [StorSimple Virtual Array'de](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) **servis kayıt anahtarını alın** - Portalı hazırlayın.
   2. Bu hizmete kaydettiğiniz ilk aygıt bu değilse, **Hizmet veri şifreleme anahtarını**sağlamanız gerekir. Bu anahtar, StorSimple Device Manager hizmetine ek aygıtlar kaydetmek için servis kayıt anahtarı ile birlikte gereklidir. Daha fazla bilgi için yerel web kullanıcı arabiriminizdeki [hizmet veri şifreleme anahtarını alın'a](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) bakın.
   3. **Kaydol'u**tıklatın. Bu, aygıtı yeniden başlatacak. Aygıtın başarıyla kaydedilmesi için 2-3 dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına götürülür.
      
      ![Kayıt cihazı](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Azure portalına dönün.
11. Hizmetinizin **Aygıtlar** bıçağına gidin. Çok fazla kaynağınız varsa, **Tüm kaynaklara**tıklayın, hizmet adınızı tıklatın (gerekirse arayın) ve ardından **Cihazlar'ı**tıklatın.
12. **Cihazlar** bıçağında, durumu araştırarak aygıtın hizmete başarıyla bağladığını doğrulayın. Cihazın durumu **Kuruluma hazır** olmalıdır.
    
    ![Kayıt cihazı](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-the-device-as-iscsi-server"></a>Adım 2: Aygıtı iSCSI sunucusu olarak yapılandırın

Gerekli aygıt kurulumını tamamlamak için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-the-device-as-iscsi-server"></a>Aygıtı iSCSI sunucusu olarak yapılandırmak için

1. StorSimple Device Manager hizmetinize gidin ve ardından **Yönetim > Cihazları'na**gidin. **Cihazlar** bıçağında, yeni oluşturduğunuz aygıtı seçin. Bu **aygıt, ayarlamaya hazır**olarak gösteriş yapacak.
   
    ![Aygıtı iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Aygıtı tıklattığınızda, aygıtın kuruluma hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Aygıtı iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Aygıt komut çubuğunda **Yapıla'yı** tıklatın. Bu, **Configure** bıçağını açar. **Yapılandırma** bıçağında aşağıdakileri yapın:
   
   * iSCSI sunucu adı otomatik olarak doldurulur.
   * Bulut depolama şifrelemesinin **Etkin**olarak ayarlandıklarına emin olun. Bu, aygıttan buluta gönderilen verilerin şifrelenmesini sağlar.
   * 32 karakterlik bir şifreleme anahtarı belirtin ve ileride başvurmak için bir anahtar yönetim uygulamasına kaydedin.
   * Cihazınızda kullanılacak bir depolama hesabı seçin. Bu abonelikte, varolan bir depolama hesabı seçebilir veya farklı bir abonelikten hesap seçmek için **Ekle'yi** tıklatabilirsiniz.
     
     ![Aygıtı iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. iSCSI sunucusunu ayarlamayı tamamlamak için **Yapıya** tıklayın.
   
    ![Aygıtı iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. iSCSI sunucu oluşturma nın devam ettiği bildirilir. iSCSI sunucusu başarıyla oluşturulduktan sonra **Aygıtlar** bıçağı güncellenir ve ilgili cihaz durumu **Çevrimiçi**olur.
   
    ![Aygıtı iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>Adım 3: Ses düzeyi ekleme

1. **Cihazlar** bıçağında, iSCSI sunucusu olarak yapılandırdığınız aygıtı seçin. **...** 'ı tıklatın (alternatif olarak bu satırda sağ tıklatın) ve bağlam menüsünden **ses ekle'yi**seçin. Komut çubuğundan **+ Ses ekle'yi** de tıklatabilirsiniz. Bu, **Ses Ekle** bıçağını açar.
   
    ![Ses düzeyi ekleme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. Ses **ekle** bıçağında aşağıdakileri yapın:
   
   * Birim **adı** alanına, biriminiz için benzersiz bir ad girin. Ad, 3 ila 127 karakter içeren bir dize olmalıdır.
   * **Tür** açılır listesinde, **Katmanlı** veya Yerel olarak **sabitlenmiş** bir birim oluşturup oluşturmayacağımı belirtin. Yerel garantiler, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için **Yerel olarak sabitlenmiş** **birimi**seçin. Diğer tüm veriler için **Katmanlı** **birim'i**seçin.
   * **Kapasite** alanında, birimin boyutunu belirtin. Katmanlı bir hacim 500 GB ile 5 TB arasında, yerel olarak sabitlenmiş bir hacim ise 50 GB ile 500 GB arasında olmalıdır.
     
     Yerel olarak sabitlenmiş bir birim kalın bir şekilde karşılanır ve birimdeki birincil verilerin aygıtta kalmasını ve buluta dökülmemesini sağlar.
     
     Diğer taraftan katmanlı bir hacim ince olarak karşılanır. Katmanlı bir hacim oluşturduğunuzda, alanın yaklaşık %10'u yerel katmanda ve alanın %90'ı bulutta karşılanır. Örneğin, 1 TB birim sağlarsanız, 100 GB yerel alanda kalır ve veri katmanları bulutta 900 GB kullanılır. Bu da, aygıttaki tüm yerel alanın tükenirse, katmanlı bir pay sağlayamamasıdır (çünkü %10'u kullanılamayacaktır).
     
     ![Ses düzeyi ekleme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * **Bağlı ana bilgisayarlara**tıklayın, bu birimde bağlanmak istediğiniz iSCSI başlatıcısına karşılık gelen bir erişim denetim kaydı (ACR) seçin ve sonra **Seç'i**tıklatın. <br><br> 
3. Yeni bir bağlı ana bilgisayar eklemek için **yeni ekle'yi**tıklatın, ana bilgisayar için bir ad girin ve iSCSI Kalifiye Adı (IQN) girin ve sonra **Ekle'yi**tıklatın. IQN'niz yoksa, [Ek A'ya gidin: Windows Server ana bilgisayarının IQN'sini alın.](#appendix-a-get-the-iqn-of-a-windows-server-host)
   
      ![Ses düzeyi ekleme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Ses inizi yapılandırmayı bitirdiğinizde **Tamam'ı**tıklatın. Belirtilen ayarlarla bir birim oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, ses düzeyi için izleme ve yedekleme etkinleştirilir.
   
     ![Ses düzeyi ekleme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. Birimin başarıyla oluşturulduğunu doğrulamak için **Volumes** bıçağına gidin. Listelenen birim görmelisiniz.
   
   ![Ses düzeyi ekleme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Adım 4: Bir birimi monte etme, başlatma ve biçimlendirme

StorSimple birimlerinizi windows server ana bilgisayara monte etmek, başlatmak ve biçimlendirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Birimi bağlamak, başlatmak ve biçimlendirmek için

1. **iSCSI başlatıcı** uygulamasını uygun sunucuda açın.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Keşif** sekmesinde, **Portal Bul**’a tıklayın.
   
    ![portalı keşfedin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. **Hedef Portal Bul** iletişim kutusuna iSCSI etkin ağ arabiriminin IP adresini girin ve ardından **Tamam**’a tıklayın.
   
    ![IP adresi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. **iSCSI Başlatıcısı Özellikleri** penceresinin **Hedefler** sekmesinde **Bulunan hedefler**’i bulun. (Her birim keşfedilen bir hedef olacaktır.) Aygıt durumu Etkin **değil**olarak görünmelidir.
   
    ![keşfedilen hedefler](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Hedef aygıtı seçin ve sonra **Bağlan'ı**tıklatın. Cihaz bağlandıktan sonra durum **Bağlandı** olarak değişmelidir. (Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için [bkz.][1]
   
    ![hedef aygıtı seçin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Windows konağında Windows Logo + X tuşlarına basıp **Çalıştır**’a tıklayın.
7. **Çalıştır** iletişim kutusuna **Diskmgmt.msc** yazın. **Tamam**’a tıklayın, **Disk Yönetimi** iletişim kutusu görüntülenir. Sağ bölmede, konaktaki birimler gösterilir.
8. **Disk Yönetimi** penceresinde bağlanan birimler aşağıdaki çizimde gösterildiği gibi görünür. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.
   
    ![disk yönetimi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Sağ tıklayın ve **Initialize Disk'i**seçin.
   
    ![disk ivedimi başlatma 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. İletişim kutusunda, başlatılması için disk(ler)'i seçin ve ardından **Tamam'ı**tıklatın.
    
    ![disk 2'yi başlatma](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. Yeni Basit Birim sihirbazı başlar. Bir disk boyutu seçin ve sonra **İleri'yi**tıklatın.
    
    ![yeni birim sihirbazı 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Ses düzeyine bir sürücü mektubu atayın ve sonra **İleri'yi**tıklatın.
    
    ![yeni birim sihirbazı 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Birimi biçimlendirmek için parametreleri girin. **Windows Server'da yalnızca NTFS desteklenir.** Ayırma birimi boyutunu 64K olarak ayarlayın. Ses düzeyiniz için bir etiket sağlayın. Bu adın StorSimple Virtual Array'inizde sağladığınız birim adıile aynı olması önerilir. **İleri**'ye tıklayın.
    
    ![yeni birim sihirbazı 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Biriminizin değerlerini kontrol edin ve sonra **Bitir'i**tıklatın.
    
    ![yeni birim sihirbazı 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    Birimler **Disk Yönetimi** sayfasında **Çevrimiçi** olarak görünür.
    
    ![ciltler çevrimiçi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Virtual Array'inizi yönetmek](storsimple-ova-web-ui-admin.md)için yerel web kullanıcı arama ayını nasıl kullanacağınızı öğrenin.

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Ek A: Windows Server ana bilgisayarının IQN'sini alma

Windows Server 2012 çalıştıran bir Windows konağının iSCSI Tam Adını (IQN) almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Windows konağının IQN’sini almak için

1. Microsoft iSCSI başlatıcısını Windows konağında başlatın.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
   
    ![iSCSI başlatıcısı özellikleri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Bu dizeyi kaydedin.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



