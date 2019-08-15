---
title: Azure Depolama Gezgini sorun giderme kılavuzu | Microsoft Docs
description: Azure Depolama Gezgini için hata ayıklama tekniklerine genel bakış
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 96a8eab57f1714eed4831bea01508e9140d1dfad
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934983"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Depolama Gezgini sorun giderme kılavuzu

Microsoft Azure Depolama Gezgini, Windows, macOS ve Linux 'ta Azure Depolama verileriyle kolayca çalışabilmenizi sağlayan tek başına bir uygulamadır. Uygulama, Azure, Ulusal bulutlar ve Azure Stack barındırılan depolama hesaplarına bağlanabilir.

Bu kılavuz, Depolama Gezgini görülen yaygın sorunların çözümlerini özetler.

## <a name="role-based-access-control-permission-issues"></a>Rol tabanlı Access Control Izin sorunları

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) , _Roller_Için izin kümelerini birleştirerek Azure kaynakları üzerinde ayrıntılı erişim yönetimi sağlar. İşte Depolama Gezgini ' de çalışan RBAC 'yi almak için izleyebileceğiniz bazı öneriler aşağıda verilmiştir.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>Depolama Gezgini kaynaklarımı görmem gerekir mi?

RBAC kullanarak depolama kaynaklarına erişirken sorun yaşıyorsanız, bunun nedeni uygun rollere atanmamanız olabilir. Aşağıdaki bölümlerde, şu anda depolama kaynaklarınıza erişmesi için gereken Depolama Gezgini izinleri açıklanır.

Uygun rollere veya izinlere sahip olduğunuzdan emin değilseniz Azure hesap yöneticinize başvurun.

#### <a name="read-listget-storage-accounts"></a>Okuyamaz Depolama Hesaplarını Listele/Al

Depolama hesaplarını listelemek için izninizin olması gerekir. Bu izni, "okuyucu" rolüne atanarak alabilirsiniz.

#### <a name="list-storage-account-keys"></a>Depolama Hesabı Anahtarlarını Listele

Depolama Gezgini, isteklerin kimliğini doğrulamak için hesap anahtarlarını da kullanabilir. "Katkıda bulunan" rolü gibi daha güçlü rollerle anahtarlara erişim sağlayabilirsiniz.

