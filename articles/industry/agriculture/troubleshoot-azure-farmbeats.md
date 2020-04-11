---
title: Azure FarmBeats sorunlarını giderme
description: Bu makalede, Azure FarmBeats sorun giderme nasıl açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: a40c64910260a2d63a529d25e1089fb618fcec1b
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113500"
---
# <a name="troubleshoot"></a>Sorun giderme

Bu makalede, ortak Azure FarmBeats sorunlarına çözümler sağlar. Ek yardım için [Destek](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats) Forumu'muza farmbeatssupport@microsoft.combaşvurun veya bize e-posta gönderin.

> [!NOTE]
  > Nisan ayında FarmBeats'i yüklediyseniz ve işleriniz boş bir hata iletisiyle başarısız oluyorsa, yüklemenize kritik sağlık ve güvenlik kuruluşları için destek önceliklerini belirlemek için toplu iş kotası tahsis edilmemiş olabilir. Daha fazla bilgi için [buraya](https://azure.microsoft.com/blog/update-2-on-microsoft-cloud-services-continuity/) bakın. İşleri başarılı bir şekilde çalıştırmak için Toplu İşlemler hesabına tahsis edilmesi için VM'lerin istenmesi gerekir.

## <a name="install-issues"></a>Yükleme sorunları

  > [!NOTE]
  > Bir hata nedeniyle yüklemeyi yeniden başlatıyorsanız, yüklemeyi yeniden tetiklemeden önce **Kaynak Grubu'nu** sildiğinizden veya Kaynak Grubu'ndan tüm kaynakları silmeden emin olun.

### <a name="invalid-sentinel-credentials"></a>Geçersiz Sentinel kimlik bilgileri

Yükleme sırasında sağlanan Sentinel kimlik bilgileri yanlış. Yüklemeyi doğru kimlik bilgileriyle yeniden başlatın.

### <a name="the-regional-account-quota-of-batch-accounts-for-the-specified-subscription-has-been-reached"></a>Belirtilen abonelik için Toplu Hesaplar'ın bölgesel hesap kotası

Kotayı artırın veya kullanılmayan toplu iş hesaplarını silin ve yüklemeyi yeniden başlatın.

### <a name="invalid-resource-group-location"></a>Geçersiz kaynak grubu konumu

Kaynak **Grubu'nun** yükleme sırasında belirtilen **Bölge** ile aynı konumda olduğundan emin olun.

### <a name="other-install-issues"></a>Diğer yükleme sorunları

Aşağıdaki ayrıntılar la bize ulaşın:

- Abonelik Kimliğiniz
- Kaynak Grubu adı
- Dağıtım hatası için günlük dosyasını eklemek için aşağıdaki adımları izleyin:

    1. Azure portalındaki **Kaynak Grubu'na** gidin.

    2. Sol taraftaki **Ayarlar** bölümünde **Dağıtımlar'ı** seçin.

    3. **Başarısız'ı**gösteren her dağıtım için ayrıntılara doğru seçim yapın ve dağıtım ayrıntılarını indirin. Bu dosyayı postaya takın.

## <a name="sensor-telemetry"></a>Sensör telemetri

### <a name="cant-view-telemetry-data"></a>Telemetri verilerini görüntüleyemiyorum

**Belirti**: Cihazlar veya sensörler dağıtılır ve FarmBeats'i aygıt ortağınızla bağladınız, ancak FarmBeats'teki telemetri verilerini alamaz veya görüntüleyemezsiniz.

**Düzeltici eylem**

1. FarmBeats Datahub kaynak grubunuza gidin.
2. Olay **Hub'ını** (DatafeedEventHubNamespace) seçin ve ardından gelen ileti sayısını denetleyin.
3. Aşağıdakilerden birini yapın:

   - *Gelen ileti yoksa,* aygıt ortağınızla iletişime geçin.  
   - *Gelen iletiler*varsa, Datahub ve Hızlandırıcı günlükleriniz ve yakalanan telemetriniz ile bize ulaşın.

