---
title: Azure Depolama Gezgini sorun giderme kılavuzu | Microsoft Dokümanlar
description: Azure Depolama Gezgini için hata ayıklama tekniklerine genel bakış
services: storage
author: Deland-Han
manager: dcscontentpm
ms.service: storage
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: db36033ea524603416f16db27f40d5eefb8bf613
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437113"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Depolama Gezgini sorun giderme kılavuzu

Microsoft Azure Depolama Gezgini, Windows, macOS ve Linux'taki Azure Depolama verileriyle çalışmayı kolaylaştıran bağımsız bir uygulamadır. Uygulama, Azure, ulusal bulutlar ve Azure Yığını'nda barındırılan depolama hesaplarına bağlanabilir.

Bu kılavuz, Depolama Gezgini'nde sık görülen sorunların çözümlerini özetler.

## <a name="rbac-permissions-issues"></a>RBAC izinleri sorunları

Rol tabanlı erişim denetimi [RBAC,](https://docs.microsoft.com/azure/role-based-access-control/overview) izin kümelerini _rollerde_birleştirerek Azure kaynaklarının son derece parçalı erişim yönetimini sağlar. RBAC'ın Depolama Gezgini'nde en iyi şekilde çalışmasını sağlamak için bazı stratejiler aşağıda vereb

### <a name="how-do-i-access-my-resources-in-storage-explorer"></a>Depolama Gezgini'ndeki kaynaklarıma nasıl erişebilirim?

RBAC aracılığıyla depolama kaynaklarına erişmekte sorun yaşıyorsanız, uygun roller atanmamış olabilirsiniz. Aşağıdaki bölümlerde Depolama Gezgini'nin depolama kaynaklarınıza erişmek için şu anda ihtiyaç duyduğu izinler açıklanır. Uygun rollere veya izinlere sahip olduğunuzdan emin değilseniz Azure hesap yöneticinize başvurun.

#### <a name="read-listget-storage-accounts-permissions-issue"></a>"Oku: List/Get Depolama Hesabı(lar)" izinleri sorunu

Depolama hesaplarını listelemek için izniniz olmalıdır. Bu izni almak için _Okuyucu_ rolüne atanmış olmalısınız.

#### <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını listele

Depolama Gezgini, istekleri doğrulamak için hesap anahtarlarını da kullanabilir. _Katılımcı_ rolü gibi daha güçlü roller aracılığıyla hesap anahtarlarına erişebilirsiniz.

> [!NOTE]
> Erişim anahtarları, bu anahtarları elinde bulunduran herkese sınırsız izin verir. Bu nedenle, bu anahtarları hesap kullanıcılarına dağıtmanızı önermiyoruz. Erişim anahtarlarını iptal etmeniz gerekiyorsa, bunları [Azure portalından](https://portal.azure.com/)yeniden oluşturabilirsiniz.

#### <a name="data-roles"></a>Veri rolleri

Kaynaklardan okuma verilerine erişim sağlayan en az bir rol atanırsınız. Örneğin, blob'ları listelemek veya indirmek istiyorsanız, en azından _Depolama Blob Veri Okuyucu_ rolüne ihtiyacınız vardır.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Depolama Gezgini'ndeki kaynaklarımı görmek için neden bir yönetim katmanı rolüne ihtiyacım var?

Azure Depolama'nın iki erişim katmanı vardır: _yönetim_ ve _veri._ Aboneliklere ve depolama hesaplarına yönetim katmanı üzerinden erişilir. Kapsayıcılara, blob'lara ve diğer veri kaynaklarına veri katmanı üzerinden erişilir. Örneğin, Azure'dan depolama alanhesaplarınızın listesini almak istiyorsanız, yönetim bitiş noktasına bir istek gönderirsiniz. Bir hesaptaki blob kapsayıcılarının listesini istiyorsanız, ilgili servis bitiş noktasına bir istek gönderirsiniz.

RBAC rolleri, yönetim veya veri katmanı erişimi için izinler içerebilir. Örneğin, Reader rolü yönetim katmanı kaynaklarına salt okunur erişim sağlar.

Açıkçası, Okuyucu rolü veri katmanı izinleri sağlamaz ve veri katmanına erişmek için gerekli değildir.

Depolama Gezgini, Azure kaynaklarınıza bağlanmak için gerekli bilgileri toplayarak kaynaklarınıza erişmenizi kolaylaştırır. Örneğin, blob kapsayıcılarınızı görüntülemek için Depolama Gezgini blob hizmeti bitiş noktasına bir "liste kapsayıcıları" isteği gönderir. Bu bitiş noktasını elde etmek için Depolama Gezgini, erişebildiğiniz aboneliklerin ve depolama hesaplarının listesini arar. Aboneliklerinizi ve depolama hesaplarınızı bulmak için Depolama Gezgini'nin yönetim katmanına da erişmesi gerekir.

Herhangi bir yönetim katmanı izni veren bir rolünüz yoksa, Depolama Gezgini veri katmanına bağlanmak için gereken bilgileri alamaz.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>Yöneticimden ihtiyacım olan yönetim katmanı izinlerini alamıyorsam ne olur?

Şu anda bu sorun için RBAC ile ilgili bir çözüm yok. Geçici çözüm olarak, kaynağınıza eklemek için bir SAS URI [isteyebilirsiniz.](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#use-a-shared-access-signature-uri)

### <a name="recommended-built-in-rbac-roles"></a>Önerilen yerleşik RBAC rolleri

Depolama Gezgini'ni kullanmak için gereken izinleri sağlayabilecek birkaç yerleşik RBAC rolü vardır. Bu rollerden bazıları şunlardır:
- [Sahibi](/azure/role-based-access-control/built-in-roles#owner): Kaynaklara erişim de dahil olmak üzere her şeyi yönetin. **Not**: Bu rol size anahtar erişim verecektir.
- [Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#contributor): Kaynaklara erişim hariç her şeyi yönetin. **Not**: Bu rol size anahtar erişim verecektir.
- [Okuyucu](/azure/role-based-access-control/built-in-roles#reader): Kaynakları okuyun ve listele.
- [Depolama Hesabı Katılımcısı](/azure/role-based-access-control/built-in-roles#storage-account-contributor): Depolama hesaplarının tam yönetimi. **Not**: Bu rol size anahtar erişim verecektir.
- [Depolama Blob Veri Sahibi](/azure/role-based-access-control/built-in-roles#storage-blob-data-owner): Azure Depolama blob kapsayıcılarına ve verilerine tam erişim.
- [Depolama Blob Veri Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor): Azure Depolama kapsayıcılarını ve bloblarını okuyun, yazın ve silin.
- [Depolama Blob Veri Okuyucu](/azure/role-based-access-control/built-in-roles#storage-blob-data-reader): Azure Depolama kapsayıcılarını ve lekelerini okuyun ve listelayın.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hata: Sertifika zincirinde kendi imzalı sertifika (ve benzer hatalar)

Sertifika hataları genellikle aşağıdaki durumlardan birinde oluşur:

- Uygulama, bir sunucunun (şirket sunucunuz gibi) HTTPS trafiğini engellediği, şifresini çözmesi ve ardından kendi imzaladığı bir sertifika kullanarak şifrelediği anlamına gelen saydam bir _proxy_aracılığıyla bağlanır.
- Aldığınız HTTPS iletilerine kendi imzalı TLS/SSL sertifikası enjekte eden bir uygulama çalıştırıyorsunuz. Sertifika enjekte eden uygulamalara örnek olarak virüsten koruma ve ağ trafiği denetleme yazılımı verilebilir.

Depolama Gezgini kendi imzalı veya güvenilmeyen bir sertifika gördüğünde, alınan HTTPS iletisinin değiştirilip değiştirilmediğini artık bilmez. Kendi imzalı sertifikanın bir kopyasına sahipseniz, aşağıdaki adımları izleyerek Depolama Gezgini'ne güvenebileceği konusunda talimat verebilirsiniz:

1. Sertifikanın Base-64 kodlu X.509 (.cer) kopyasını edinin.
2.  > **SSL Sertifikalarını** > **Edit Sertifikalarını** **Edin'e**gidin ve sonra .cer dosyasını bulmak, seçmek ve açmak için dosya seçiciyi kullanın.

Birden çok sertifika (kök ve ara) varsa, bu sorun da oluşabilir. Bu hatayı gidermek için her iki sertifikanın da eklenmesi gerekir.

Sertifikanın nereden geldiğinden emin değilseniz, sertifikayı bulmak için aşağıdaki adımları izleyin:

1. OpenSSL'yi yükleyin.
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html): Işık sürümlerinden herhangi biri yeterli olmalıdır.
    * Mac ve Linux: İşletim sisteminize dahil edilmelidir.
2. OpenSSL çalıştırın.
    * Windows: Yükleme dizinini açın, **/bin/**' seçeneğini seçin ve ardından **opensl.exe'yi**çift tıklatın.
    * Mac ve Linux: Bir terminalden çalıştırın. `openssl`
3. `s_client -showcerts -connect microsoft.com:443` öğesini çalıştırın.
4. Otomatik olarak imzalanan sertifikaları bulun. Hangi sertifikaların kendi imzalı olduğundan emin değilseniz, konu `("s:")` nun ve verenin `("i:")` aynı olduğu her yeri not edin.
5. Kendi imzalı sertifikaları bulduğunuzda, her biri için, yeni bir .cer `-----BEGIN CERTIFICATE-----` `-----END CERTIFICATE-----` dosyasına her şeyi kopyalayın ve yapıştırın (ve dahil) yeni bir .cer dosyasına.
6. Açık Depolama Explorer ve**SSL Sertifikaları** > **Alma Sertifikaları** **edit** > gidin. Ardından, oluşturduğunuz .cer dosyalarını bulmak, seçmek ve açmak için dosya seçiciyi kullanın.

Bu adımları izleyerek kendi imzalı sertifikalar bulamıyorsanız, geri bildirim aracı aracılığıyla bize ulaşın. Bayrağı kullanarak komut satırından Depolama Gezgini'ni `--ignore-certificate-errors` de açabilirsiniz. Bu bayrakla açıldığında, Depolama Gezgini sertifika hatalarını yoksayılmaktadır.

## <a name="sign-in-issues"></a>Oturum açma sorunları

### <a name="blank-sign-in-dialog-box"></a>Boş oturum açma iletişim kutusu

Boş oturum açma iletişim kutuları genellikle Active Directory Federation Services (AD FS) Depolama Gezgini'ni Electron tarafından desteklenmeyen bir yönlendirme gerçekleştirmesini istediğinde oluşur. Bu sorunu çözmek için oturum açma için Aygıt Kodu Akışını kullanmayı deneyebilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Sol daki dikey araç çubuğunda **Ayarlar'ı**açın. Ayarlar Paneli'nde **Uygulama** > **Oturum Aç'ına**gidin. **EtkinleştirAygıt kodu akış oturum açma'** yı kullan.
2. **Bağlan** iletişim kutusunu açın (sol taraftaki dikey çubuktaki fiş simgesinden veya hesap panelinde **Hesap Ekle'yi** seçerek).
3. Oturum etmek istediğiniz ortamı seçin.
4. **Oturum Aç'ı**seçin.
5. Bir sonraki paneldeki talimatları izleyin.

Varsayılan tarayıcınız zaten farklı bir hesapta oturum açmış olduğundan kullanmak istediğiniz hesapta oturum açamıyorsanız, aşağıdakilerden birini yapın:

- Bağlantıyı ve kodu tarayıcınızın özel oturumuna el ile kopyalayın.
- Bağlantıyı ve kodu el ile farklı bir tarayıcıya kopyalayın.

### <a name="reauthentication-loop-or-upn-change"></a>Yeniden authentication döngüsü veya UPN değişikliği

Yeniden kimlik doğrulama döngüsündeyseniz veya hesaplarınızdan birinin UPN'sini değiştirdiyseniz aşağıdaki adımları izleyin:

1. Tüm hesapları kaldırın ve ardından Depolama Gezgini'ni kapatın.
2. Sil. Makinenizden IdentityService klasörü. Windows'da klasör ' de `C:\users\<username>\AppData\Local`bulunur. Mac ve Linux için, klasörü kullanıcı dizinizin kökünde bulabilirsiniz.
3. Mac veya Linux çalıştırıyorsanız, microsoft.developer.identityservice girişini işletim sisteminizin anahtar mağazasından da silmeniz gerekir. Mac'te anahtarlık *Gnome Anahtarlık* uygulamasıdır. Linux'ta, uygulama genellikle _Anahtarlık_olarak adlandırılır, ancak adınız dağıtımınıza bağlı olarak değişebilir.

### <a name="conditional-access"></a>Koşullu Erişim

Depolama Gezgini tarafından kullanılan Azure REKLAM Kitaplığı'ndaki bir sınırlama nedeniyle, Depolama Gezgini Windows 10, Linux veya macOS'ta kullanılırken Koşullu Erişim desteklenmez.

## <a name="mac-keychain-errors"></a>Mac Anahtarlık hataları

macOS Anahtar Zinciri bazen Depolama Gezgini kimlik doğrulama kitaplığı için sorunlara neden olan bir durum girebilir. Anahtarzinciri bu durumdan çıkarmak için aşağıdaki adımları izleyin:

1. Depolama Gezgini'ni kapatın.
2. Anahtarlık aç (Komut+Boşluk çubuğuna basın, **anahtarlık**yazın ve Enter tuşuna basın).
3. "Giriş" Anahtarlık'ını seçin.
4. Anahtarlık'ı kilitlemek için asma kilit simgesini seçin. (İşlem tamamlandığında asma kilit kilitli görünür. Hangi uygulamaları açtığınıza bağlı olarak birkaç saniye sürebilir).

    ![Asma kilit simgesi](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Depolama Gezgini'ni açın.
6. "Hizmet hub'ı Anahtarlık'a erişmek istiyor" gibi bir ileti yle istenir. Mac yönetici hesap parolanızı girin ve **Her Zaman İzin Ver** 'i seçin (veya Her Zaman İzin **Ver** kullanılamıyorsa **İzin Ver'i)** seçin.
7. Oturum açmayı dene.

### <a name="general-sign-in-troubleshooting-steps"></a>Genel oturum açma sorun giderme adımları

* macOS'taysanız ve oturum açma penceresi kimlik doğrulama yı **beklerken** iletişim kutusunun üzerinde hiç görünmüyorsa, [aşağıdaki adımları](#mac-keychain-errors)deneyin.
* Depolama Gezgini'ni yeniden başlatın.
* Kimlik doğrulama penceresi boşsa, kimlik doğrulama iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarlarınızın hem makineniz hem de Depolama Gezgini için düzgün şekilde yapılandırıldığından emin olun.
* Windows'u çalıştırıyorsanız ve aynı makinede ve oturum açma kimlik bilgileriyle Visual Studio 2019'a erişiminiz varsa, Visual Studio 2019'a kaydolmayı deneyin. Visual Studio 2019'da başarılı bir oturum açtıktan sonra Storage Explorer'ı açabilir ve hesap panelinde hesabınızı görebilirsiniz.

Bu yöntemlerin hiçbiri çalışmıyorsa, [GitHub'da bir sorun açın.](https://github.com/Microsoft/AzureStorageExplorer/issues)

### <a name="missing-subscriptions-and-broken-tenants"></a>Eksik abonelikler ve bozuk kiracılar

Başarılı bir şekilde oturum açınca aboneliklerinizi geri alamıyorsanız, aşağıdaki sorun giderme yöntemlerini deneyin:

* Hesabınızın beklediğiniz aboneliklere erişimi olduğunu doğrulayın. Kullanmaya çalıştığınız Azure ortamı için portalda oturum açarak erişiminizi doğrulayabilirsiniz.
* Doğru Azure ortamında (Azure, Azure China 21Vianet, Azure Almanya, Azure ABD Hükümeti veya Özel Ortam) oturum yaptığınızdan emin olun.
* Proxy sunucusunun arkasındaysanız, Depolama Gezgini proxy'sini doğru şekilde yapılandırdığınızdan emin olun.
* Hesabı kaldırmayı ve yeniden eklemeyi deneyin.
* "Daha fazla bilgi" bağlantısı varsa, başarısız olan kiracılar için hangi hata iletilerinin bildirildiğini denetleyin. Hata iletilerine nasıl yanıt vereceğinden emin değilseniz, [GitHub'da bir sorun açmaktan çekinmeyin.](https://github.com/Microsoft/AzureStorageExplorer/issues)

## <a name="cant-remove-an-attached-account-or-storage-resource"></a>Ekli bir hesabı veya depolama kaynağını kaldıramıyorum

Kullanıcı Arabirimi aracılığıyla ekli bir hesabı veya depolama kaynağını kaldıramıyorsanız, aşağıdaki klasörleri silerek eklenen tüm kaynakları el ile silebilirsiniz:

* Windows:`%AppData%/StorageExplorer`
* Macos:`/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux:`~/.config/StorageExplorer`

> [!NOTE]
> Bu klasörleri silmeden önce Depolama Gezgini'ni kapatın.

> [!NOTE]
> Hiç SSL sertifikası aldıysanız, dizinin içeriğini yedekle. `certs` Daha sonra, SSL sertifikalarınızı yeniden almak için yedeklemeyi kullanabilirsiniz.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, girdiğiniz aşağıdaki bilgilerin doğru olduğundan emin olun:

* Proxy URL'si ve bağlantı noktası numarası
* Proxy gerektiriyorsa kullanıcı adı ve parola

> [!NOTE]
> Depolama Gezgini, proxy ayarlarını yapılandırmak için proxy otomatik config dosyalarını desteklemez.

### <a name="common-solutions"></a>Ortak çözümler

Sorun yaşamaya devam ediyorsanız, aşağıdaki sorun giderme yöntemlerini deneyin:

* Proxy'nizi kullanmadan internete bağlanabilirseniz, Depolama Gezgini'nin proxy ayarları etkinleştirilmeden çalıştığını doğrulayın. Bu durumda, proxy ayarlarınızla ilgili bir sorun olabilir. Sorunları tanımlamak için yöneticinizle birlikte çalışın.
* Proxy sunucusukullanan diğer uygulamaların beklendiği gibi çalıştığını doğrulayın.
* Kullanmaya çalıştığınız Azure ortamı için portala bağlanabileceğinizi doğrulayın.
* Hizmet bitiş noktanızdan yanıt alabileceğinizden doğrulayın. Tarayıcınıza bitiş noktası URL'lerinden birini girin. Bağlanabilirseniz, InvalidQueryParameterValue veya benzer bir XML yanıtı almanız gerekir.
* Başka biri de proxy sunucunuzla Depolama Gezgini kullanıyorsa, bağlanabiliyor muş doğrulayın. Eğer yapabilirlerse, proxy sunucu yöneticinize başvurmanız gerekebilir.

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılama araçları

Windows için Fiddler gibi ağ araçlarınız varsa, sorunları aşağıdaki gibi tanılayabilirsiniz:

* Proxy'niz aracılığıyla çalışmanız gerekiyorsa, proxy üzerinden bağlanmak için ağ aracınızı yapılandırmanız gerekebilir.
* Ağ aracınız tarafından kullanılan bağlantı noktası numarasını kontrol edin.
* Depolama Gezgini'nde proxy ayarları olarak yerel ana bilgisayar URL'sini ve ağ aracının bağlantı noktası numarasını girin. Bunu doğru yaptığınızda, ağ aracınız Depolama Gezgini tarafından yapılan ağ isteklerini yönetim ve hizmet bitiş noktalarına günlüğe kaydetmeye başlar. Örneğin, bir `https://cawablobgrs.blob.core.windows.net/` tarayıcıdaki blob bitiş noktanızı girin ve aşağıdakilere benzeyen bir yanıt alırsınız:

  ![Kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

  Bu yanıt, kaynağın erişemeseniz bile var olduğunu gösterir.

### <a name="contact-proxy-server-admin"></a>İletişim proxy sunucu yöneticisi

Proxy ayarlarınız doğruysa, proxy sunucu yöneticinize başvurmanız gerekebilir:

* Proxy'nizin Azure yönetimine veya kaynak bitiş noktalarına olan trafiği engellemediğinden emin olun.
* Proxy sunucunuz tarafından kullanılan kimlik doğrulama iletişim kuralını doğrulayın. Depolama Gezgini şu anda NTLM ek lerini desteklemiyor.

## <a name="unable-to-retrieve-children-error-message"></a>"Çocuk Alınamıyor" hata iletisi

Bir proxy aracılığıyla Azure'a bağlıysanız, proxy ayarlarınızın doğru olduğundan doğru olun. Aboneliğin veya hesabın sahibinden bir kaynağa erişim izni aldıysanız, bu kaynak için izinleri okuduğunuzdan veya listelediğinizi doğrulayın.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>Bağlantı dizesi tam yapılandırma ayarlarına sahip değil

Bu hata iletisini alırsanız, depolama hesabınız için anahtarları almak için gerekli izinlere sahip değil olabilirsiniz. Durumun böyle olduğunu doğrulamak için portala gidin ve depolama hesabınızı bulun. Bunu, depolama hesabınız için düğüme sağ tıklayarak ve **Portal'da Aç'ı**seçerek yapabilirsiniz. Ardından **Access Keys** bıçağına gidin. Anahtarları görüntüleme izniniz yoksa, "Erişiminiz yok" iletisi görürsünüz. Bu sorunu çözmek için, hesap anahtarını başka birinden alabilir ve ad ve anahtar alabilirsiniz veya bir kişiden depolama hesabına SAS isteyip depolama hesabını eklemek için kullanabilirsiniz.

Hesap anahtarlarını görürseniz, sorunu çözmenize yardımcı olmamız için GitHub'da bir sorun dosyalayın.

## <a name="error-occurred-while-adding-new-connection-typeerror-cannot-read-property-version-of-undefined"></a>Yeni bağlantı eklerken hata oluştu: TypeError: Tanımlanmamış özelliği 'sürümü' okuyamıyorum

Özel bir bağlantı eklemeye çalıştığınızda bu hata iletisini alırsanız, yerel kimlik bilgisi yöneticisinde depolanan bağlantı verileri bozulabilir. Bu sorunu çözmek için bozuk yerel bağlantılarınızı silmeyi deneyin ve sonra yeniden ekleyin:

1. Depolama Gezgini'ni başlatın. Menüden Geliştirici**Araçları Toggle** **Yardım** > gidin.
2. Açılan pencerede, **Uygulama** sekmesinde, **yerel depolama** (sol taraf) **> file://** gidin.
3. Sorun yaşadığınız bağlantının türüne bağlı olarak, anahtarını arayın ve değerini metin düzenleyicisine kopyalayın. Değer, aşağıdaki gibi özel bağlantı adlarınızın bir dizisidir:
    * Depolama hesapları
        * `StorageExplorer_CustomConnections_Accounts_v1`
    * Blob kaplar
        * `StorageExplorer_CustomConnections_Blobs_v1`
        * `StorageExplorer_CustomConnections_Blobs_v2`
    * Dosya paylaşımları
        * `StorageExplorer_CustomConnections_Files_v1`
    * Kuyruklar
        * `StorageExplorer_CustomConnections_Queues_v1`
    * Tablolar
        * `StorageExplorer_CustomConnections_Tables_v1`
4. Geçerli bağlantı adlarınızı kurtardıktan sonra Geliştirici Araçları'ndaki değeri ' ye `[]`ayarlayın.

Bozuk olmayan bağlantıları korumak istiyorsanız, bozuk bağlantıları bulmak için aşağıdaki adımları kullanabilirsiniz. Varolan tüm bağlantıları kaybetmenin sakıncası yoksa, bu adımları atlayabilir ve bağlantı verilerinizi temizlemek için platforma özgü yönergeleri izleyebilirsiniz.

1. Bir metin düzenleyicisinden, her bağlantı adını Geliştirici Araçları'na yeniden ekleyin ve ardından bağlantının hala çalışıp çalışmadığını denetleyin.
2. Bir bağlantı düzgün çalışıyorsa, bozuk değildir ve güvenli bir şekilde orada bırakabilirsiniz. Bir bağlantı çalışmıyorsa, değerini Geliştirici Araçları'ndan kaldırın ve daha sonra tekrar ekleyebilmeniz için kaydedin.
3. Tüm bağlantılarınızı inceleyene kadar tekrarlayın.

Tüm bağlantılarınızı inceledikten sonra, geri eklenmemiş tüm bağlantı adları için bozuk verilerini temizlemeniz (varsa) temizlemeniz ve Depolama Gezgini'ndeki standart adımları kullanarak bunları geri eklemeniz gerekir:

# <a name="windows"></a>[Windows](#tab/Windows)

1. **Başlat** **menüsünde, Kimlik Bilgisi Yöneticisi'ni** arayın ve açın.
2. Windows **Kimlik Bilgileri'ne**gidin.
3. **Genel Kimlik Bilgileri**altında, `<connection_type_key>/<corrupted_connection_name>` anahtara sahip girişleri `StorageExplorer_CustomConnections_Accounts_v1/account1`arayın (örneğin, ).
4. Bu girişleri silin ve bağlantıları yeniden ekleyin.

# <a name="macos"></a>[Macos](#tab/macOS)

1. Spotlight'ı (Komut+Boşluk Çubuğu) açın ve **Anahtarlık erişimini**arayın.
2. Anahtara `<connection_type_key>/<corrupted_connection_name>` sahip girişleri arayın (örneğin, `StorageExplorer_CustomConnections_Accounts_v1/account1`).
3. Bu girişleri silin ve bağlantıları yeniden ekleyin.

# <a name="linux"></a>[Linux](#tab/Linux)

Yerel kimlik bilgisi yönetimi Linux dağıtımına bağlı olarak değişir. Linux dağıtımınız yerel kimlik bilgileri yönetimi için yerleşik bir GUI aracı sağlamazsa, yerel kimlik bilgilerinizi yönetmek için bir üçüncü taraf aracı yükleyebilirsiniz. Örneğin, Linux yerel kimlik bilgilerini yönetmek için açık kaynak gui aracı olan [Seahorse'u](https://wiki.gnome.org/Apps/Seahorse/)kullanabilirsiniz.

1. Yerel kimlik bilgileri yönetim aracınızı açın ve kayıtlı kimlik bilgilerinizi bulun.
2. Anahtara `<connection_type_key>/<corrupted_connection_name>` sahip girişleri arayın (örneğin, `StorageExplorer_CustomConnections_Accounts_v1/account1`).
3. Bu girişleri silin ve bağlantıları yeniden ekleyin.
---

Bu adımları çalıştırdıktan sonra bu hatayla karşılaşırsanız veya bağlantıları bozmuş olduğundan şüphelendiğiniz bir hatayı paylaşmak istiyorsanız, GitHub sayfamızda [bir sorun açın.](https://github.com/microsoft/AzureStorageExplorer/issues)

## <a name="issues-with-sas-url"></a>SAS URL ile ilgili sorunlar

Bir hizmete SAS URL üzerinden bağlanıyorve bir hata yaşıyorsanız:

* URL'nin kaynakları okumak veya listelemek için gerekli izinleri sağladığını doğrulayın.
* URL'nin süresinin dolmadığını doğrulayın.
* SAS URL bir erişim ilkesini temel alsa, erişim ilkesinin iptal edilmemiş olduğundan doğrulayın.

Geçersiz bir SAS URL'si kullanarak yanlışlıkla iliştirildiyseniz ve şimdi ayıramıyorsanız, aşağıdaki adımları izleyin:

1. Depolama Gezgini'ni çalıştırırken, Geliştirici Araçları penceresini açmak için F12 tuşuna basın.
2. **Uygulama** sekmesinde, soldaki ağaçta **Yerel Depolama** > **file://'ni** seçin.
3. Sorunlu SAS URI'nin hizmet türüyle ilişkili anahtarı bulun. Örneğin, kötü SAS URI bir blob kapsayıcı içinise, `StorageExplorer_AddStorageServiceSAS_v1_blob`adlı anahtarı arayın.
4. Anahtarın değeri bir JSON dizisi olmalıdır. Kötü URI ile ilişkili nesneyi bulun ve sonra silin.
5. Depolama Gezgini'ni yeniden yüklemek için Ctrl+R tuşuna basın.

## <a name="linux-dependencies"></a>Linux bağımlılıkları

Depolama Explorer 1.10.0 ve daha sonra Snap Store'dan bir çırpıda kullanılabilir. Depolama Gezgini tutturma tüm bağımlılıklarını otomatik olarak yükler ve snap'in yeni bir sürümü kullanılabilir olduğunda güncellenir. Depolama Gezgini'ni yükleme işlemi önerilen yükleme yöntemidir.

Depolama Gezgini, Depolama Gezgini'nin düzgün çalışması için el ile bağlanmanız gerekebilecek bir parola yöneticisi nin kullanılmasını gerektirir. Aşağıdaki komutu çalıştırarak Storage Explorer'ı sisteminizin parola yöneticisine bağlayabilirsiniz:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Uygulamayı .tar.gz dosyası olarak da indirebilirsiniz, ancak bağımlılıkları el ile yüklemeniz gerekir.

> [!IMPORTANT]
> .tar.gz indirmesinde sağlanan Depolama Gezgini yalnızca Ubuntu dağıtımları için desteklenir. Diğer dağıtımlar doğrulanmadı ve alternatif veya ek paketler gerektirebilir.

Bu paketler Linux'ta Storage Explorer için en yaygın gereksinimlerdir:

* [.NET Core 2.2 Çalışma Süresi](/dotnet/core/install/dependencies?tabs=netcore22&pivots=os-linux)
* `libgconf-2-4`
* `libgnome-keyring0` veya `libgnome-keyring-dev`
* `libgnome-keyring-common`

> [!NOTE]
> Depolama Gezgini sürüm 1.7.0 ve önceki gerektirir .NET Core 2.0. .NET Core'un daha yeni bir sürümü yüklüyse, [Depolama Gezgini'ni düzeltmeniz](#patching-storage-explorer-for-newer-versions-of-net-core)gerekir. Depolama Gezgini 1.8.0 veya daha yeni bir süre çalıştırıyorsanız, .NET Core 2.2'ye kadar kullanabilmelisiniz. 2.2'nin dışındaki sürümlerin şu anda çalışmak üzere doğrulanmadı.

# <a name="ubuntu-1904"></a>[Ubuntu 19.04](#tab/1904)

1. Depolama Explorer'ı karşıdan yükleyin.
2. [.NET Çekirdek Çalışma Süresini](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu19-04/runtime-current)yükleyin.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring0
   ```

# <a name="ubuntu-1804"></a>[Ubuntu 18.04](#tab/1804)

1. Depolama Explorer'ı karşıdan yükleyin.
2. [.NET Çekirdek Çalışma Süresini](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-current)yükleyin.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring-common libgnome-keyring0
   ```

# <a name="ubuntu-1604"></a>[Ubuntu 16.04](#tab/1604)

1. Depolama Explorer'ı karşıdan yükleyin.
2. [.NET Çekirdek Çalışma Süresini](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-current)yükleyin.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

# <a name="ubuntu-1404"></a>[Ubuntu 14.04](#tab/1404)

1. Depolama Explorer'ı karşıdan yükleyin.
2. [.NET Çekirdek Çalışma Süresini](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-current)yükleyin.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```
---

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>.NET Core'un yeni sürümleri için Depolama Gezgini'ni yamalama

Depolama Gezgini 1.7.0 veya daha önce için, Depolama Gezgini tarafından kullanılan .NET Core sürümünü düzeltmeniz gerekebilir:

1. [NuGet](https://www.nuget.org/packages/StreamJsonRpc/1.5.43)streamJsonRpc sürümü 1.5.43 indirin. Sayfanın sağ tarafındaki "Paketi İndir" bağlantısını arayın.
2. Paketi indirdikten sonra dosya uzantısını `.nupkg` `.zip`' dan ' a değiştirin.
3. Paketin zip'ini aç.
4. Klasörü `streamjsonrpc.1.5.43/lib/netstandard1.1/` açın.
5. Depolama `StreamJsonRpc.dll` Gezgini klasöründeki aşağıdaki konumlara kopyalayın:
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-the-azure-portal-doesnt-work"></a>Azure portalından "Explorer'da Aç" çalışmıyor

Azure portalındaki **Explorer'da Aç** düğmesi çalışmıyorsa, uyumlu bir tarayıcı kullandığınızdan emin olun. Aşağıdaki tarayıcılar uyumluluk için test edilmiştir:
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Sonraki adımlar

Bu çözümlerin hiçbiri sizin için çalışmıyorsa, [GitHub'da bir sorun açın.](https://github.com/Microsoft/AzureStorageExplorer/issues) Bunu, sol alt köşedeki **GitHub düğmesine Rapor sorunu** seçeneğini seçerek de yapabilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
