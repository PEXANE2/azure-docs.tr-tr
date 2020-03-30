---
title: StorSimple 8000 serisi güvenlik | Microsoft Dokümanlar
description: StorSimple hizmetinizi, cihazınızı ve verilerinizi şirket içinde ve bulutta koruyan güvenlik ve gizlilik özelliklerini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2018
ms.author: alkohli
ms.openlocfilehash: 4598f71f9b611e68f8eb00676138784833c39f32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75891507"
---
# <a name="storsimple-security-and-data-protection"></a>StorBasit güvenlik ve veri koruması

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

Güvenlik, özellikle teknoloji gizli veya özel verilerle kullanıldığında, yeni bir teknolojiyi benimseyen herkes için önemli bir sorundur. Farklı teknolojileri değerlendirirken, veri koruması için artan riskleri ve maliyetleri göz önünde bulundurmanız gerekir. Microsoft Azure StorSimple, veri koruması için hem güvenlik hem de gizlilik çözümü sağlayarak şunları sağlar:

* **Gizlilik** – Verilerinizi yalnızca yetkili kuruluşlar görüntüleyebilir.
* **Bütünlük** – Verilerinizi yalnızca yetkili kuruluşlar değiştirebilir veya silebilir.

Microsoft Azure StorSimple çözümü, birbiriyle etkileşim edebilen dört ana bileşenden oluşur:

* **StorSimple Device Manager hizmeti Microsoft Azure'da barındırılır** – StorSimple aygıtını yapılandırmak ve sağlamak için kullandığınız yönetim hizmeti.
* **StorSimple aygıt** – Veri merkezinizde yüklü fiziksel bir aygıt. Veri üreten tüm ana bilgisayarlar ve istemciler StorSimple aygıtına bağlanır ve aygıt verileri yönetir ve uygun şekilde Azure bulutuna taşır.
* **Aygıta bağlı istemciler/ana bilgisayarlar** – Altyapınızda StorSimple aygıtına bağlanan ve korunması gereken veriler üreten istemciler.
* **Bulut depolama** – Azure bulutunda verilerin depolandığı konum.

Aşağıdaki bölümlerde, bu bileşenlerin her birinin korunmasına yardımcı olan StorSimple güvenlik özellikleri ve bu bileşenlerde depolanan veriler açıklanır. Ayrıca, Microsoft Azure StorSimple güvenliği hakkında sorabileceğiniz soruların bir listesini ve ilgili yanıtları içerir.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple Aygıt Yöneticisi servis koruması

StorSimple Device Manager hizmeti, Microsoft Azure'da barındırılan ve kuruluşunuzun temin ettiği tüm StorSimple aygıtlarını yönetmek için kullanılan bir yönetim hizmetidir. Bir web tarayıcısı üzerinden Azure portalında oturum açmak için kuruluş kimlik bilgilerinizi kullanarak StorSimple Device Manager hizmetine erişebilirsiniz.

StorSimple Aygıt Yöneticisi hizmetine erişim, kuruluşunuzun StorSimple içeren bir Azure aboneliğine sahip olmasını gerektirir. Aboneliğiniz, Azure portalında erişebildiğiniz özellikleri yönetir. Kuruluşunuzun bir Azure aboneliği yoksa ve bunlar hakkında daha fazla bilgi edinmek istiyorsanız, [kuruluş olarak Azure'a Kaydolun'a](../active-directory/fundamentals/sign-up-organization.md)bakın.

StorSimple Device Manager hizmeti Azure'da barındırıldığı için Azure güvenlik özellikleri tarafından korunur. Microsoft Azure tarafından sağlanan güvenlik özellikleri hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/security/)’ne gidin.

## <a name="storsimple-device-protection"></a>StorBasit cihaz koruması

StorSimple cihazı, katı hal sürücüleri (SSD'ler) ve sabit disk sürücüleri (HDD'ler) ile yedek denetleyiciler ve otomatik arıza özellikleri içeren şirket içi karma depolama aygıtıdır. Denetleyiciler depolama katmanlamasını yönetir, şu anda kullanılan (veya sıcak) verileri yerel depolamaya (StorSimple aygıtında veya şirket içi sunuculara) yerleştirirken, daha az kullanılan verileri buluta taşıyor.

