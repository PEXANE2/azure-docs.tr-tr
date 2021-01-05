---
title: Algılayıcı ve şirket içi yönetim konsolunda sorun giderme
description: Sahip olabileceğiniz sorunları ortadan kaldırmak için sensörizin ve şirket içi yönetim konsolunuzun sorunlarını giderin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/12/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: a57db4f88de4a3b32b4fb315fb331500f955d501
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97843002"
---
# <a name="troubleshoot-the-sensor-and-on-premises-management-console"></a>Algılayıcı ve şirket içi yönetim konsolunda sorun giderme

Bu makalede, algılayıcı ve şirket içi yönetim konsolu için temel sorun giderme araçları açıklanmaktadır. Burada açıklanan öğelere ek olarak, aşağıdaki yollarla sisteminizin sistem durumunu kontrol edebilirsiniz:

**Uyarılar**: trafiği izleyen algılayıcı arabirimi kapalıysa bir uyarı oluşturulur. 

**SNMP**: ALGıLAYıCı durumu SNMP aracılığıyla izlenir. IoT için Azure Defender, yetkilendirilmiş bir izleme sunucusundan gönderilen SNMP sorgularına yanıt verir. 

**Sistem bildirimleri**: bir yönetim konsolu algılayıcıyı denetliyorsa, başarısız algılayıcı yedeklemeleri ve bağlantısı kesilen sensörlerle ilgili uyarıları iletebilirsiniz.

## <a name="sensor-troubleshooting-tools"></a>Algılayıcı sorun giderme araçları

### <a name="investigate-password-failure-at-initial-sign-in"></a>İlk oturum açma sırasında parola hatasını araştırın

Önceden yapılandırılmış bir ok algılayıcısı üzerinde ilk kez oturum açtığınızda, aşağıdaki parola kurtarma işlemini gerçekleştirmeniz gerekir:

1. IoT için Defender oturum açma ekranında **parola kurtarma** seçeneğini belirleyin. 

   **Parola kurtarma** ekranı açılır. Burada, Kullanıcı ve aboneliği seçmeniz istenir ve benzersiz bir tanımlayıcı verilirler.

1. IoT **siteleri ve sensör** için Defender sayfasına gidin ve **parolamı kurtar** sekmesini seçin.

1. **Parola kurtarma** ekranında aldığınız benzersiz tanımlayıcıyı girip **kurtar**' ı seçin. `password_recovery.zip`Dosya indirilir.

   > [!NOTE]
   > Etkinleştirme dosyasını değiştirmeyin. Bu, imzalanmış bir dosyadır ve bununla karşılaşırsanız çalışmaz.

1. **Parola kurtarma** ekranında dosyayı karşıya yükleyin `password_recovery.zip` ve **İleri ' yi** seçin.

Daha sonra yönetim konsolunuz için sistem tarafından oluşturulan parolayı alırsınız. 

### <a name="investigate-a-lack-of-traffic"></a>Trafik eksikliğinden araştırma

Algılayıcı, yapılandırılmış bağlantı noktalarından birinde hiçbir trafik olmadığını tanıdığında konsolun üst kısmında görünür. Bu gösterge tüm kullanıcılar tarafından görülebilir.

:::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/no-traffic-detected.png" alt-text="Hiçbir trafik algılanmadığını belirten uyarı ekran görüntüsü.":::
 
Bu ileti göründüğünde, hiçbir trafik olmadığını inceleyebilirsiniz. Yayılma kablosunun bağlı olduğundan ve yayılmış mimaride hiçbir değişiklik olmadığından emin olun.  

