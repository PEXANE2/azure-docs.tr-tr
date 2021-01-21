---
title: Şirket içi yönetim konsolunuzu etkinleştirme ve ayarlama
description: Yönetim Konsolu etkinleştirme ve kurulumu, algılayıcıların Azure 'a kaydedilmesini ve şirket içi yönetim konsoluna bilgi göndermesini ve şirket içi yönetim konsolunun bağlı sensörlerde yönetim görevleri kapsamasını sağlar.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/12/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 94a1db30419e5d7e52f369392d94b817d0dc273a
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98623763"
---
# <a name="activate-and-set-up-your-on-premises-management-console"></a>Şirket içi yönetim konsolunuzu etkinleştirme ve ayarlama 

Şirket içi yönetim konsolunun etkinleştirilmesi ve kurulumu şunları sağlar:

- Bağlı sensörlerle izlemekte olduğunuz ağ cihazları bir Azure hesabıyla kaydedilir.

- Sensörler şirket içi yönetim konsoluna bilgi gönderir.

- Şirket içi yönetim konsolu, bağlı sensörler üzerinde yönetim görevlerini yürütür.

- Bir SSL sertifikası yüklediniz.

## <a name="sign-in-for-the-first-time"></a>İlk kez oturum açma

Yönetim konsolunda oturum açmak için:

- Bir Web tarayıcısı açın ve sistem yüklemesi sırasında şirket içi yönetim konsolu için aldığınız IP adresini ve parolayı girin. Parolanızı unuttuysanız **parolayı kurtar** ' ı seçin ve [parola kurtarma](how-to-manage-the-on-premises-management-console.md#password-recovery)bölümüne bakın.

## <a name="upload-an-activation-file"></a>Bir etkinleştirme dosyasını karşıya yükle

İlk kez oturum açtıktan sonra, IoT portalının Azure Defender 'ın **fiyatlandırma** sayfasından bir etkinleştirme dosyası indirerek şirket içi yönetim konsolunu etkinleştirin. Bu dosya ekleme işlemi sırasında tanımlanan toplam kaydedilmiş cihazları içerir. **Kaydedilen cihazlar** , her abonelik Için Defender 'ın IoT tarafından izleneceği cihaz sayısını belirtir.

Bir etkinleştirme dosyasını karşıya yüklemek için:

1. IoT için Defender **fiyatlandırma** sayfasına gidin.
1. **Yönetim Konsolu için etkinleştirme dosyasını indirin** sekmesini seçin. Etkinleştirme dosyası indirilir.

   :::image type="content" source="media/how-to-manage-sensors-from-the-on-premises-management-console/cloud_download_opm_activation_file.png" alt-text="Etkinleştirme dosyasını indirin.":::

1. Yönetim konsolundan **sistem ayarları** ' nı seçin.
1. **Etkinleştirme**'yi seçin.
1. **Dosya Seç** ' i seçin ve kaydettiğiniz dosyayı seçin.

İlk etkinleştirmeden sonra, izlenen cihazların sayısı ekleme sırasında tanımlanan kaydedilmiş cihazların sayısını aşabilir. Bu, örneğin, yönetim konsoluna daha fazla algılayıcı bağladığınızda meydana gelebilir. İzlenen cihazların sayısıyla kaydedilmiş cihazların sayısı arasında bir tutarsızlık varsa, yönetim konsolunda bir uyarı görüntülenir. Bu durumda yeni bir etkinleştirme dosyası yüklemeniz gerekir.

## <a name="set-up-a-certificate"></a>Sertifika ayarlama

Yönetim konsolunun yüklenmesinden sonra, yerel olarak imzalanan bir sertifika oluşturulur ve konsola erişim için kullanılır. Yönetici Yönetim konsolunda ilk kez oturum açtıktan sonra, bu kullanıcıdan bir SSL/TLS sertifikası girmesi istenir. 

İki güvenlik düzeyi mevcuttur:

- CA imzalı sertifikayı karşıya yükleyerek kuruluşunuz tarafından istenen belirli sertifika ve şifreleme gereksinimlerini karşılayın.
- Yönetim Konsolu ve bağlı algılayıcılar arasında doğrulamaya izin verin. Doğrulama, bir sertifika iptal listesi ve sertifika sona erme tarihine göre değerlendirilir. *Doğrulama başarısız olursa, Yönetim Konsolu ile algılayıcı arasındaki iletişim durdurulur ve konsolda bir doğrulama hatası sunulur.* Bu seçenek, yüklemeden sonra varsayılan olarak etkinleştirilmiştir.  