Günlükleri nasıl indireceklerini anlamak için ["Günlükleri el ile topla"](#collect-logs-manually) bölümüne gidin.  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerinizden geçmiş/akışverilerini sindirdikten sonra telemetri verilerini görüntüleyemiyorum

**Belirti**: Cihazlar veya sensörler dağıtılır ve FarmBeats'teki cihazları/sensörleri oluşturdunuz ve EventHub'a telemetri aldınız, ancak FarmBeats'teki telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**

1. İş ortağı kaydını doğru yaptığınızdan emin olun - datahub swagger'ınıza giderek bunu kontrol edebilir, /İş Ortağı API'sine gidin, Ortak kayıtlı olup olmadığını alın ve kontrol edin. Değilse, iş ortağı eklemek için aşağıdaki [adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.

2. Doğru Telemetri ileti biçimini kullandığınızdan emin olun:

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>":"<value>"
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        }
      ]
    }
 ]
}
```

### <a name="dont-have-the-azure-event-hubs-connection-string"></a>Azure Etkinlik Hub'ları bağlantı dizesi yok

**Düzeltici eylem**

1. Datahub Swagger'da İş Ortağı API'sine gidin.
2. **Çalıştır'ı** > **Deneyin'i** > **seçin.**

> [!NOTE]
> İlgilendiğiniz sensör ortağının ortak kimliği.

3. İş Ortağı API'sine geri dön ve **>Kimliği\<Al'ı **seçin.
4. Adım 3'ten ortak kimliğini belirtin ve ardından **Yürüt'ün'u**seçin.

   API yanıtı Olay Hub'ları bağlantı dizeolmalıdır.

### <a name="device-appears-offline"></a>Aygıt çevrimdışı görünüyor

**Belirtiler**: Cihazlar yüklenir ve FarmBeats'i aygıt ortağınızla ilişkilendirmişsiniz. Aygıtlar çevrimiçi ve telemetri verileri gönderiyor, ancak çevrimdışı görünüyorlar.

**Düzeltici eylem** Raporlama aralığı bu aygıt için yapılandırılmamıştır. Raporlama aralığını ayarlamak için aygıt üreticinize başvurun. 

### <a name="error-deleting-a-device"></a>Aygıtı silme hatası

Aygıtı silerken, aşağıdaki yaygın hata senaryolarından biriyle karşılaşabilirsiniz:  

**Mesaj**: "Cihaz sensörlerde referans: Cihazla ilişkili bir veya daha fazla sensör vardır. Sensörleri silin ve sonra cihazı silin."  

**Anlamı**: Cihaz, çiftlikte bulunan birden fazla sensörle ilişkilidir.

**Düzeltici eylem**  

1. Hızlandırıcı aracılığıyla cihazla ilişkili sensörleri silin.  
2. Sensörleri farklı bir cihazla ilişkilendirmek istiyorsanız, aygıt ortağınızdan da aynısını yapmasını isteyin.  
3. Bir `DELETE API` arama kullanarak aygıtı silin ve kuvvet parametresini *doğru*olarak ayarlayın.  

**Mesaj**: "Aygıt, aygıtlarda ParentDeviceId olarak başvurulur: Bu aygıtla alt aygıt olarak ilişkili bir veya daha fazla aygıt vardır. Bunları silin ve sonra bu aygıtı silin."  

**Anlamı**: Cihazınızda bununla ilişkili başka aygıtlar da vardır.  

**Düzeltici eylem**

1. Bu belirli aygıtla ilişkili aygıtları silin.  
2. Belirli aygıtı silin.  

    > [!NOTE]
    > Sensörler aygıtla ilişkiliyse aygıtı silemezsiniz. İlişkili sensörlerin nasıl silindikhakkında daha fazla bilgi için sensör [ortaklarından sensör verilerini al'daki](get-sensor-data-from-sensor-partner.md) **Delete sensörü** bölümüne bakın.
    > İş ortaklarının bir cihazı veya sensörü silme izni yoktur. Yalnızca Yöneticilerin silme izni vardır.

## <a name="issues-with-jobs"></a>İşlerle ilgili sorunlar

### <a name="farmbeats-internal-error"></a>FarmBeats iç hata

**İleti**: "FarmBeats iç hatası, daha fazla ayrıntı için sorun giderme kılavuzuna bakın."

**Düzeltici eylem** Bu sorun, veri ardışık durumunda geçici bir hata dan kaynaklanabilir. İşi yeniden oluşturun. Hata devam ederse, hata iletisi/günlükleri ile bize ulaşın.

## <a name="accelerator-troubleshooting"></a>Hızlandırıcı sorun giderme

### <a name="access-control"></a>Erişim denetimi

**Sorun**: Rol ataması eklerken bir hata alırsınız.

**İleti**: "Eşleşen kullanıcı bulunamadı."

**Düzeltici eylem** Rol ataması eklemeye çalıştığınız e-posta kimliğini denetleyin. E-posta kimliği, bu kullanıcı için Active Directory'de kayıtlı olan kimliğin tam eşleşmesi olmalıdır. Hata devam ederse, hata iletisi/günlükleri ile bize ulaşın.

### <a name="unable-to-log-in-to-accelerator"></a>Hızlandırıcı'da oturum açamıyor

**Mesaj**: "Hata: Hizmeti arama yetkiniz yok. Yetkilendirme için yöneticiye başvurun."

**Düzeltici eylem** Yöneticiden FarmBeats dağıtımına erişmeniz için yetki vermelerini isteyin. Bu, RoleAssignment API'lerinin post'u yaparak veya Hızlandırıcı'daki **Ayarlar** bölmesinde Erişim Denetimi aracılığıyla yapılabilir.  

Zaten erişim izni niz varsa ve bu hatayla karşı karşıyaysanız, sayfayı yenileyerek yeniden deneyin. Hata devam ederse, hata iletisi/günlükleri ile bize ulaşın.

![Proje FarmBeats](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Hızlandırıcı sorunları  

**Sorun**: Belirlenemeyen bir nedenden oluşan bir Hızlandırıcı hatası aldınız.

**İleti**: "Hata: Bilinmeyen bir hata oluştu."

**Düzeltici eylem** Sayfayı çok uzun süre boşta bırakırsanız bu hata oluşur. Sayfayı yenileyin. Hata devam ederse, hata iletisi/günlükleri ile bize ulaşın.

**Sorun**: FarmBeats Accelerator, FarmBeatsDeployment'ı yükselttikten sonra bile en son sürümü göstermiyor.

**Düzeltici eylem** Bu hata, tarayıcıda hizmet çalışanı kalıcılığı nedeniyle oluşur. Şunları yapın:

1. Hızlandırıcı açık olan tüm tarayıcı sekmelerini kapatın ve tarayıcı penceresini kapatın.
2. Tarayıcının yeni bir örneğini başlatın ve Hızlandırıcı URI'yi yeniden yükleyin. Bu eylem Hızlandırıcı'nın yeni sürümünü yükler.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: Görüntülerle ilgili sorunlar

### <a name="wrong-username-or-password"></a>Yanlış kullanıcı adı veya parola

**İş hatası iletisi**: "Bu kaynağa erişmek için tam kimlik doğrulama gerekir."

**Düzeltici eylem**: Aşağıdakilerden birini yapın:

- FarmBeats'i aşağıdaki adımları kullanarak doğru kullanıcı adı/parolayla güncelleştirin ve işi yeniden deneyin.

  **Sentinel kullanıcı adını güncelleştir**

    1. [Azure portalında](https://portal.azure.com)oturum açın.
    2. **Arama** kutusunda FarmBeats Datahub kaynak grubunu arayın.
    3. Depolama hesabı depolama***** > **Kapsayıcılar** > **toplu hazırlama dosyaları** > **to_vm** > **config.ini'yi** seçin
    4. **Edit'i** seçin
    5. sentinel_account bölümündeki kullanıcı adını güncelleştirme

  **Sentinel parolayı güncelleştir**

    1. [Azure portalında](https://portal.azure.com)oturum açın.
    2. **Arama** kutusunda FarmBeats Datahub kaynak grubunu arayın.
    3. Keyvault'u seçin-*****
    4. Ayarlar altında Erişim İlkeleri'ni seçin
    5. **Erişim Ekle İlkesi'ni** seçin
    6. Şablondan Yapılandırmak için **Gizli yönetimi** kullanın ve kendinizi Asıl'a ekleyin
    7. **Ekle'yi**seçin ve ardından **Access İlkeleri** sayfasında **Kaydet'i** seçin
    8. **Ayarlar** altında **Sırları** Seçin
    9. **Sentinel parolayı** seçin
    10. Değerin yeni bir sürümünü oluşturun ve etkinleştirin.

- Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Sentinel hub'ı: Yanlış URL veya site erişilemez

**İş başarısızlık mesajı**: "Oops, bir şeyler yanlış gitti. Erişmeye çalıştığınız sayfa (geçici olarak) kullanılamıyor."

**Düzeltici eylem**:

1. Web sitesinin erişilebilir olup olmadığını görmek için tarayıcınızda [Sentinel'i](https://scihub.copernicus.eu/dhus/) açın.
2. Web sitesine erişilemiyorsa, herhangi bir güvenlik duvarının, şirket ağının veya diğer engelleme yazılımların web sitesine erişimi engelleyip engellemediğini kontrol edin ve Sentinel URL'ye izin vermek için gerekli adımları atın. 
3. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel sunucusu: Bakım için aşağı

**İş hatası mesajı**: "Copernicus Open Access Hub yakında geri dönecek! Verdiğimiz rahatsızlıktan dolayı özür dileriz, şu anda biraz bakım yapıyoruz. Kısa bir süre sonra tekrar çevrimiçi olacağız!" 

**Düzeltici eylem**:

Sentinel sunucusunda herhangi bir bakım aktivitesi yapılıyorsa bu sorun oluşabilir.

1. Bakım gerçekleştirildiği için herhangi bir iş veya ardışık işlem başarısız olursa, bir süre sonra işi yeniden gönderin. 

   Planlanan veya planlanmamış Sentinel bakım faaliyetleri hakkında daha fazla bilgi için [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: Ulaşılan maksimum bağlantı sayısı

**İş hatası iletisi**: "Kullanıcı 'kullanıcı adı\<>' tarafından elde edilen en fazla iki eşzamanlı akış sayısı."

**Anlamı**: Bir iş, en fazla bağlantı sayısına ulaşıldığı için başarısız olursa, aynı Sentinel hesabı birden çok işte kullanılıyor.

**Düzeltici eylem**: Aşağıdakilerden birini deneyin:

* Başarısız işi yeniden çalıştırmadan önce diğer işlerin bitmesini bekleyin.
* Yeni bir Sentinel hesabı oluşturun ve FarmBeats'te Sentinel kullanıcı adını ve parolasını güncelleştirin.

### <a name="sentinel-server-refused-connection"></a>Sentinel sunucusu: Reddedilen bağlantı

**İş hatası iletisi**: http://172.30.175.69:8983/solr/dhus"Sunucu bağlantı reddetti: ."

**Düzeltici eylem**: Sentinel sunucusunda herhangi bir bakım faaliyeti yapılıyorsa bu sorun oluşabilir.

1. Bakım gerçekleştirildiği için herhangi bir iş veya ardışık işlem başarısız olursa, bir süre sonra işi yeniden gönderin.

   Planlanan veya planlanmamış Sentinel bakım faaliyetleri hakkında daha fazla bilgi için [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="soil-moisture-map-has-white-areas"></a>Toprak Nem haritası beyaz alanlara sahiptir

**Sorun**: **Toprak Nem haritası** oluşturuldu, ancak harita çoğunlukla beyaz alanlara sahiptir.

**Düzeltici eylem**: Haritanın istendiği süre için oluşturulan uydu endekslerinde 0,3'ten küçük NDVI değerleri varsa bu sorun oluşabilir. Daha fazla bilgi için [Sentinel'den Teknik Rehber'i](https://earth.esa.int/web/sentinel/technical-guides/sentinel-2-msi/level-2a/algorithm)ziyaret edin.

1. Farklı bir tarih aralığı için işi yeniden çalıştırın ve uydu endekslerinde NDVI değerlerinin 0,3'ten fazla olup olmadığını kontrol edin.

## <a name="collect-logs-manually"></a>Günlükleri el ile toplama

[Azure Depolama Gezgini'ni yükleyin ve dağıtın.]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

### <a name="collect-azure-data-factory-job-logs-or-app-service-logs-in-datahub"></a>Datahub'da Azure Veri Fabrikası iş günlüklerini veya Uygulama Hizmeti günlüklerini topla

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda FarmBeats Datahub kaynak grubunu arayın.
3. Kaynak **Grubu** panosunda *datahublogs\* * depolama hesabını arayın. Örneğin, *datahublogsmvxmq*.  
4. **Ad** sütununda, **Depolama Hesabı** panosunu görüntülemek için depolama hesabını seçin.
5. **\* Datahubblogs** bölmesinde, **Açık Azure Depolama Gezgini** uygulamasını görüntülemek için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob**Kapsayıcıları'nı seçin ve ardından Azure Veri Fabrikası günlükleri veya Uygulama Hizmeti günlükleri için **uygulama günlükleri** için iş **günlüklerini** seçin.
7. **İndir'i** seçin ve günlükleri makinenizdeki yerel bir klasöre indirin.

    ![Proje FarmBeats](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-or-app-service-logs-for-accelerator"></a>Hızlandırıcı için Azure Veri Fabrikası iş günlüklerini veya Uygulama Hizmeti günlüklerini topla

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda FarmBeats Hızlandırıcı kaynak grubunu arayın.
3. Kaynak **Grubu** panosunda *depolama\* * alanı hesabını arayın. Örneğin, *storagedop4k\**.
4. **Depolama Hesabı** panosunu görüntülemek için **Ad** sütunundaki depolama hesabını seçin.
5. **Depolama\* ** bölmesinde, Azure Depolama Gezgini uygulamasını açmak için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob**Kapsayıcıları'nı seçin ve ardından Azure Veri Fabrikası günlükleri veya Uygulama Hizmeti günlükleri için **uygulama günlükleri** için iş **günlüklerini** seçin.
7. **İndir'i** seçin ve günlükleri makinenizdeki yerel bir klasöre indirin.

## <a name="high-cpu-usage"></a>Yüksek CPU kullanımı

**Hata**: **Yüksek CPU Kullanım Uyarısı'na**başvuran bir e-posta uyarısı alırsınız.

**Düzeltici eylem**:

1. FarmBeats Datahub kaynak grubunuza gidin.
2. Uygulama **hizmetini**seçin.  
3. [Uygulama Hizmeti fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/app-service/windows/)ölçeklendir'e gidin ve ardından uygun bir fiyatlandırma katmanı seçin.