Destek ve sorun giderme bilgileri için [Microsoft desteği](https://support.serviceshub.microsoft.com/supportforbusiness/create?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099)başvurun.

### <a name="check-system-performance"></a>Sistem performansını denetle 

Yeni bir algılayıcı dağıtıldığında veya örneğin, algılayıcı yavaş çalışıyor veya herhangi bir uyarıyı göstermiyorsa, sistem performansını kontrol edebilirsiniz.

Sistem performansını denetlemek için:

1. Panoda, olduğundan emin olun `PPS > 0` .

   :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/dashboard-view-v2.png" alt-text="Örnek panonun ekran görüntüsü."::: 

2. Yan menüden **cihazlar**' ı seçin.

3. **Cihazlar** penceresinde cihazların bulunduğundan emin olun.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/discovered-devices.png" alt-text="Cihazların bulunduğundan emin olun.":::

4. Yan menüden **veri araştırma**' yı seçin.

5. **Veri araştırma** penceresinde, **Tümü** ' nü seçin ve bir rapor oluşturun.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/new-report-generated.png" alt-text="Veri madenciliği kullanarak yeni bir rapor oluşturun.":::

6. Raporun veri içerdiğinden emin olun.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/new-report-generated.png" alt-text="Raporun veri içerdiğinden emin olun.":::

7. Yan menüden **eğilimler & istatistik**' i seçin.

8. **Eğilimler & istatistikleri** penceresinde **pencere öğesi Ekle**' yi seçin.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/add-widget.png" alt-text="Seçerek bir pencere öğesi ekleyin.":::

9. Pencere öğesi ekleyin ve verileri gösterdiğinizden emin olun.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/widget-data.png" alt-text="Pencere öğesinin verileri göstermesini sağlayın.":::

10. Yan menüden **Uyarılar**' ı seçin. **Uyarılar** penceresi görüntülenir.

11. Uyarıların oluşturulduğundan emin olun.

    :::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/alerts-created.png" alt-text="Uyarıların oluşturulduğundan emin olun.":::


### <a name="investigate-a-lack-of-expected-alerts"></a>Beklenen uyarıların eksik olduğunu araştırın

**Uyarılar** penceresinde beklediğiniz bir uyarı gösterilmiyorsa, aşağıdakileri doğrulayın:

- Aynı uyarının, farklı bir güvenlik örneğine yeniden eylem olarak **Uyarılar** penceresinde zaten görünüp göründüğünü denetleyin. Yanıt Evet ise ve bu uyarı henüz işlenmediyse, algılayıcı konsolu yeni bir uyarı göstermez.

- Yönetim konsolundaki **Uyarı dışlama** kurallarını kullanarak bu uyarıyı dışladığınızdan emin olun. 

### <a name="investigate-widgets-that-show-no-data"></a>Veri gösteren pencere öğelerini araştırın

**Eğilimler & istatistikleri** penceresindeki pencere öğeleri veri göstermesiz, şunları yapın:

- [Sistem performansını denetleyin](#check-system-performance).

- Zaman ve bölge ayarlarının doğru şekilde yapılandırıldığından ve gelecekte bir zamana ayarlı olmadığından emin olun. 

### <a name="investigate-a-device-map-that-shows-only-broadcasting-devices"></a>Yalnızca yayın cihazlarını gösteren bir cihaz haritasını araştırın

Haritada gösterilen cihazlar birbirlerine bağlı değilse, YAYıLMA bağlantı noktası yapılandırmasıyla ilgili bir sorun olabilir. Diğer bir deyişle, yalnızca yayın cihazlarını görüyor ve tek noktaya yayın trafiği bulunmayabilir.

:::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/broadcasting-devices.png" alt-text="Yayın cihazlarınızı görüntüleyin.":::

Böyle bir durumda, yalnızca yayın trafiğini görebileceğiniz doğrulamanız gerekir. Ardından, tek noktaya yayın trafiğini görebilmeniz için ağ mühendisinden YAYıLMA bağlantı noktası yapılandırmasını düzeltmesini isteyin.

Yalnızca yayın trafiğini gördüğünüzü doğrulamak için:

- **Veri araştırma** ekranında, **Tümü** seçeneğini kullanarak bir rapor oluşturun. Daha sonra raporda yalnızca yayın ve çok noktaya yayın trafiği (tek noktaya yayın trafiği yok) görünüp göründüğünü görün.

Veya

- Doğrudan anahtardan bir PCAP kaydı yapın veya Wireshark kullanarak bir dizüstü bilgisayar bağlayın.

### <a name="connect-the-sensor-to-ntp"></a>Algılayıcıyı NTP 'e bağlama

NTP 'e bağlanmak için bir tek başına algılayıcı ve onunla ilgili sensörlerle bir yönetim konsolu yapılandırabilirsiniz.

Tek başına algılayıcıyı NTP 'e bağlamak için:

- [Yardım almak Için destek ekibine başvurun](https://support.microsoft.com/en-us/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099).

Yönetim Konsolu tarafından denetlenen bir algılayıcıyı NTP 'e bağlamak için:

- NTP bağlantısı, yönetim konsolunda yapılandırılır. Yönetim Konsolu denetimlerinin tüm sensörleri NTP bağlantısını otomatik olarak alır.

### <a name="investigate-when-devices-arent-shown-on-the-map-or-you-have-multiple-internet-related-alerts"></a>Cihazların haritada ne zaman gösterilmemiş olduğunu veya Internet ile ilgili birden çok uyarı olduğunu araştırın

Bazen ICS cihazları dış IP adresleriyle yapılandırılır. Bu ICS cihazları haritada gösterilmez. Cihazlar yerine, haritada bir internet bulutu görüntülenir. Bu cihazların IP adresleri, bulut görüntüsüne dahil edilmiştir.

Aynı sorunun bir diğer göstergesi, Internet ile ilgili birden çok uyarının görünmediğinin bir göstergesidir.

:::image type="content" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/alert-problems.png" alt-text="Birden çok Internet ile ilgili uyarı.":::

Yapılandırmayı onarmak için:

1. Cihaz haritasında bulut simgesine sağ tıklayın ve **IP adreslerini dışarı aktar**' ı seçin. Özel olan ortak aralıkları kopyalayın ve bunları alt ağ listesine ekleyin. Daha fazla bilgi için bkz. [alt ağları yapılandırma](how-to-control-what-traffic-is-monitored.md#configure-subnets).

2. İnternet bağlantıları için yeni bir veri araştırma raporu oluşturun.

3. Veri araştırma raporunda, :::image type="icon" source="media/how-to-troubleshoot-the-sensor-and-on-premises-management-console/administrator-mode.png" border="false"::: yönetici moduna girmek ve ICS cihazlarınızın IP adreslerini silmek için seçin.

### <a name="tweak-the-sensors-quality-of-service"></a>Sensörin hizmet kalitesini ince ayar

Ağ kaynaklarınızı kaydetmek için, sensörin günlük yordamlar için kullandığı arabirim bant genişliğini sınırlayabilirsiniz.

Arabirim bant genişliğini sınırlandırmak için, `cyberx-xsense-limit-interface` sudo izinleriyle çalıştırılması gereken CLI aracını kullanın. Araç aşağıdaki bağımsız değişkenleri alır:

  - `* -i`: arabirimler (örnek: eth0).

  - `* -l`: limit (örnek: 30 kbit/1 Mbit). Şu bant genişliği birimlerini kullanabilirsiniz: Kbps, Mbps, kbit, Mbit veya bps.

  - `* -c`: Clear (arabirim bant genişliği sınırlamasını temizlemek için).

Hizmet kalitesini ince ayar için:

1. Bir IoT kullanıcısı için bir Defender olarak algılayıcı CLı 'da oturum açın ve girin `sudo cyberx-xsense-limit-interface-I eth0 -l value` .

   Örnek: `sudo cyberx-xsense-limit-interface -i eth0 -l 30kbit`

   > [!NOTE]
   > Fiziksel bir gereç için EM1 arabirimini kullanın.

2. Arabirim sınırlamasını temizlemek için girin `sudo cyberx-xsense-limit-interface -i eth0 -l 1mbps -c` .

## <a name="on-premises-management-console-troubleshooting-tools"></a>Şirket içi yönetim konsolu sorun giderme araçları

### <a name="investigate-a-lack-of-expected-alerts"></a>Beklenen uyarıların eksik olduğunu araştırın

**Uyarılar** penceresinde beklenen bir uyarı gösterilmezse, aşağıdakileri doğrulayın:

- Aynı uyarının, farklı bir güvenlik örneğine yeniden eylem olarak **Uyarılar** penceresinde zaten görünüp göründüğünü denetleyin. Yanıt Evet ise ve bu uyarı henüz işlenmediyse yeni bir uyarı gösterilmez.

- Şirket içi yönetim konsolundaki **Uyarı dışlama** kurallarını kullanarak bu uyarıyı dışlamadınız emin olun.  

### <a name="tweak-the-quality-of-service"></a>Hizmet kalitesini ince ayar

Ağ kaynaklarınızı kaydetmek için, bir gereç ve şirket içi yönetim konsolu arasında tek bir eşitleme işleminde dış sistemlere (e-postalar veya SıEM gibi) gönderilen uyarı sayısını sınırlayabilirsiniz.

Varsayılan değer 50 ' dir. Yani, bir gereç ve şirket içi yönetim konsolu arasındaki bir iletişim oturumunda, dış sistemlere 50 'den fazla uyarı olmayacaktır. 

Uyarı sayısını sınırlandırmak için ' de bulunan özelliği kullanın `notifications.max_number_to_report` `/var/cyberx/properties/management.properties` . Bu özelliği değiştirdikten sonra yeniden başlatma gerekmez.

Hizmet kalitesini ince ayar için:

1. IoT kullanıcısı için bir Defender olarak oturum açın. 

2. Varsayılan değerleri doğrulayın:

   ```bash
   grep \"notifications\" /var/cyberx/properties/management.properties
   ```

   Aşağıdaki varsayılan değerler görünür:

   ```bash
   notifications.max_number_to_report=50
   notifications.max_time_to_report=10 (seconds)
   ```

3. Varsayılan ayarları düzenleyin:

   ```bash
   sudo nano /var/cyberx/properties/management.properties
   ```

4. Aşağıdaki satırların ayarlarını düzenleyin:

   ```bash
   notifications.max_number_to_report=50
   notifications.max_time_to_report=10 (seconds)
   ```

5. Değişiklikleri kaydedin. Yeniden başlatma gerekmez.

## <a name="export-information-for-troubleshooting"></a>Sorun giderme için dışarı aktarma bilgileri

Ağınızı izlemeye ve çözümlemeye yönelik araçların yanı sıra, daha fazla araştırma için destek ekibine bilgi gönderebilirsiniz. Günlükleri dışa aktardığınızda, algılayıcı ayrı bir metin dosyasında dışa aktarılmış Günlükler için otomatik olarak bir kerelik parola (OTP) oluşturur. 

Günlükleri dışarı aktarmak için:

1. Sol bölmede **sistem ayarları**' nı seçin.

2. **Günlükleri Dışarı Aktar** öğesini seçin.

    :::image type="content" source="media/how-to-export-information-for-troubleshooting/export-a-log.png" alt-text="Bir günlüğü sistem desteğine aktarma.":::

3. **Dosya adı** kutusuna, günlük dışarı aktarma için kullanmak istediğiniz dosya adını girin. Varsayılan değer geçerli tarihtir.

4. Dışarı aktarmak istediğiniz verileri tanımlamak için veri kategorilerini seçin:  

    | Kategoriyi dışarı aktar | Açıklama |
    |--|--|
    | **İşletim sistemi günlükleri** | İşletim sistemi durumu hakkında bilgi almak için bu seçeneği belirleyin. |
    | **Yükleme/yükseltme günlükleri** | Yükleme ve yükseltme yapılandırma parametrelerinin araştırılması için bu seçeneği belirleyin. |
    | **Sistem Sanity çıkışı** | Sistem performansını denetlemek için bu seçeneği belirleyin. |
    | **Dissection günlükleri** | Protokol Dissection 'ın gelişmiş incelemesini sağlamak için bu seçeneği belirleyin. |
    | **İşletim sistemi çekirdeği dökümleri** | Çekirdek bellek dökümünü dışarı aktarmak için bu seçeneği belirleyin. Çekirdek bellek dökümü, bu çekirdekte oluşan sorun sırasında çekirdeğin kullandığı tüm belleği içerir. Döküm dosyasının boyutu, tüm bellek dökümünden daha küçüktür. Genellikle, döküm dosyası sistemdeki fiziksel belleğin tek üçte biridir. |
    | **Günlükler iletiliyor** | İletme kurallarının araştırılması için bu seçeneği belirleyin. |
    | **SNMP günlükleri** | SNMP sistem durumu denetimi bilgilerini almak için bu seçeneği belirleyin. |
    | **Çekirdek uygulama günlükleri** | Çekirdek uygulama yapılandırması ve işlemle ilgili verileri dışarı aktarmak için bu seçeneği belirleyin. |
    | **CM günlükleriyle iletişim** | Yönetim konsoluyla sürekli sorunlar veya bağlantı kesintileri varsa bu seçeneği belirleyin. |
    | **Web uygulaması günlükleri** | Uygulamanın web arabiriminden gönderilen tüm istekler hakkında bilgi almak için bu seçeneği belirleyin. |
    | **Sistem yedeklemesi** | Sistemin tam durumunu araştırmak için tüm sistem verilerinin bir yedeklemesini dışarı aktarmak için bu seçeneği belirleyin. |
    | **Dissection Istatistikleri** | Protokol istatistiklerinin gelişmiş incelemesini sağlamak için bu seçeneği belirleyin. |
    | **Veritabanı günlükleri** | Günlükleri sistem veritabanından dışarı aktarmak için bu seçeneği belirleyin. Sistem günlüklerini araştırmak, sistem sorunlarını tanımlamaya yardımcı olur. |
    | **Yapılandırma** | Her şeyin doğru şekilde yapılandırıldığından emin olmak için tüm yapılandırılabilir parametrelerle ilgili bilgileri dışarı aktarmak için bu seçeneği belirleyin. |

5. Tüm seçenekleri belirlemek için **Kategoriler Seç**' ın yanındaki **Tümünü Seç** ' i seçin.

6. **Günlükleri Dışarı Aktar** öğesini seçin.

İçe aktarılmış Günlükler **arşivlenmiş günlükler** listesine eklenir. OTP 'yi destek ekibine ayrı bir ileti ile ve orta/veya orta düzeydeki günlüklere gönderin. Destek ekibi, yalnızca günlükleri şifrelemek için kullanılan benzersiz OTP 'yi kullanarak dışarı aktarılmış günlükleri ayıklayabilecektir.

Arşivlenmiş günlüklerin listesi en fazla beş öğe içerebilir. Listedeki öğe sayısı bu sayının ötesine geçtiğinde, en erken öğe silinir.

## <a name="see-also"></a>Ayrıca bkz.

- [Uyarıları görüntüleme](how-to-view-alerts.md)

- [SNMP MıB izlemeyi ayarlama](how-to-set-up-snmp-mib-monitoring.md)

- [Algılayıcı bağlantısının kesilmesi olaylarını anlama](how-to-manage-sensors-from-the-on-premises-management-console.md#understand-sensor-disconnection-events)