> [!NOTE]
> Erişim tuşları, bunları tutan herkese Kısıtlanmamış izinler verir. Bu nedenle, genellikle hesap kullanıcılarına verilmeleri önerilmez. Erişim anahtarlarını iptal etmeniz gerekirse, bunları [Azure portalından](https://portal.azure.com/)yeniden oluşturabilirsiniz.

#### <a name="data-roles"></a>Veri rolleri

Kaynaklardan veri okuma erişimi veren en az bir rol atanması gerekir. Örneğin, Blobları listeetmeniz veya indirmeniz gerekiyorsa, en azından "Depolama Blobu veri okuyucu" rolüne sahip olmanız gerekir.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Kaynaklarımı Depolama Gezgini görmek için neden bir yönetim katmanı rolüne ihtiyacım var?

Azure Storage iki erişim katmanına sahiptir: _Yönetim_ ve _veri_. Abonelikler ve depolama hesaplarına yönetim katmanı üzerinden erişilir. Kapsayıcılar, Bloblar ve diğer veri kaynaklarına veri katmanı üzerinden erişilir. Örneğin, Azure 'daki depolama hesaplarınızın bir listesini almak istiyorsanız Yönetim uç noktasına bir istek gönderirsiniz. Bir hesapta blob kapsayıcıları listesini isterseniz, uygun hizmet uç noktasına bir istek gönderirsiniz.

RBAC rolleri, yönetim veya veri katmanı erişimi için izinler içerebilir. Örneğin, "okuyucu" rolü, size salt okunurdur erişim yönetim katmanı kaynakları verir.

Kesinlikle konuşuyor, "okuyucu" rolü veri katmanı izinleri sağlamaz ve veri katmanına erişmek için gerekli değildir.

Depolama Gezgini, sizin için Azure kaynaklarınıza bağlanmak üzere gerekli bilgileri toplayıp kaynaklarınıza erişmeyi kolaylaştırır. Örneğin, blob Kapsayıcılarınızı göstermek için Depolama Gezgini blob hizmeti uç noktasına bir liste kapsayıcıları isteği gönderir. Bu uç noktayı almak için Depolama Gezgini, erişiminiz olan aboneliklerin ve depolama hesaplarının listesini arar. Ancak, aboneliklerinizi ve depolama hesaplarınızı bulmak için Depolama Gezgini yönetim katmanına erişime de ihtiyaç duyuyor.

Herhangi bir yönetim katmanı izinleri veren bir rolünüz yoksa Depolama Gezgini veri katmanına bağlanması için gereken bilgileri alamaz.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>Yöneticimde ihtiyacım olan yönetim katmanı izinlerini alamazsanız ne yapmalıyım?

Şu anda RBAC ile ilgili bir çözümünüz henüz yok. Geçici bir çözüm olarak, [kaynağına eklemek](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#use-a-sas-uri)IÇIN BIR SAS URI 'si isteyebilirsiniz.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hata: Sertifika zincirindeki otomatik olarak Imzalanan sertifika (ve benzer hatalar)

Sertifika hataları aşağıdaki iki durumlardan biri nedeniyle oluşur:

1. Uygulama bir "saydam proxy" aracılığıyla bağlanır, yani bir sunucu (örneğin, şirket sunucunuz) HTTPS trafiğini kesintiye uğratır, şifrelerini çözerek otomatik olarak imzalanan bir sertifika kullanarak şifreliyor.
2. Aldığınız HTTPS iletilerine otomatik olarak imzalanan bir SSL sertifikası ekleme bir uygulama çalıştırıyorsunuz. Sertifika ekleme yapan uygulamalara örnek olarak virüsten koruma ve ağ trafiği inceleme yazılımı dahildir.

Depolama Gezgini kendinden imzalı veya güvenilmeyen bir sertifikayı gördüğünde, alınan HTTPS iletisinin değiştirilip değiştirilmediğini artık bilmez. Otomatik olarak imzalanan sertifikanın bir kopyasına sahipseniz, aşağıdaki adımları uygulayarak Depolama Gezgini bu sertifikaya güvenmesini isteyebilirsiniz:

1. Sertifikanın Base-64 kodlamalı X. 509.440 (. cer) kopyasını edinin
2. **SSL sertifikalarını** > Düzenle > **sertifikaları içeri aktar**' a tıklayın ve ardından dosya seçiciyi kullanarak. cer dosyasını bulun, seçin ve açın

Bu sorun birden çok sertifikanın (kök ve ara) sonucu da olabilir. Hatanın üstesinden gelmek için her iki sertifikanın de eklenmesi gerekir.

Sertifikanın nereden geldiği konusunda emin değilseniz, bulmak için aşağıdaki adımları deneyebilirsiniz:

1. Açık SSL yükleme
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (hafif sürümlerden herhangi biri yeterli olmalıdır)
    * Mac ve Linux: işletim sisteminize eklenmelidir
2. Açık SSL çalıştırma
    * Windows: yükleme dizinini açın, **/bin/** ' ye tıklayın ve **OpenSSL. exe**' ye çift tıklayın.
    * Mac ve Linux: bir terminalden **OpenSSL** çalıştırın.
3. `s_client -showcerts -connect microsoft.com:443` yürütme
4. Otomatik olarak imzalanan sertifikaları bulun. Hangi sertifikaların kendinden imzalandığından emin değilseniz, konunun `("s:")` ve verenin `("i:")` aynı olduğu her yerde arayın.
5. Her biri için kendinden imzalı bir sertifika bulduğunuz zaman, **-----Başlangıç sertifikası-----** yeni bir. cer dosyasına **-----son-----sertifikaya** ekleyerek ve dahil olmak üzere her şeyi kopyalayıp yapıştırın.
6. Depolama Gezgini açın,**SSL sertifikalarını** >  **Düzenle** > **sertifikaları içeri aktar**' a tıklayın ve sonra oluşturduğunuz. cer dosyalarını bulmak, seçmek ve açmak için dosya seçiciyi kullanın.

Yukarıdaki adımları kullanarak otomatik olarak imzalanan sertifikalar bulamıyorsanız, daha fazla yardım için geri bildirim aracı aracılığıyla bizimle iletişim kurun. Ayrıca, `--ignore-certificate-errors` bayrağı ile komut satırından Depolama Gezgini başlatmayı da tercih edebilirsiniz. Bu bayrağıyla başlatıldığında Depolama Gezgini sertifika hatalarını yoksayar.

## <a name="sign-in-issues"></a>Oturum açma sorunları

### <a name="blank-sign-in-dialog"></a>Boş oturum açma Iletişim kutusu

Boş oturum açma iletişim kutuları genellikle ADFS tarafından desteklenmeyen Depolama Gezgini bir yeniden yönlendirme gerçekleştirmesini istiyor. Bu sorunu geçici olarak çözmek için, oturum açma için cihaz kod akışını kullanmayı deneyebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin:

1. Menü Önizleme-> "cihaz kodu oturum açma kullan".
2. Bağlan Iletişim kutusunu açın (sol taraftaki dikey çubukta bulunan tak simgesi aracılığıyla veya hesap panelinde "Hesap Ekle").
3. Oturum açmak istediğiniz ortamı seçin.
4. "Oturum aç" düğmesine tıklayın.
5. Sonraki bölmede yer alan yönergeleri izleyin.

Varsayılan tarayıcınız zaten farklı bir hesapta oturum açtığı için kullanmak istediğiniz hesapta oturum açma konusunda sorun yaşadıysanız şunlardan birini yapabilirsiniz:

1. Bağlantıyı ve kodu tarayıcınızın özel oturumuna el ile kopyalayın.
2. Bağlantıyı ve kodu farklı bir tarayıcıya el ile kopyalayın.

### <a name="reauthentication-loop-or-upn-change"></a>Yeniden kimlik doğrulama döngüsü veya UPN değişikliği

Bir yeniden kimlik doğrulama döngüsüyle veya hesaplarınızdan birinin UPN 'sini değiştirdiyseniz, aşağıdaki adımları deneyin:

1. Tüm hesapları kaldırın ve ardından Depolama Gezgini kapatın
2. Öğesini silin. Makinenizden IdentityService klasörü. Windows üzerinde, klasörü konumunda `C:\users\<username>\AppData\Local`bulunur. Mac ve Linux için, klasörü Kullanıcı dizininizin kökünde bulabilirsiniz.
3. Mac veya Linux kullanıyorsanız, işletim sistemi ' keystore ' dan Microsoft. Developer. IdentityService girişini de silmeniz gerekir. Mac üzerinde, anahtar deposu "Gnome anahtarlığı" uygulamasıdır. Linux için, uygulamaya genellikle "kimlik anahtarlığı" denir, ancak ad, dağıtıma bağlı olarak farklı olabilir.

### <a name="conditional-access"></a>Koşullu Erişim

Windows 10, Linux veya macOS 'ta Depolama Gezgini kullanılırken koşullu erişim desteklenmez. Bunun nedeni, Depolama Gezgini tarafından kullanılan AAD kitaplığındaki bir kısıtlamadır.

## <a name="mac-keychain-errors"></a>Mac Anahtarlık hataları

MacOS anahtarlığı bazen Depolama Gezgini kimlik doğrulama kitaplığı için sorunlara neden olan bir duruma alabilir. Anahtarlığı bu durumdan dışarı almak için aşağıdaki adımları deneyin:

1. Depolama Gezgini kapatın.
2. Anahtarlık açın (**cmd + Space**, anahtarlık olarak yazın, ENTER tuşuna basın).
3. "Oturum açma" anahtarışını seçin.
4. Anahtarlık kilidini kilitlemek için asma kilit simgesine tıklayın (işlem tamamlandığında bir kilitli konuma animasyon uygular, bu işlem, açtığınız uygulamalara bağlı olarak birkaç saniye sürebilir).

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Depolama Gezgini başlatın.
6. "Hizmet hub 'ı anahtarlığa erişmek istiyor" gibi bir açılır pencere görünür. Bu durumda, Mac yönetici hesabı parolanızı girip **her zaman izin** ver ' e tıklayın (veya **her zaman izin** ver ' e **izin ver** ).
7. Oturum açmayı deneyin.

### <a name="general-sign-in-troubleshooting-steps"></a>Genel oturum açma sorunlarını giderme adımları

* MacOS kullanıyorsanız ve oturum açma penceresi "kimlik doğrulaması bekleniyor..." üzerinde hiçbir şekilde görüntülenmiyor iletişim kutusunda [şu adımları](#mac-keychain-errors) deneyin
* Depolama Gezgini yeniden Başlat
* Kimlik doğrulama penceresi boşsa, kimlik doğrulama iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarlarınızın hem makineniz hem de Depolama Gezgini için düzgün yapılandırıldığından emin olun.
* Windows kullanıyorsanız ve aynı makinede Visual Studio 2019 ' e erişiminiz varsa ve oturum açarsanız, Visual Studio 2019 ' de oturum açmayı deneyin. Visual Studio 2019 ' de başarılı bir oturum açma işleminden sonra, Depolama Gezgini açıp hesabınızı hesap panelinde görebilirsiniz.

Bu yöntemlerin hiçbiri çalışmazsa [GitHub 'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Eksik abonelikler ve bozuk kiracılar

Başarıyla oturum açtıktan sonra aboneliklerinizi alamadıysanız aşağıdaki sorun giderme yöntemlerini deneyin:

* Hesabınızın, bekleyen aboneliklere erişimi olduğunu doğrulayın. Kullanmaya çalıştığınız Azure ortamı için Portal oturumu açarak erişiminizi doğrulayabilirsiniz.
* Doğru Azure ortamını (Azure, Azure Çin 21Vianet, Azure Almanya, Azure ABD kamu veya özel ortam) kullanarak oturum açtığınızdan emin olun.
* Bir proxy 'nin arkasındaysanız, Depolama Gezgini proxy 'yi doğru şekilde yapılandırdığınızdan emin olun.
* Hesabı kaldırmayı ve kullanmayı deneyin.
* "Daha fazla bilgi" bağlantısı varsa, başarısız olan kiracılar için hangi hata iletilerinin raporlandığını göz atın ve görün. Gördüğünüz hata iletileriyle ne yapabileceğinize emin değilseniz, [GitHub 'da bir sorun açmayı](https://github.com/Microsoft/AzureStorageExplorer/issues)ücretsiz olarak kullanabilirsiniz.

## <a name="cant-remove-attached-account-or-storage-resource"></a>Eklenmiş hesap veya depolama kaynağı kaldırılamıyor

Kullanıcı arabirimi aracılığıyla ekli bir hesabı veya depolama kaynağını kaldıramıyoruz, aşağıdaki klasörleri silerek tüm bağlı kaynakları el ile silebilirsiniz:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> Yukarıdaki klasörleri silmeden önce Depolama Gezgini kapatın.

> [!NOTE]
> Herhangi bir SSL sertifikası aldıysanız, `certs` dizinin içeriğini yedekleyin. Daha sonra, SSL sertifikalarınızı yeniden içeri aktarmak için yedekleme 'yi kullanabilirsiniz.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, girdiğiniz aşağıdaki bilgilerin tamamen doğru olduğundan emin olun:

* Proxy URL 'SI ve bağlantı noktası numarası
* Proxy için gerekliyse Kullanıcı adı ve parola

> [!NOTE]
> Depolama Gezgini, proxy ayarlarını yapılandırmak için proxy otomatik yapılandırma dosyalarını desteklemez.

### <a name="common-solutions"></a>Ortak çözümler

Sorun yaşamaya devam ediyorsanız, aşağıdaki sorun giderme yöntemlerini deneyin:

* Proxy 'nizi kullanmadan Internet 'e bağlanabildiğinizi, Depolama Gezgini proxy ayarları etkin olmadan çalıştığını doğrulayın. Bu durumda, ara sunucu ayarlarınızda bir sorun olabilir. Sorunları belirlemek için proxy yöneticinizle birlikte çalışın.
* Proxy sunucusunu kullanan diğer uygulamaların beklendiği gibi çalıştığını doğrulayın.
* Kullanmaya çalıştığınız Azure ortamı için portala bağlanabildiğinizi doğrulayın
* Hizmet uç noktaınızdan yanıt alabildiğinizi doğrulayın. Tarayıcınızın uç nokta URL 'Lerinden birini girin. Bağlantı kurmak için bir InvalidQueryParameterValue veya benzer bir XML yanıtı almanız gerekir.
* Başka biri de ara sunucu ile Depolama Gezgini kullanıyorsa, bağlanabildiğini doğrulayın. Bağlanabiliyorlarsa ara sunucu yöneticinize başvurmanız gerekebilir.

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılamaya yönelik araçlar

Windows için Fiddler gibi ağ araçlarınız varsa, sorunları aşağıdaki gibi tanılayabilirsiniz:

* Proxy 'niz üzerinden çalışmanız gerekiyorsa, ağ iletişimi aracınızı proxy üzerinden bağlanacak şekilde yapılandırmanız gerekebilir.
* Ağ aracınız tarafından kullanılan bağlantı noktası numarasını kontrol edin.
* Depolama Gezgini ' deki proxy ayarları olarak yerel ana bilgisayar URL 'sini ve ağ aracının bağlantı noktası numarasını girin. Doğru şekilde tamamlandığında, ağ aracınız, yönetim ve hizmet uç noktalarına Depolama Gezgini tarafından yapılan ağ isteklerini günlüğe kaydetmeye başlar. Örneğin, bir tarayıcıda https://cawablobgrs.blob.core.windows.net/ blob uç noktanız için girin ve aşağıdakine benzer bir yanıt alırsınız; ancak, bu kaynağa erişmeniz mümkün olsa da kaynağın var olduğunu öneren bir yanıt alırsınız.

![kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Proxy Sunucu Yöneticisi ile iletişim kurun

Proxy ayarlarınız doğruysa, proxy sunucu yöneticinize başvurmanız gerekebilir ve

* Proxy 'nizin Azure yönetimine veya kaynak uç noktalarına giden trafiği engellemediğinden emin olun.
* Proxy sunucunuz tarafından kullanılan kimlik doğrulama protokolünü doğrulayın. Depolama Gezgini Şu anda NTLM proxy 'leri desteklemiyor.

## <a name="unable-to-retrieve-children-error-message"></a>"Alt öğeler alınamıyor" hata iletisi

Azure 'a bir proxy üzerinden bağlıysanız, proxy ayarlarınızın doğru olduğundan emin olun. Aboneliğin veya hesabın sahibinden bir kaynağa erişim izni verildiyse, bu kaynak için okuma veya listeleme izinlerine sahip olduğunuzu doğrulayın.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>Bağlantı dizesinin yapılandırma ayarları Tamam

Bu hata iletisini alırsanız, depolama hesabınız için anahtarları almak için gerekli izinlere sahip olmanız mümkün değildir. Durumun bu olup olmadığını doğrulamak için portala gidin ve depolama hesabınızı bulun. Depolama hesabınızın düğümüne sağ tıklayıp "portalda aç" ' a tıklayarak bunu hızlıca yapabilirsiniz. Bunu yaptıktan sonra, "erişim tuşları" dikey penceresine gidin. Anahtarları görüntüleme izniniz yoksa, "erişiminiz yok" iletisini içeren bir sayfa görürsünüz. Bu sorunu geçici olarak çözmek için, hesap anahtarını başka bir kişiden edinebilir ve ad ve anahtarla iliştirebilirsiniz ya da bir kullanıcıdan depolama hesabı için bir SAS isteyebilir ve depolama hesabını eklemek için bunu kullanmasını sağlayabilirsiniz.

Hesap anahtarlarını görürseniz, sorunu çözmenize yardımcı olması için GitHub 'da bir sorun çözün.

## <a name="issues-with-sas-url"></a>SAS URL 'SI ile ilgili sorunlar

Bir SAS URL 'SI kullanarak bir hizmete bağlanıyorsanız ve bu hatayla karşılaşırsanız:

* URL 'nin kaynakları okumak veya listelemek için gerekli izinleri sağladığını doğrulayın.
* URL 'nin süresi dolmadığından emin olun.
* SAS URL 'SI bir erişim ilkesini temel alıyorsa, erişim ilkesinin iptal edilmediğini doğrulayın.

Yanlışlıkla geçersiz bir SAS URL 'SI kullanarak iliştirildiğinde ve ayıramazsınız, şu adımları izleyin:

1. Depolama Gezgini çalıştırılırken, geliştirici araçları penceresini açmak için F12 tuşuna basın.
2. Uygulama sekmesine tıklayın ve ardından sol taraftaki ağaçta yerel depolama > file://' ye tıklayın.
3. Sorunlu SAS URI 'sinin hizmet türüyle ilişkili anahtarı bulun. Örneğin, hatalı SAS URI 'SI bir blob kapsayıcısı için ise adlı `StorageExplorer_AddStorageServiceSAS_v1_blob`anahtarı bulun.
4. Anahtarın değeri bir JSON dizisi olmalıdır. Hatalı URI ile ilişkili nesneyi bulun ve kaldırın.
5. Depolama Gezgini yeniden yüklemek için CTRL + R tuşlarına basın.

## <a name="linux-dependencies"></a>Linux bağımlılıkları

<!-- Storage Explorer 1.9.0 and later is available as a snap from the Snap Store. The Storage Explorer snap installs all of its dependencies with no extra hassle.

Storage Explorer requires the use of a password manager, which may need to be connected manually before Storage Explorer will work correctly. You can connect Storage Explorer to your system's password manager with the following command:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

You can also download the application .tar.gz file, but you'll have to install dependencies manually. -->

> [!IMPORTANT]
> . Tar. gz indirmesi içinde sağlandığı gibi Depolama Gezgini yalnızca Ubuntu dağıtımları için desteklenir. Diğer dağıtımlar doğrulanmadı ve alternatif veya ek paketler gerektirebilir.

Bu paketler, Linux üzerinde Depolama Gezgini için en yaygın gereksinimlerdir:

* [.NET Core 2,0 çalışma zamanı](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libgconf-2-4`
* `libgnome-keyring0` veya `libgnome-keyring-dev`
* `libgnome-keyring-common`

> [!NOTE]
> Depolama Gezgini Version 1.7.0 ve önceki sürümleri .NET Core 2,0 gerektirir. Daha yeni bir .NET Core sürümüne sahipseniz [Depolama Gezgini düzeltme eki](#patching-storage-explorer-for-newer-versions-of-net-core)uygulamanız gerekir. Depolama Gezgini 1.8.0 veya sonraki bir sürümü çalıştırıyorsanız, .NET Core 2,2 ' e kadar kullanabilmeniz gerekir. 2,2 ' den fazla sürüm şu anda çalışacak şekilde doğrulanmadı.

# <a name="ubuntu-1904tab1904"></a>[Ubuntu 19,04](#tab/1904)

1. Depolama Gezgini indirin.
2. [.NET Core çalışma zamanını](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu19-04/runtime-current)yükler.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring0
   ```

# <a name="ubuntu-1804tab1804"></a>[Ubuntu 18,04](#tab/1804)

1. Depolama Gezgini indirin.
2. [.NET Core çalışma zamanını](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-current)yükler.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring-common libgnome-keyring0
   ```

# <a name="ubuntu-1604tab1604"></a>[Ubuntu 16,04](#tab/1604)

1. Depolama Gezgini indir
2. [.NET Core çalışma zamanını](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-current)yükler.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

# <a name="ubuntu-1404tab1404"></a>[Ubuntu 14,04](#tab/1404)

1. Depolama Gezgini indir
2. [.NET Core çalışma zamanını](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-current)yükler.
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```
---

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>.NET Core 'un daha yeni sürümleri için Depolama Gezgini düzeltme eki uygulama

Depolama Gezgini 1.7.0 veya üzeri için, Depolama Gezgini tarafından kullanılan .NET Core sürümüne yama yapmanız gerekebilir.

1. [NuGet 'Den](https://www.nuget.org/packages/StreamJsonRpc/1.5.43)StreamJsonRpc 'nin 1.5.43 sürümünü indirin. Sayfanın sağ tarafındaki "paketi Indir" bağlantısını bulun.
2. Paketi indirdikten sonra, dosya uzantısını `.nupkg` olarak `.zip`değiştirin.
3. Paketi sıkıştırmayı açın.
4. Açık `streamjsonrpc.1.5.43/lib/netstandard1.1/` klasör.
5. Depolama Gezgini `StreamJsonRpc.dll` klasörü içinde aşağıdaki konumlara kopyalayın:
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-azure-portal-doesnt-work"></a>Azure portal çalışmazsa Explorer 'Da aç

Azure portal ' Explorer 'Da aç "düğmesi sizin için çalışmazsa, uyumlu bir tarayıcı kullandığınızdan emin olun. Aşağıdaki tarayıcılar uyumluluk için sınanmıştır.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Sonraki adımlar

Çözümlerden hiçbiri sizin için işe çalışmadıysanız, [GitHub 'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues). Sol alt köşedeki "GitHub 'a sorun bildir" düğmesini kullanarak da hızlıca GitHub 'a ulaşabilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