Konsolu aşağıdaki sertifika türlerini destekler:

- Özel ve Kurumsal anahtar altyapısı (özel PKI)

- Ortak anahtar altyapısı (genel PKI)

- Gereç üzerinde yerel olarak oluşturuldu (yerel olarak kendinden imzalı) 

  > [!IMPORTANT]
  > Kendinden imzalı bir sertifika kullanmanızı öneririz. Sertifika güvenli değil ve yalnızca test ortamları için kullanılmalıdır. Sertifikanın sahibi doğrulanamaz ve sisteminizin güvenliği korunabilir olamaz. Bu seçeneği, üretim ağları için hiçbir şekilde kullanmayın.

Bir sertifikayı karşıya yüklemek için:

1. Oturum açtıktan sonra istendiğinde, bir sertifika adı tanımlayın.
1. CRT ve anahtar dosyalarını karşıya yükleyin.
1. Bir parola girin ve gerekirse bir ped dosyası yükleyin.

CA imzalı sertifikayı karşıya yükledikten sonra ekranınızı yenilemeniz gerekebilir.

Yönetim Konsolu ve bağlı sensörler arasında doğrulamayı devre dışı bırakmak için:

1. **İleri**’yi seçin.
1. **Sistem genelindeki doğrulama** geçiş geçişi ' ni kapatın.

Yeni sertifikayı, desteklenen sertifika dosyalarını ve ilgili öğeleri karşıya yükleme hakkında bilgi için bkz. [Şirket içi yönetim konsolunu yönetme](how-to-manage-the-on-premises-management-console.md).

## <a name="connect-sensors-to-the-on-premises-management-console"></a>Algılayıcıları şirket içi yönetim konsoluna bağlama

Algılayıcıların şirket içi yönetim konsoluna bilgi göndermesini ve şirket içi yönetim konsolunun yedeklemeler gerçekleştirmesini, uyarıları yönetmesini ve sensörlerdeki diğer etkinlikleri gerçekleştirmesini sağlamalısınız. Bunu yapmak için, algılayıcılar ve şirket içi yönetim konsolu arasında ilk bağlantı yapıldığını doğrulamak için aşağıdaki yordamları kullanın.

IoT algılayıcılarının Azure Defender 'ı şirket içi yönetim konsoluna bağlamak için iki seçenek mevcuttur:

- Algılayıcı konsolundan Bağlan

- Tünel kullanarak bağlanma

Bağlandıktan sonra, bu sensörlerle bir site ayarlamanız gerekir.

### <a name="connect-sensors-from-the-sensor-console"></a>Algılayıcı konsolundan algılayıcıların bağlantısını yapın

Belirli algılayıcılar 'ı, algılayıcı konsolundan Şirket içi yönetim konsoluna bağlamak için:

1. Algılayıcı konsolunun sol bölmesinde **sistem ayarları**' nı seçin.

2. **Yönetimine bağlantıyı** seçin.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/connection-status-window-not-connected.png" alt-text="Şirket içi yönetim konsolunun durum penceresinin, bağlı olmayan öğesini gösteren ekran görüntüsü.":::

3. **Adres** metin kutusuna, bağlanmak istediğiniz şirket içi YÖNETIM konsolunun IP adresini girin.

4. **Bağlan**’ı seçin. Durum değiştiğinde:

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/connection-status-window-connected.png" alt-text="Şirket içi yönetim konsolunun durum penceresinin, bağlı olduğunu gösteren ekran görüntüsü.":::

### <a name="connect-sensors-by-using-tunneling"></a>Tüneli kullanarak sensörlerden bağlanma

Kurumsal algılayıcılar ve şirket içi yönetim konsolu arasında güvenli bir tünel oluşturma bağlantısı etkinleştirin. Bu kurulum, kurumsal güvenlik duvarıyla etkileşimi atlayor ve sonuç olarak saldırı yüzeyini azaltır.

Tünel kullanımı, şirket içi yönetim konsoluna IP adresinden ve tek bir bağlantı noktasından (yani, 9000) herhangi bir sensöre bağlanmanızı sağlar.

Şirket içi yönetim konsolunda tünel ayarlamak için:

- Şirket içi yönetim konsolunda oturum açın ve aşağıdaki komutları çalıştırın:

  ```bash
  cyberx-management-tunnel-enable
  service apache2 reload
  sudo cyberx-management-tunnel-add-xsense --xsenseuid <sensorIPAddress> --xsenseport 9000
  service apache2 reload
  ```

Sensörde tünel ayarlamak için:

1. Sensörde (ağ. Özellikler) TCP bağlantı noktası 9000 ' i el ile açın. Bağlantı noktası açık değilse, algılayıcı şirket içi yönetim konsolundan bağlantıyı reddeder.

2. Her sensörde oturum açın ve aşağıdaki komutları çalıştırın:

   ```bash
   sudo cyberx-xsense-management-connect -ip <centralmanagerIPAddress>
   sudo cyberx-xsense-management-tunnel
   sudo vi /var/cyberx/properties/network.properties
   opened_tcp_incoming_ports=22,80,443,102,9000
   sudo cyberx-xsense-network-validation
   sudo /etc/network/if-up.d/iptables-recover
   sudo iptables -nvL
   ```

## <a name="set-up-a-site"></a>Site ayarlama

Varsayılan kurumsal harita, çeşitli coğrafi konumlar düzeylerine göre cihazlarınızın genel bir görünümünü sağlar.

Cihazların görünümü, kuruluş yapısının ve Kullanıcı izinlerinin karmaşık olduğu durumlarda gerekli olabilir. Bu durumlarda, site kurulumu standart site veya bölge yapısına ek olarak genel bir kuruluş yapısıyla belirlenebilir.

Bu ortamı desteklemek için kuruluşunuzun iş birimlerini, bölgelerini, sitelerini ve bölgelerini temel alan küresel bir iş topolojisi oluşturmanız gerekir. Ayrıca, erişim gruplarını kullanarak bu varlıklar etrafında Kullanıcı erişim izinleri tanımlamanız gerekir.

Erişim grupları, kullanıcıların, IoT Platformu için Defender 'da cihazları yönetmesi ve analiz etmek için daha iyi denetim sağlar.

### <a name="how-it-works"></a>Nasıl çalışır?

Her site için bir iş birimi ve bölge tanımlayabilirsiniz. Daha sonra, ağınızdaki mantıksal varlıklar olan bölgeleri ekleyebilirsiniz. 

Her bölge için en az bir algılayıcı atamanız gerekir. Beş düzeyli model, kuruluşunuzun yapısını yansıtan koruma sistemini sağlamak için gereken esnekliği ve ayrıntı düzeyini sağlar.

Sitelerinizi doğrudan harita görünümlerinden herhangi birinden düzenleyebilirsiniz. Bir siteyi harita görünümünden açtığınızda, her bir bölgenin yanında açık uyarı sayısı görüntülenir.

:::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/console-map-with-data-overlay-v2.png" alt-text="Berlin veri kaplaması ile şirket içi yönetim konsolu eşlemesinin ekran görüntüsü.":::

:::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/diagram-of-sensor-showing-relationships.png" alt-text="Algılayıcılar ve bölgesel ilişkiyi gösteren diyagram.":::

Bir site ayarlamak için:

1. Kuruluşunuzun mantıksal yapısını yansıtmak için yeni iş birimleri ekleyin.

2. Kuruluşunuzun bölgelerini yansıtmak için yeni bölgeler ekleyin.

3. Site ekleyin.

4. Bölgeye bölge ekleyin.

5. Algılayıcıları bağlayın.

6. Site bölgelerine algılayıcı atayın.

İş birimleri eklemek için:

1. Kurumsal görünümden **tüm siteler**  >  **iş birimlerini Yönet**' i seçin.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/manage-business-unit-screen.png" alt-text="Iş birimlerini Yönet görünümünü gösteren ekran görüntüsü.":::

2. Yeni iş birimi adını girin ve **Ekle**' yi seçin.

Yeni bir bölge eklemek için:

1. Kurumsal görünümden **tüm bölgeler**  >  **bölgeleri Yönet**' i seçin.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/manage-regions-screen.png" alt-text="Bölgeleri Yönet görünümünü gösteren ekran görüntüsü.":::

2. Yeni bölge adını girin ve **Ekle**' yi seçin.

Yeni bir site eklemek için:

1. Kuruluş görünümünden üstteki çubukta öğesini seçin :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/new-site-icon.png" border="false"::: . İmlecinizin artı işareti () olarak gösterilir **+** .

2. **+** Yeni sitenin konumuna konumlandırın ve seçin. **Yeni site oluştur** iletişim kutusu açılır.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/create-new-site-screen.png" alt-text="Yeni site oluştur görünümünün ekran görüntüsü.":::

3. Yeni site için adı ve fiziksel adresi tanımlayın ve **Kaydet**' i seçin. Yeni site site haritasında görüntülenir.

Bir siteyi silmek için:

1. **Site yönetimi** penceresinde, :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/expand-view-icon.png" border="false"::: site adını içeren çubuktan seçim yapın ve ardından **siteyi Sil**' i seçin. Siteyi silmek istediğinizi doğrulayarak onay kutusu görüntülenir.

2. Onay kutusunda **Evet**' i seçin. Onay kutusu kapanır ve **site yönetimi** penceresi sildiğiniz site olmadan görüntülenir.

## <a name="create-enterprise-zones"></a>Kurumsal bölgeler oluşturma

Bölgeler, bir sitedeki cihazları çeşitli özelliklere göre gruplara bölmenizi sağlayan mantıksal varlıklardır. Örneğin, üretim hatları, alt istasyonlar, site alanları veya cihaz türleri için gruplar oluşturabilirsiniz. Alanları, kuruluşunuz için uygun olan herhangi bir özelliğe göre tanımlayabilirsiniz.

Bölgeleri, site yapılandırma sürecinin bir parçası olarak yapılandırırsınız.

:::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/site-management-zones-screen-v2.png" alt-text="Site yönetimi bölgeleri görünümünün ekran görüntüsü.":::

Aşağıdaki tabloda, **site yönetim** penceresindeki parametreler açıklanmaktadır.

| Parametre | Açıklama |
|--|--|
| Ad | Sensör adı. Bu adı yalnızca sensörden değiştirebilirsiniz. Daha fazla bilgi için bkz. IoT için Defender Kullanıcı Kılavuzu. |
| IP | Algılayıcı IP adresi. |
| Sürüm | Algılayıcı sürümü. |
| Bağlantı | Algılayıcı bağlantı durumu. Durum **bağlı** veya **bağlantısı kesilmiş** olabilir. |
| Son yükseltme | Son yükseltmenin tarihi. |
| Yükseltme Ilerleme durumu | İlerleme çubuğu, yükseltme işleminin durumunu aşağıdaki gibi gösterir:<br />-Paket karşıya yükleniyor<br />-Yüklenmeye hazırlanıyor<br />-İşlem durduruluyor<br />-Verileri yedekleme<br />-Anlık görüntü alınıyor<br />-Yapılandırma güncelleştiriliyor<br />-Bağımlılıklar güncelleştiriliyor<br />-Kitaplıklar güncelleştiriliyor<br />-Düzeltme eki uygulama<br />-Süreçler başlatılıyor<br />-Sistem sağlamlık doğrulanıyor<br />-Doğrulama başarılı<br />-Başarılı<br />-Hata<br />-Yükseltme başlatıldı<br />-Yükleme başlatılıyorogress bar shows the status of the upgrade process, as follows:<br />- Uploading package<br />- Preparing to install<br />- Stopping processes<br />- Backing up data<br />- Taking snapshot<br />- Updating configuration<br />- Updating dependencies<br />- Updating libraries<br />- Patching databases<br />- Starting processes<br />- Validating system sanity<br />- Validation succeeded<br />- Success<br />- Failure<br />- Upgrade started<br />- Starting installation<br /></br >Yükseltme hakkında daha fazla bilgi için, yardım için [Microsoft desteği](https://support.microsoft.com/) başvurun. |
| Cihazlar | Algılayıcıyı izleyen OT cihaz sayısı. |
| Uyarılar | Sensördeki uyarı sayısı. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/assign-icon.png" border="false"::: | Bölgelere bir algılayıcı atanmasını sağlar. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/delete-icon.png" border="false":::| Siteden bağlantısı kesilen bir algılayıcıyı silmeye izin vermez. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/sensor-icon.png" border="false"::: | Şu anda bölgeye bağlı olan sensörler sayısını gösterir. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/ot-assets-icon.png" border="false"::: | Şu anda bölgeye kaç OT varlıklarının bağlı olduğunu gösterir. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/number-of-alerts-icon.png" border="false"::: | Bölgeye atanan sensörler tarafından gönderilen uyarı sayısını gösterir. |
| :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/unassign-sensor-icon.png" border="false"::: | Bölgelerden algılayıcıların atamasını kaldırır. |

Bir siteye bölge eklemek için:

1. **Site yönetimi** penceresinde, :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/expand-view-icon.png" border="false"::: site adını içeren çubuktan seçim yapın ve ardından **bölge Ekle**' yi seçin. **Yeni bölge oluştur** iletişim kutusu görüntülenir.

    :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/create-new-zone-screen.png" alt-text="Yeni bölge oluştur görünümünün ekran görüntüsü.":::

2. Bölge adını girin.

3. Siteyi bölgelere bölmek için kullandığınız özellikleri açıkça belirten yeni bölge için bir açıklama girin.

4. **Kaydet**' i seçin. Yeni bölge, bu bölgenin ait olduğu sitenin altındaki **site yönetimi** penceresinde görünür.

Bir bölgeyi düzenlemek için:

1. **Site yönetimi** penceresinde, :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/expand-view-icon.png" border="false"::: bölge adını içeren çubuktan seçim yapın ve ardından **bölgeyi Düzenle**' yi seçin. **Bölgeyi Düzenle** iletişim kutusu görüntülenir.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/zone-edit-screen.png" alt-text="Bölgeyi Düzenle iletişim kutusunu gösteren ekran görüntüsü.":::

2. Bölge parametrelerini düzenleyin ve **Kaydet**' i seçin.

Bir bölgeyi silmek için:

1. **Site yönetimi** penceresinde, :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/expand-view-icon.png" border="false"::: bölge adını içeren çubuktan seçim yapın ve ardından **bölgeyi Sil**' i seçin.

2. Onay kutusunda **Evet**' i seçin.

Bağlantı durumuna göre filtrelemek için:

- Sol üst köşedeki :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/down-pointing-icon.png" border="false"::: **bağlantı**' nın yanındaki ' ı seçin ve ardından aşağıdaki seçeneklerden birini belirleyin:

  - **Tümü**: Bu şirket içi yönetim konsoluna rapor veren tüm sensöri gösterir.

  - **Bağlandı**: yalnızca bağlı algılayıcılar sunar.

  - **Bağlantısı kesik**: yalnızca bağlantısı kesilen algılayıcılar sunar.

Yükseltme durumuna göre filtrelemek için:

- Sağ üst köşedeki yükseltme durumu ' nu seçin :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/down-pointing-icon.png" border="false"::: ve aşağıdaki  seçeneklerden birini belirleyin:

  - **Tümü**: Bu şirket içi yönetim konsoluna rapor veren tüm sensöri gösterir.

  - **Geçerli**: geçerli bir yükseltme durumuyla algılayıcılar sunar.

  - **Devam** ediyor: yükseltme sürecinde olan algılayıcılar sunar.

  - **Başarısız**: yükseltme işlemi başarısız olan algılayıcılar sunar.

## <a name="assign-sensors-to-zones"></a>Dilimlerden algılayıcılar atama

Her bölge için, yerel trafik analizi ve uyarı gerçekleştiren algılayıcılar atamanız gerekir. Yalnızca şirket içi yönetim konsoluna bağlı sensörlerden atayabilirsiniz.

Bir algılayıcı atamak için:

1. **Site yönetimi**' ni seçin. Atanmamış algılayıcılar iletişim kutusunun sol üst köşesinde görüntülenir.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/unassigned-sensors-view.png" alt-text="Atanmamış algılayıcılar görünümünün ekran görüntüsü.":::

2. **Bağlantı** durumunun bağlı olduğunu doğrulayın. Aksi takdirde, bağlanma hakkındaki ayrıntılar için bkz. Şirket [içi yönetim konsoluna sensörlerden bağlanma](#connect-sensors-to-the-on-premises-management-console) . 

3. :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/assign-icon.png" border="false":::Atamak istediğiniz algılayıcı için seçin.

4. **Algılayıcı ata** iletişim kutusunda atanacak iş birimini, bölgeyi, siteyi ve bölgeyi seçin.

   :::image type="content" source="media/how-to-activate-and-set-up-your-on-premises-management-console/assign-sensor-screen.png" alt-text="Algılayıcı görünümü atama ekranının ekran görüntüsü.":::

5. **Ata**' yı seçin.

Bir sensör atamasını kaldırmak ve silmek için:

1. Algılayıcısı şirket içi yönetim konsolundan sökün. Ayrıntılar için bkz. Şirket [içi yönetim konsoluna algılayıcılar bağlama](#connect-sensors-to-the-on-premises-management-console) .

2. **Site yönetimi** penceresinde, algılayıcı ' ı seçin ve öğesini seçin :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/unassign-sensor-icon.png" border="false"::: . Algılayıcı, birkaç dakika sonra atanmamış algılayıcılar listesinde görünür.

3. Atanmamış algılayıcıyı siteden silmek için, atanmamış algılayıcılar listesinden algılayıcı ' ı seçin ve öğesini seçin :::image type="icon" source="media/how-to-activate-and-set-up-your-on-premises-management-console/delete-icon.png" border="false"::: .

## <a name="see-also"></a>Ayrıca bkz.

[Algılayıcı ve şirket içi yönetim konsolunda sorun giderme](how-to-troubleshoot-the-sensor-and-on-premises-management-console.md)