Azure aboneliğinizde oluşturduğunuz StorSimple Aygıt Yöneticisi hizmetine yalnızca yetkili StorSimple aygıtları katılabilir. Bir aygıtı yetkilendirmek için, servis kayıt anahtarını sağlayarak aygıtı StorSimple Device Manager hizmetine kaydettirmelisiniz. Hizmet kayıt anahtarı, Azure portalında oluşturulan 128 bitlik rastgele bir anahtardır.

![Servis kayıt anahtarı](./media/storsimple-security/ServiceRegistrationKey.png)

Servis kayıt anahtarını nasıl alın diye öğrenmek için [Adım 2'ye gidin: Servis kayıt anahtarını edinin.](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key)

Hizmet kayıt anahtarı, 100'den fazla karakter içeren uzun bir anahtardır. Anahtarı kopyalayabilir ve güvenli bir konumda bir metin dosyasına kaydedebilirsiniz, böylece gerektiğinde ek aygıtları yetkilendirmek için kullanabilirsiniz. İlk cihazınızı kaydettikten sonra servis kayıt anahtarı kaybolursa, StorSimple Device Manager hizmetinden yeni bir anahtar oluşturabilirsiniz. Bu, varolan aygıtların çalışmasını etkilemez.

Bir aygıt kaydedildikten sonra, Microsoft Azure ile iletişim kurmak için belirteçleri kullanır. Servis kayıt anahtarı cihaz kaydından sonra kullanılmaz.

> [!NOTE]
> Her kullanımdan sonra hizmet kayıt anahtarını yeniden oluşturmanızı öneririz.


## <a name="protect-your-storsimple-solution-via-passwords"></a>StorSimple çözümünüzü parolalarla koruyun

Parolalar bilgisayar güvenliğinin önemli bir yönüdür ve verilerinizin yalnızca yetkili kullanıcılar tarafından erişilebilir olmasını sağlamaya yardımcı olmak için StorSimple çözümünde yaygın olarak kullanılır. StorSimple aşağıdaki parolaları yapılandırmanızı sağlar:

* StorBasit aygıt yöneticisi parolası
* Challenge El Sıkışma Kimlik Doğrulama Protokolü (CHAP) başlatıcısı ve hedef şifreleri
* StorSimple Snapshot Manager parolası

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>StorSimple ve StorSimple aygıt yöneticisi parolası için Windows PowerShell

StorSimple için Windows PowerShell, StorSimple aygıtını yönetmek için kullanabileceğiniz bir komut satırı arabirimidir. StorSimple için Windows PowerShell, cihazınızı kaydetmenize, cihazınızdaki ağ arabirimini yapılandırmanıza, belirli güncelleştirme türlerini yüklemenize, destek oturumuna erişerek cihazınızın sorun gidermesine ve aygıt durumunu değiştirmenize olanak tanıyan özelliklere sahiptir. StorSimple için Windows PowerShell'e aygıttaki seri konsola bağlanarak veya Windows PowerShell remoting'i kullanarak erişebilirsiniz.

PowerShell remoting HTTPS veya HTTP üzerinden yapılabilir. HTTPS üzerinden uzaktan yönetim etkinse, uzaktan yönetim sertifikasını aygıttan indirmeniz ve uzak istemciye yüklemeniz gerekir. PowerShell remoting hakkında daha fazla bilgi [için, StorSimple cihazınıza uzaktan Bağlan'a](storsimple-8000-remote-connect.md)gidin.

Cihaza bağlanmak için StorSimple için Windows PowerShell'i kullandıktan sonra, aygıtta oturum açmak için aygıt yöneticisi parolasını sağlamanız gerekir.

![Cihaz yöneticisi parolası](./media/storsimple-security/DeviceAdminPW.png)

Aşağıdaki en iyi uygulamaları aklınızda bulundurun:

* Uzaktan yönetim varsayılan olarak kapatılır. Bunu etkinleştirmek için StorSimple Device Manager hizmetini kullanabilirsiniz. Güvenlik en iyi uygulama olarak, uzaktan erişim yalnızca gerçekte gerekli olan zaman diliminde etkinleştirilmelidir.
* Parolayı değiştirirseniz, beklenmeyen bir bağlantı kaybı yaşamaması için tüm uzaktan erişim kullanıcılarına bildirimde bulunun.
* StorSimple Device Manager hizmeti varolan parolaları alamaz: yalnızca bunları sıfırlayabilir. Unutulursa parolayı sıfırlamak zorunda kalmamak için tüm parolaları güvenli bir yerde saklamanızı öneririz. Parolayı sıfırlamanız gerekiyorsa, sıfırlamadan önce tüm kullanıcıları bilgilendirdiğinizden emin olun.

Aygıta seri bağlantı kullanarak Windows PowerShell arabirimine erişebilirsiniz. Ayrıca, ek güvenlik sağlayan HTTP veya HTTPS'yi kullanarak uzaktan erişebilirsiniz. HTTPS, seri veya HTTP bağlantısından daha yüksek bir güvenlik düzeyi sağlar. Ancak, HTTPS'yi kullanmak için öncelikle istemci bilgisayara aygıta erişecek bir sertifika yüklemeniz gerekir. Uzaktan erişim sertifikasını StorSimple Device Manager hizmetindeki aygıt yapılandırma sayfasından indirebilirsiniz. Uzaktan erişim sertifikası kaybolursa, yeni bir sertifika indirmeniz ve uzaktan yönetim kullanma yetkisi olan tüm istemcilere yaymanız gerekir.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Challenge El Sıkışma Kimlik Doğrulama Protokolü (CHAP) başlatıcısı ve hedef şifreleri

CHAP, Uzak istemcilerin kimliğini doğrulamak için StorSimple aygıtı tarafından kullanılan bir kimlik doğrulama şemasıdır. Doğrulama paylaşılan bir parolayı temel adatır. CHAP tek yönlü (tek yönlü) veya karşılıklı (çift yönlü) olabilir. Tek yönlü CHAP ile hedef (StorSimple aygıtı) bir başlatıcıyı (ana bilgisayar) doğrular. Karşılıklı veya ters CHAP, hedefin başlatıcıyı ve ardından başlatıcının hedefi doğrulamasını gerektirir. StorSimple'ınız her iki yöntemi de kullanacak şekilde yapılandırılabilir.

CHAP'yi yapılandırırken aşağıdakihususlara dikkat edin:

* CHAP kullanıcı adı 233 karakterden az olmalıdır.
* CHAP parolası 12 ile 16 karakter arasında olmalıdır. Daha uzun bir kullanıcı adı veya parola kullanmaya çalışmak, Windows ana bilgisayarda kimlik doğrulama hatasına neden olur.
* Hem CHAP başlatıcısı hem de CHAP hedefi için aynı parolayı kullanamazsınız.
* Parolayı ayarladıktan sonra değiştirilebilir, ancak geri alınamaz. Parola değiştirilirse, StorSimple cihazına başarılı bir şekilde bağlanabilmeleri için tüm uzaktan erişim kullanıcılarına bildirimde bulunun.

CHAP hakkında daha fazla bilgi ve StorSimple çözümünüz için nasıl yapılandırılabildiğini zedebilirsiniz, [StorSimple cihazınız için CHAP'yi Yapılandır'a](storsimple-8000-configure-chap.md)gidin.

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolası

StorSimple Snapshot Manager, uygulama tutarlı yedeklemeler oluşturmak için birim grupları ve Windows Birim Gölge Kopyalama Hizmeti kullanan bir Microsoft Yönetim Konsolu (MMC) snap-in'dir. Buna ek olarak, yedekleme zamanlamaları oluşturmak ve birimleri klonlamak veya geri yüklemek için StorSimple Snapshot Manager'ı kullanabilirsiniz.

Bir aygıtı StorSimple Snapshot Manager'ı kullanacak şekilde yapılandırdığınızda, StorSimple Snapshot Manager parolasını sağlamanız gerekir. Bu parola ilk olarak kayıt sırasında StorSimple için Windows PowerShell'de ayarlanır. Parola, StorSimple Device Manager hizmetinden de ayarlanabilir ve değiştirilebilir. Bu parola, StorSimple Snapshot Manager ile aygıtın kimliğini doğrular.

![StorSimple Snapshot Manager parolası](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple Snapshot Manager parolası 14 ila 15 karakter olmalıdır ve büyük harf, küçük harf, sayısal ve özel karakterlerin 3 veya daha fazlasını içermelidir. StorSimple Snapshot Manager parolasını ayarladıktan sonra değiştirilebilir, ancak alınamaz. Parolayı değiştirirseniz, tüm uzak kullanıcıları bilgilendirdiğinizden emin olun.

StorSimple Snapshot Manager hakkında daha fazla bilgi için [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Parolalarla ilgili en iyi yöntemler

StorSimple parolalarının güçlü ve iyi korunduğundan emin olmak için aşağıdaki yönergeleri kullanmanızı öneririz:

* Parolalarınızı her üç ayda bir değiştirin. Parolaları değiştirme her yıl uygulanır.
* Güçlü parolalar kullanın. Daha fazla bilgi için [daha güçlü parolalar oluştur'a](https://cloudblogs.microsoft.com/microsoftsecure/2014/08/25/create-stronger-passwords-and-protect-them/)gidin ve bunları koruyun.
* Her zaman farklı erişim mekanizmaları için farklı parolalar kullanın; belirttiğiniz parolaların her biri benzersiz olmalıdır.
* StorSimple cihazına erişme yetkisi olmayan kişilerle parola paylaşmayın.
* Başkalarının önünde parola hakkında konuşmayın veya parolanın biçimihakkında ipucu vermez.
* Bir hesabın veya parolanın ele geçirildiğini sanıyorsanız, olayı bilgi güvenliği departmanınıza bildirin.
* Tüm parolaları hassas ve gizli bilgiler olarak ele alın. 

## <a name="storsimple-data-protection"></a>StorBasit veri koruması

Bu bölümde, aktarım ve depolanan verilerdeki verileri koruyan StorSimple güvenlik özellikleri açıklanmaktadır.

Diğer bölümlerde açıklandığı gibi, parolalar, StorSimple çözümünüze erişebilmeleri için kullanıcıları yetkilendirmek ve doğrulamak için kullanılır. Başka bir güvenlik hususu, depolama sistemleri arasında aktarılırken ve depolanırken verileri yetkisiz kullanıcılardan korumaktır. Aşağıdaki bölümlerde StorSimple ile sağlanan veri koruma özellikleri açıklanmaktadır.

> [!NOTE]
> Deduplication, StorSimple aygıtında ve Microsoft Azure depolama alanında depolanan veriler için ek koruma sağlar. Veriler çoğaltıldığında, veri nesneleri eşlemek ve bunlara erişmek için kullanılan meta verilerden ayrı olarak depolanır: birim yapısına, dosya sistemine veya dosya adına dayalı verileri yeniden oluşturmak için kullanılabilir depolama düzeyi bağlamı yoktur.


## <a name="protect-data-flowing-through-the-service"></a>Hizmet aracılığıyla akan verileri koruma

StorSimple Aygıt Yöneticisi hizmetinin birincil amacı, StorSimple cihazını yönetmek ve yapılandırmaktır. StorSimple Device Manager hizmeti Microsoft Azure'da çalışır. Aygıt yapılandırma verilerini girmek için Azure portalını kullanırsınız ve microsoft Azure verileri aygıta göndermek için StorSimple Device Manager hizmetini kullanır. StorSimple, Azure hizmetinin tehlikeye atılmaması için bir asimetrik anahtar çifti sistemi kullanır.

![Uçuşta veri şifreleme](./media/storsimple-security/DataEncryption.png)

Asimetrik anahtar sistemi, hizmet aracılığıyla akan verilerin aşağıdaki gibi korunmasına yardımcı olur:

1. Aygıtta asimetrik ortak ve özel anahtar çifti kullanan bir veri şifreleme sertifikası oluşturulur ve verileri korumak için kullanılır. Anahtarlar ilk aygıt kaydedildiğinde oluşturulur.
2. Veri şifreleme sertifikası anahtarları, kayıt sırasında ilk aygıt tarafından rasgele oluşturulan güçlü bir 128 bit anahtar olan hizmet veri şifreleme anahtarı tarafından korunan Bir Kişisel Bilgi Alışverişi (.pfx) dosyasına dışa aktarılır.
3. Sertifikanın ortak anahtarı Güvenli bir şekilde StorSimple Device Manager hizmetine verilir ve özel anahtar aygıtta kalır.
4. Hizmete giren veriler ortak anahtar kullanılarak şifrelenir ve aygıtta depolanan özel anahtar kullanılarak şifrelenir ve Azure hizmetinin aygıta akan verilerin şifresini çözememesini sağlar.

Hizmet veri şifreleme anahtarı yalnızca hizmete kayıtlı ilk aygıtta oluşturulur. Hizmete kayıtlı sonraki tüm aygıtlar aynı hizmet veri şifreleme anahtarını kullanmalıdır.

> [!IMPORTANT]
> Hizmet veri şifreleme anahtarının bir kopyasını yapmak ve güvenli bir konuma kaydetmek çok önemlidir. Hizmet veri şifreleme anahtarının bir kopyası yetkili bir kişi tarafından erişilebilen ve aygıt yöneticisine kolayca iletilebilecek şekilde depolanmalıdır.
> 
> Hizmet veri şifreleme anahtarı kaybolursa, bir Microsoft destek elemanı çevrimiçi durumda en az bir aygıtınız olması koşuluyla bu anahtarı almanıza yardımcı olabilir. Hizmet veri şifreleme anahtarını alındıktan sonra değiştirmenizi öneririz.

Hizmet veri şifreleme anahtarını ve ilgili veri şifreleme sertifikasını değiştirmek [için StorSimple Device Manager hizmetiniz için hizmet veri şifreleme anahtarını değiştir'deki](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)adımları izleyin. Şifreleme anahtarlarını değiştirmek için tüm aygıtların yeni anahtarla güncelleştirilmesi gerekir. Bu nedenle, tüm aygıtlar çevrimiçi olduğunda anahtarı değiştirmenizi öneririz. Aygıtlar çevrimdışıysa, anahtarları farklı bir zamanda değiştirilebilir. Güncel olmayan anahtarları olan aygıtlar yedeklemeleri çalıştırmaya devam eder, ancak anahtar güncelleştirilene kadar verileri geri yükemezler.

Hizmet veri şifreleme anahtarı ve veri şifreleme sertifikasının süresi dolmaz. Ancak, anahtar gizliliğini önlemeye yardımcı olmak için hizmet veri şifreleme anahtarını her yıl değiştirmenizi öneririz.

## <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

StorSimple cihazı, kullanım sıklığına bağlı olarak verileri yerel olarak ve bulutta katmanlarda depolayarak yönetir. Aygıta bağlı tüm ana bilgisayar makineleri aygıta veri gönderir ve verileri uygun şekilde buluta taşır. Veriler cihazdan Bulut'a Internet üzerinden güvenli bir şekilde aktarılır. Her aygıtın, o cihazdaki tüm paylaşılan birimleri yüzer bir iSCSI hedefi vardır. Tüm veriler bulut depolamaya gönderilmeden önce şifrelenir. 

![Bulut depolama şifreleme anahtarı](./media/storsimple-security/CloudStorageEncryption.png)

StorSimple, buluta taşınan verilerin güvenliğini ve bütünlüğünü sağlamak için bulut depolama şifreleme anahtarlarını aşağıdaki gibi tanımlamanıza olanak tanır:

* Bir birim kapsayıcı oluştururken bulut depolama şifreleme anahtarını belirtirsiniz. Anahtar değiştirilemez veya daha sonra eklenemez.
* Birim kapsayıcıdaki tüm birimler aynı şifreleme anahtarını paylaşır. Belirli bir birim için farklı bir şifreleme biçimi istiyorsanız, bu birimi barındıracak yeni bir birim kapsayıcıoluşturmanızı öneririz.
* StorSimple Device Manager hizmetinde bulut depolama şifreleme anahtarını girdiğinizde, anahtar hizmet veri şifreleme anahtarının ortak bölümü kullanılarak şifrelenir ve aygıta gönderilir.
* Bulut depolama şifreleme anahtarı hizmetin hiçbir yerinde depolanmaz ve yalnızca aygıt tarafından bilinir.
* Bulut depolama şifreleme anahtarı belirtmek isteğe bağlıdır. Ana bilgisayarda şifrelenmiş verileri aygıta gönderebilirsiniz.

### <a name="additional-security-best-practices"></a>Ek güvenlik en iyi uygulamaları

* Trafiği böl: tamamen ayrılmış bir ağ dağıtarak ve fiziksel yalıtımın bir seçenek olmadığı VLAN'ları kullanarak iSCSI SAN'ınızı kurumsal bir LAN'daki kullanıcı trafiğinden yalıtın. iSCSI depolama için özel bir ağ, iş açısından kritik verilerinizin güvenliğini ve performansını garanti eder. Depolama ve kullanıcı trafiğinin kurumsal bir LAN üzerinden karıştırılması önerilmez ve gecikmeyi artırabilir ve ağ hatalarına neden olabilir.
* Ana bilgisayar ağ güvenliği için, TCP/IP Boşaltma Motoru'nun (TOE) destekleyen ağ arabirimlerini kullanın. TOE, ağ bağdaştırıcısı üzerinde TCP işleyerek CPU yükünü azaltır.

## <a name="protect-data-via-storage-accounts"></a>Depolama hesapları aracılığıyla verileri koruma

Her Microsoft Azure aboneliği bir veya daha fazla depolama hesabı oluşturabilir. (Depolama hesabı, Azure bulutunda depolanan verilerle çalışmak için benzersiz bir ad alanı sağlar.) Bir depolama hesabına erişim, abonelik ve bu depolama hesabıyla ilişkili erişim anahtarları tarafından denetlenir.

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure, StorSimple aygıtı depolama hesabına eriştığında kimlik doğrulama için kullanılan iki adet 512 bit depolama erişim anahtarı oluşturur. Bu anahtarlardan yalnızca birinin kullanıldığını unutmayın. Diğer anahtar, anahtarları periyodik olarak döndürmenize olanak tanıyan yedekte tutulur. Tuşları döndürmek için ikincil anahtarı etkin hale getirin ve ardından birincil anahtarı silersiniz. Daha sonra bir sonraki döndürme sırasında kullanmak üzere yeni bir anahtar oluşturabilirsiniz. (Güvenlik nedenleriyle, birçok veri merkezi anahtar döndürme gerektirir.)

Anahtar döndürme için aşağıdaki en iyi uygulamaları izlemenizi öneririz:

* Depolama hesabınıza yetkisiz kullanıcılar tarafından erişilemediğinden emin olmak için depolama hesabı anahtarlarını düzenli olarak döndürmeniz gerekir.
* Azure yöneticiniz, depolama hesabına doğrudan erişmek için Azure portalının Depolama bölümünü kullanarak birincil veya ikincil anahtarı düzenli olarak değiştirmeli veya yeniden oluşturmalıdır.

## <a name="protect-data-via-encryption"></a>Şifreleme yoluyla verileri koruma

StorSimple, StorSimple çözümünüzün bileşenleri arasında depolanan veya bunlar arasında seyahat eden verileri korumak için aşağıdaki şifreleme algoritmalarını kullanır.

| Algoritma | Anahtar uzunluğu | Protokoller/uygulamalar/yorumlar |
| --- | --- | --- |
| RSA |2048 |RSA PKCS 1 v1.5, aygıta gönderilen yapılandırma verilerini şifrelemek için Azure portalı tarafından kullanılır: örneğin, depolama hesabı kimlik bilgileri, StorSimple aygıt yapılandırması ve bulut depolama şifreleme anahtarları. |
| AES |256 |CBC'li AES, StorSimple aygıtından Azure portalına gönderilmeden önce hizmet veri şifreleme anahtarının ortak bölümünü şifrelemek için kullanılır. Ayrıca, veriler bulut depolama hesabına gönderilmeden önce verileri şifrelemek için StorSimple aygıtı tarafından da kullanılır. |

## <a name="storsimple-cloud-appliance-security"></a>StorSimple Cloud Appliance güvenliği

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="managing-personal-information"></a>Kişisel bilgilerin yönetimi

Hem fiziksel hem de sanal seriler için StorSimple Aygıt Yöneticisi aşağıdaki önemli durumlarda kişisel bilgileri toplar:

- Kullanıcıların e-posta adresinin yapılandırıldığı kullanıcı ayarlarını uyar. Bu bilgiler yönetici tarafından görüntülenebilir ve temizlenebilir. Bu, hem StorSimple 8000 serisi aygıtlar hem de StorSimple Sanal Diziler için geçerlidir.
  * StorSimple 8000 serisinin ayarlarını görüntülemek ve temizlemek için Görünüm'deki adımları izleyin [ve StorSimple uyarılarını yönetin](storsimple-8000-manage-alerts.md#configure-alert-settings)
  * StorSimple Virtual Array ayarlarını görüntülemek ve temizlemek için Görünüm'deki adımları izleyin [ve StorSimple uyarılarını yönetin](storsimple-virtual-array-manage-alerts.md#configure-alert-settings)
- Paylaşımlarda bulunan verilere erişebilen kullanıcılar. Paylaşım verilerine erişebilen kullanıcıların listesi görüntülenir ve görüntülenebilir. Paylaşımlar silindiğinde bu liste de silinir. Bu yalnızca StorSimple Sanal Diziler için geçerlidir.
  * Bir paylaşıma erişebilen veya silebilen kullanıcı listesini görüntülemek için [StorSimple Virtual Array'deki paylaşımları yönet](storsimple-virtual-array-manage-shares.md) adımlarını izleyin

Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Aşağıda güvenlik ve Microsoft Azure StorSimple ile ilgili bazı sorular ve yanıtlar verilmiştir.

**S:** Hizmetim tehlikede. Sonraki adımlarım ne olmalı?

**A:** Verileri katmanlama için kullanılan depolama hesabının hizmet veri şifreleme anahtarını ve depolama hesabı anahtarlarını hemen değiştirmeniz gerekir. Talimatlar için şu telefona gidin:

* [Hizmet veri şifreleme anahtarını değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [Depolama hesaplarının anahtar döndürmesi](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**S:** Ben hizmet kayıt anahtarı için soran yeni bir StorSimple cihaz var. Nasıl geri alabiliyorum?

**A:** Bu anahtar, StorSimple Device Manager hizmetini ilk oluşturduğunuzda oluşturulmuştur. Cihaza bağlanmak için StorSimple Device Manager hizmetini kullandığınızda, servis kayıt anahtarını görüntülemek veya yeniden oluşturmak için servis hızlı başlangıç sayfasını kullanabilirsiniz. Yeni bir hizmet kayıt anahtarı oluşturmak varolan kayıtlı aygıtları etkilemez. Talimatlar için şu telefona gidin:

* [Hizmet kayıt anahtarını görüntüleme veya yenileme](storsimple-8000-manage-service.md#regenerate-the-service-registration-key)

**S:** Hizmet veri şifreleme anahtarımı kaybettim. Ne yapmalıyım?

**A:** Microsoft Destek'e başvurun. Cihazınızdaki bir destek oturumunda oturum açabilir ve anahtarı almanıza yardımcı olabilir (en az bir cihaz çevrimiçi olması koşuluyla). Hizmet veri şifreleme anahtarını aldıktan hemen sonra, yeni anahtarın yalnızca sizin için bilindiğinden emin olmak için bu anahtarı değiştirmeniz gerekir. Talimatlar için şu telefona gidin:

* [Hizmet veri şifreleme anahtarını değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)

**S:**  Bir hizmet veri şifreleme anahtarı değişikliği için bir cihazyetkilendim, ancak anahtar değiştirme işlemini başlatmadım. Ne yapmalıyım?

**A:** Zaman aşımı süresi dolduysa, servis veri şifreleme anahtarı değişikliği için aygıtı yeniden yetkilendirmeniz ve işlemi yeniden başlatmanız gerekir.

**S:**  Hizmet veri şifreleme anahtarını değiştirdim, ancak diğer aygıtları 4 saat içinde güncelleştiremiyorum. Yeniden başlamak zorunda mıyım?

**A:** 4 saatlik zaman dilimi yalnızca değişikliği başlatmak içindir. Yetkili StorSimple aygıtında güncelleştirme işlemini başlattıktan sonra, tüm aygıtlar güncelleştirilene kadar yetkilendirme geçerlidir.

**S:** StorSimple yöneticimiz şirketten ayrıldı. Ne yapmalıyım?

**A:** StorSimple aygıtına erişime izin veren parolaları değiştirin ve sıfırlayın ve yeni bilgilerin yetkisiz personel tarafından bilinmediğinden emin olmak için hizmet veri şifreleme anahtarını değiştirin. Talimatlar için şu telefona gidin:

* [StorSimple Device Manager hizmetini kullanarak storsimple parolalarınızı değiştirin](storsimple-8000-change-passwords.md)
* [Hizmet veri şifreleme anahtarını değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [StorSimple cihazınız için CHAP'yi yapılandırın](storsimple-8000-configure-chap.md)

**S:** StorSimple aygıtına bağlanan bir ana bilgisayara StorSimple Snapshot Manager parolasını sağlamak istiyorum, ancak parola kullanılamıyor. Ne yapabilirim?

**A:** Parolayı unuttuysanız, yeni bir parola oluşturmanız gerekir. Ardından, varolan tüm kullanıcılara parolanın değiştirildiğini ve yeni parolayı kullanmak için istemcilerini güncelleştirmeleri gerektiğini bildirdiğinden emin olun. Talimatlar için şu telefona gidin:

* [StorSimple Snapshot Manager parolasını değiştirme](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Aygıtın kimliğini doğrulatın](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**S:** StorSimple için Windows PowerShell'e uzaktan erişim sertifikası aygıtta değiştirildi. Uzaktan erişim istemcilerimi nasıl güncellerim?

**A:** Yeni sertifikayı StorSimple Device Manager hizmetinden indirebilir ve uzaktan erişim istemcilerinizin sertifika deposuna yüklenmesini sağlayabilirsiniz. Talimatlar için şu telefona gidin:

* [İthalat-Sertifika cmdlet](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate)

**S:** StorSimple Device Manager hizmeti tehlikeye girerse verilerim korunur mu?

**A:** Hizmet yapılandırma verileri, bir web tarayıcısında görüntülediğinizde her zaman ortak anahtarınızla şifrelenir. Hizmetin özel anahtara erişimi olmadığından, hizmet herhangi bir veri göremez. StorSimple Device Manager hizmeti tehlikeye girerse, StorSimple Device Manager hizmetinde depolanan anahtar lar olmadığından hiçbir etkisi yoktur.

**S:** Birisi veri şifreleme sertifikasına erişirse, verilerim tehlikeye girer mi?

**A:** Microsoft Azure, müşterinin veri şifreleme anahtarını (.pfx dosyası) şifreli bir biçimde saklar. .pfx dosyası şifrelenmiş olduğundan ve StorSimple hizmeti .pfx dosyasının şifresini çözmek için hizmet veri şifreleme anahtarına sahip olmadığından, sadece .pfx dosyasına erişim elde etmek herhangi bir sırrı açığa çıkarmaz.

**S:** Bir devlet kurumu Microsoft'a verilerimi sorarsa ne olur?

**A:** Tüm veriler hizmette şifrelendirildiğinden ve özel anahtar cihazla tutulduğundan, kamu tüzel kişistürünün müşteriden verileri istemesi gerekir.



## <a name="next-steps"></a>Sonraki adımlar

[StorSimple cihazınızı dağıtın.](storsimple-8000-deployment-walkthrough-u2.md)

