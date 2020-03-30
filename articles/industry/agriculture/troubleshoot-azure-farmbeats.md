---
title: Azure FarmBeats sorunlarını giderme
description: Bu makalede, Azure FarmBeats sorun giderme nasıl açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: a429a1e454e73a1a9d544e308e5b2d60052d91a9
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349766"
---
# <a name="troubleshoot"></a>Sorun giderme

Bu makalede, ortak Azure FarmBeats sorunlarına çözümler sağlar.

Ek yardım için bize farmbeatssupport@microsoft.comulaşın. **Deployer.log** dosyasını e-postanıza eklediğinden emin olun.

**deployer.log** dosyasını indirmek için aşağıdakileri yapın:

1. **Azure portalında** oturum açın ve aboneliğinizi ve Azure AD kiracınızı seçin.
2. Cloud Shell’i Azure portalında en üst gezinti bölmesinden başlatın.
3. Tercih edilen Cloud Shell deneyimi olarak **Bash'i** seçin.
4. Vurgulanan simgeyi seçin ve ardından açılan listede **İndir'i**seçin.

    ![Proje FarmBeats](./media/troubleshoot-azure-farmbeats/download-deployer-log-1.png)

5. Sonraki bölmede, **deployer.log** dosyanıza giden yolu girin. Örneğin, **farmbeats-deployer.log**girin.

## <a name="sensor-telemetry"></a>Sensör telemetri

### <a name="cant-view-telemetry-data"></a>Telemetri verilerini görüntüleyemiyorum

**Belirti**: Cihazlar veya sensörler dağıtılır ve FarmBeats'i aygıt ortağınızla bağladınız, ancak FarmBeats'teki telemetri verilerini alamaz veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. FarmBeats Datahub kaynak grubunuza gidin.   
2. Olay **Hub'ını** (DatafeedEventHubNamespace) seçin ve ardından gelen ileti sayısını denetleyin.
3. Aşağıdakilerden birini yapın:   
   - *Gelen ileti yoksa,* aygıt ortağınızla iletişime geçin.  
   - *Gelen iletiler*varsa, iletişim farmbeatssupport@microsoft.com. Datahub ve Hızlandırıcı günlüklerinizi ve yakalanan telemetrinizi tekme.

Günlükleri nasıl indireceklerini anlamak için ["Günlükleri el ile topla"](#collect-logs-manually) bölümüne gidin.  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerinizden geçmiş/akışverilerini sindirdikten sonra telemetri verilerini görüntüleyemiyorum

**Belirti**: Cihazlar veya sensörler dağıtılır ve FarmBeats'teki cihazları/sensörleri oluşturdunuz ve EventHub'a telemetri aldınız, ancak FarmBeats'teki telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. İş ortağı kaydını doğru yaptığınızdan emin olun - datahub swagger'ınıza giderek bunu kontrol edebilir, /İş Ortağı API'sine gidin, Ortak kayıtlı olup olmadığını alın ve kontrol edin. Değilse, iş ortağı eklemek için [aşağıdaki adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.
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

**Düzeltici eylem**:

1. Datahub Swagger'da İş Ortağı API'sine gidin.
2. **Çalıştır'ı** > **Deneyin'i** > **seçin.**
3. İlgilendiğiniz sensör ortağının iş ortağının kimliğine dikkat edin.
4. İş Ortağı API'sine geri dön ve **>Kimliği\<Al'ı **seçin.
5. Adım 3'ten ortak kimliğini belirtin ve ardından **Yürüt'ün'u**seçin.

   API yanıtı Olay Hub'ları bağlantı dizeolmalıdır.

### <a name="device-appears-offline"></a>Aygıt çevrimdışı görünüyor

**Belirtiler**: Cihazlar yüklenir ve FarmBeats'i aygıt ortağınızla ilişkilendirmişsiniz. Aygıtlar çevrimiçi ve telemetri verileri gönderiyor, ancak çevrimdışı görünüyorlar.

**Düzeltici eylem**: Raporlama aralığı bu aygıt için yapılandırılmamıştır. Raporlama aralığını ayarlamak için aygıt üreticinize başvurun. 

### <a name="error-deleting-a-device"></a>Aygıtı silme hatası

Aygıtı silerken, aşağıdaki yaygın hata senaryolarından biriyle karşılaşabilirsiniz:  

**Mesaj**: "Cihaz sensörlerde referans: Cihazla ilişkili bir veya daha fazla sensör vardır. Sensörleri silin ve sonra cihazı silin."  

**Anlamı**: Cihaz, çiftlikte bulunan birden fazla sensörle ilişkilidir.   

**Düzeltici eylem**:  

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

    > İş ortaklarının bir aygıtı veya sensörü silme erişimi yoktur. Yalnızca Yöneticiler aynı şeyi yapabilir.


## <a name="issues-with-jobs"></a>İşlerle ilgili sorunlar

### <a name="farmbeats-internal-error"></a>FarmBeats iç hata

**İleti**: "FarmBeats iç hatası, daha fazla bilgi için sorun giderme kılavuzuna bakın".

**Düzeltici eylem**: Bu sorun, veri ardışık etki alanında geçici bir hatadan kaynaklanabilir. İşi yeniden oluşturun. Hata devam ederse, FarmBeats forumundaki bir gönderiye hata iletisi ekleyin veya iletişim kurun. FarmBeatsSupport@microsoft.com

## <a name="accelerator-troubleshooting"></a>Hızlandırıcı sorun giderme

### <a name="access-control"></a>Erişim denetimi

**Sorun**: Rol ataması eklerken bir hata alırsınız.

**İleti**: "Eşleşen kullanıcı bulunamadı."

**Düzeltici eylem**: Rol ataması eklemeye çalıştığınız e-posta kimliğini kontrol edin. E-posta kimliği, bu kullanıcı için Active Directory'de kayıtlı olan kimliğin tam eşleşmesi olmalıdır. Hata devam ederse, FarmBeats forumundaki bir gönderiye hata iletisi ekleyin veya iletişim kurun. FarmBeatsSupport@microsoft.com

### <a name="unable-to-log-in-to-accelerator"></a>Hızlandırıcı'da oturum açamıyor

**Mesaj**: "Hata: Hizmeti arama yetkiniz yok. Yetkilendirme için yöneticiye başvurun."

**Düzeltici eylem**: Yöneticiden FarmBeats dağıtımına erişmeniz için yetki vermelerini isteyin. Bu, RoleAssignment API'lerinin post'u yaparak veya Hızlandırıcı'daki **Ayarlar** bölmesinde Erişim Denetimi aracılığıyla yapılabilir.  

Zaten erişim izni niz varsa ve bu hatayla karşı karşıyaysanız, sayfayı yenileyerek yeniden deneyin. Hata devam ederse, FarmBeats forumundaki bir gönderiye hata iletisi ekleyin veya iletişim kurun. FarmBeatsSupport@microsoft.com

![Proje FarmBeats](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Hızlandırıcı sorunları  

**Sorun**: Belirlenemeyen bir nedenden oluşan bir Hızlandırıcı hatası aldınız.

**İleti**: "Hata: Bilinmeyen bir hata oluştu."

**Düzeltici eylem**: Sayfayı çok uzun süre boş tasak ederseniz bu hata oluşur. Sayfayı yenileyin.  

Hata devam ederse, FarmBeats forumundaki bir gönderiye hata iletisi ekleyin veya iletişim kurun. FarmBeatsSupport@microsoft.com

**Sorun**: FarmBeats Accelerator, FarmBeatsDeployment'ı yükselttikten sonra bile en son sürümü göstermiyor.

**Düzeltici eylem**: Bu hata, tarayıcıda servis çalışanı kalıcılığı nedeniyle oluşur. Şunları yapın:

1. Hızlandırıcı açık olan tüm tarayıcı sekmelerini kapatın ve tarayıcı penceresini kapatın.
2. Tarayıcının yeni bir örneğini başlatın ve Hızlandırıcı URI'yi yeniden yükleyin. Bu eylem Hızlandırıcı'nın yeni sürümünü yükler.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: Görüntülerle ilgili sorunlar

### <a name="wrong-username-or-password"></a>Yanlış kullanıcı adı veya parola

**İş hatası iletisi**: "Bu kaynağa erişmek için tam kimlik doğrulama gerekir."

**Düzeltici eylem**:

Aşağıdakilerden birini yapın:

- Datahub'ı doğru kullanıcı adı ve parolayla yükseltmek için yükleyiciyi yeniden çalıştırın.
- Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Sentinel hub'ı: Yanlış URL veya site erişilemez 

**İş başarısızlık mesajı**: "Oops, bir şeyler yanlış gitti. Erişmeye çalıştığınız sayfa (geçici olarak) kullanılamıyor." 

**Düzeltici eylem**:
1. Web sitesinin erişilebilir olup olmadığını görmek için tarayıcınızda [Sentinel'i](https://scihub.copernicus.eu/dhus/) açın. 
2. Web sitesine erişilemiyorsa, herhangi bir güvenlik duvarının, şirket ağının veya diğer engelleme yazılımların web sitesine erişimi engelleyip engellemediğini kontrol edin ve Sentinel URL'ye izin vermek için gerekli adımları atın. 
3. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel sunucusu: Bakım için aşağı

**İş hatası mesajı**: "Copernicus Open Access Hub yakında geri dönecek! Verdiğimiz rahatsızlıktan dolayı özür dileriz, şu anda biraz bakım yapıyoruz. Kısa bir süre sonra tekrar çevrimiçi olacağız!" 

**Düzeltici eylem**:

Sentinel sunucusunda herhangi bir bakım aktivitesi yapılıyorsa bu sorun oluşabilir.

1. Bakım gerçekleştirildiği için herhangi bir iş veya ardışık işlem başarısız olursa, bir süre sonra işi yeniden gönderin. 

   Planlanan veya planlanmamış Sentinel bakım faaliyetleri hakkında daha fazla bilgi için [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: Ulaşılan maksimum bağlantı sayısı

**İş hatası iletisi**: "Kullanıcı 'kullanıcı adı\<>' tarafından elde edilen en fazla iki eşzamanlı akış sayısı."

**Anlamı**: Bir iş, en fazla bağlantı sayısına ulaşıldığı için başarısız olursa, aynı Sentinel hesabı başka bir yazılım dağıtımında kullanılıyor.

**Düzeltici eylem**: Aşağıdakilerden birini deneyin:

* Yeni bir Sentinel hesabı oluşturun ve yeni bir Sentinel kullanıcı adı ve parola kullanarak Datahub'ı yükseltmek için yükleyiciyi yeniden çalıştırın.  
* Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri iş çalıştırın ve sonra işin başarılı olup olmadığını denetleyin.

### <a name="sentinel-server-refused-connection"></a>Sentinel sunucusu: Reddedilen bağlantı 

**İş hatası iletisi**: http://172.30.175.69:8983/solr/dhus"Sunucu bağlantı reddetti: ." 

**Düzeltici eylem**: Sentinel sunucusunda herhangi bir bakım faaliyeti yapılıyorsa bu sorun oluşabilir. 
1. Bakım gerçekleştirildiği için herhangi bir iş veya ardışık işlem başarısız olursa, bir süre sonra işi yeniden gönderin. 

   Planlanan veya planlanmamış Sentinel bakım faaliyetleri hakkında daha fazla bilgi için [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ila 7 günlük bir tarih aralığı için uydu endeksleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="soil-moisture-map-has-white-areas"></a>Toprak Nem haritası beyaz alanlara sahiptir 

**Sorun**: Toprak Nem haritası oluşturuldu, ancak harita çoğunlukla beyaz alanlara sahiptir.

**Düzeltici eylem**: Haritanın istendiği süre için oluşturulan uydu endekslerinde 0,3'ten küçük NDVI değerleri varsa bu sorun oluşabilir. Daha fazla bilgi için lütfen [Sentinel'den Teknik Rehber'i ziyaret edin.](https://earth.esa.int/web/sentinel/technical-guides/sentinel-2-msi/level-2a/algorithm)
1. Farklı bir tarih aralığı için işi yeniden çalıştırın ve uydu endekslerinde NDVI değerlerinin 0,3'ten fazla olup olmadığını kontrol edin

## <a name="collect-logs-manually"></a>Günlükleri el ile toplama

[Azure Depolama Gezgini'ni yükleyin ve dağıtın.]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

### <a name="collect-azure-data-factory-job-logs-in-datahub"></a>Datahub'da Azure Veri Fabrikası iş günlüklerini toplama

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Arama** kutusunda FarmBeats Datahub kaynak grubunu arayın.

    > [!NOTE]
    > FarmBeats yüklemesi sırasında belirttiğiniz Datahub kaynak grubunu seçin.

3. Kaynak **Grubu** panosunda *datahublogs\* * depolama hesabını arayın. Örneğin, **datahublogsmvxmq**için arama .  
4. **Ad** sütununda, **Depolama Hesabı** panosunu görüntülemek için depolama hesabını seçin.
5. **\* Datahubblogs** bölmesinde, **Açık Azure Depolama Gezgini** uygulamasını görüntülemek için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob Kapsayıcıları'nı**seçin ve ardından **iş günlüklerini**seçin.
7. İş **günlükleri** bölmesinde **İndir'i**seçin.
8. Günlükleri makinenizdeki yerel bir klasöre indirin.
9. İndirilen .zip dosyasını farmbeatssupport@microsoft.com.

    ![Proje FarmBeats](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-in-accelerator"></a>Hızlandırıcı'da Azure Veri Fabrikası iş günlüklerini topla

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Arama** kutusunda FarmBeats Hızlandırıcı kaynak grubunu arayın.

    > [!NOTE]
    > FarmBeats yüklemesi sırasında belirttiğiniz Hızlandırıcı kaynak grubunu seçin.

3. Kaynak **Grubu** panosunda *depolama\* * alanı hesabını arayın. Örneğin, **storagedop4k\*** için arama .
4. **Depolama Hesabı** panosunu görüntülemek için **Ad** sütunundaki depolama hesabını seçin.
5. **Depolama\* ** bölmesinde, Azure Depolama Gezgini uygulamasını açmak için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob Kapsayıcıları'nı**seçin ve ardından **iş günlüklerini**seçin.
7. İş **günlükleri** bölmesinde **İndir'i**seçin.
8. Günlükleri makinenizdeki yerel bir klasöre indirin.
9. İndirilen .zip dosyasını farmbeatssupport@microsoft.com.


### <a name="collect-datahub-app-service-logs"></a>Datahub uygulama hizmet günlüklerini topla

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Arama** kutusunda FarmBeats Datahub kaynak grubunu arayın.

    > [!NOTE]
    > FarmBeats yüklemesi sırasında belirttiğiniz Datahub kaynak grubunu seçin.

3. Kaynak grubunda, *datahublogs\* * depolama hesabı arayın. Örneğin, **fordatahublogsmvxmq\*** için arama .
4. **Depolama Hesabı** panosunu görüntülemek için **Ad** sütunundaki depolama hesabını seçin.
5. **Datahubblogs\* ** bölmesinde, Azure Depolama Gezgini uygulamasını açmak için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob**Kapsayıcıları'nı seçin ve ardından **appinsights-logs'u**seçin.
7. **Appinsights-logs** bölmesinde **İndir'i**seçin.
8. Günlükleri makinenizdeki yerel bir klasöre indirin.
9. İndirilen .zip dosyasını farmbeatssupport@microsoft.com.

### <a name="collect-accelerator-app-service-logs"></a>Hızlandırıcı uygulaması hizmet günlüklerini topla

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Arama** kutusunda FarmBeats Hızlandırıcı kaynak grubunu arayın.

    > [!NOTE]
    > FarmBeats kurulumu sırasında sağlanan FarmBeats Hızlandırıcı kaynak grubunu seçin.

3. Kaynak grubunda, *depolama\* * alanı hesabını arayın. Örneğin, **storagedop4k\*** için arama .
4. **Depolama Hesabı** panosunu görüntülemek için **Ad** sütunundaki depolama hesabını seçin.
5. **Depolama\* ** bölmesinde, Azure Depolama Gezgini uygulamasını açmak için **Explorer'da Aç'ı** seçin.
6. Sol bölmede **Blob**Kapsayıcıları'nı seçin ve ardından **appinsights-logs'u**seçin.
7. **Appinsights-logs** bölmesinde **İndir'i**seçin.
8. Günlükleri makinenizdeki yerel bir klasöre indirin.
9. İndirilen klasörü farmbeatssupport@microsoft.com' ye e-posta yla gönder.

## <a name="known-issues"></a>Bilinen sorunlar

## <a name="batch-related-issues"></a>Toplu işle ilgili sorunlar

**Hata iletisi**: "Belirtilen abonelik için Toplu Hesapların bölgesel hesap kotası ulaşıldı."

**Düzeltici eylem**: Kotayı artırın veya kullanılmayan toplu iş hesaplarını silin ve dağıtımı yeniden çalıştırın.

### <a name="azure-active-directory-azure-ad-related-issues"></a>Azure Etkin Dizin (Azure AD) ile ilgili sorunlar

**Hata iletisi**: "Gerekli ayarları Azure AD App d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0 güncelleştirmedi: İşlemi tamamlamak için yetersiz ayrıcalıklar. Yukarıdaki ayarların Azure AD Uygulaması için doğru şekilde yapılandırıldığından emin olun."

**Anlamı**: Azure AD uygulama kayıt yapılandırması düzgün tamamlanmadı.  

**Düzeltici eylem**: BT yöneticisinden (kiracı okuma erişimi olan kişi) Azure AD uygulama kaydını oluşturmak için [komut dosyamızı](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect/tree/master/AppCreationScripts) kullanmasını isteyin. Bu komut dosyası otomatik olarak yapılandırma adımlarını da halledin.

**Hata iletisi**: "Bu kiracıda\<yeni\>Active Directory Application ' uygulama adı oluşturulamadı: Özellik tanımlayıcıi II'leri için aynı değere sahip başka bir nesne zaten var."

**Anlamı**: Aynı ada sahip bir Azure AD uygulama kaydı zaten var.

**Düzeltici eylem**: Varolan Azure AD uygulama kaydını silin veya yükleme için yeniden kullanın. Mevcut Azure AD uygulama kaydını yeniden kullanıyorsanız, uygulama kimliğini ve istemci sırrını yükleyiciye iletin ve yeniden dağıtın.

## <a name="issues-with-the-inputjson-file"></a>input.json dosyasıyla ilgili sorunlar

**Hata**: *input.json* dosyasından bir hata okuma girişi var.

**Düzeltici eylem**: Bu sorun genellikle yükleyiciye doğru *input.json* dosya yolunu veya adını belirtmedeki bir hatadan kaynaklanır. Uygun düzeltmeleri yapın ve dağıtımı yeniden deneyin.

**Hata**: *input.json* dosyasında değerleri ayrıştırma hatası var.

**Düzeltici eylem**: Bu sorun çoğunlukla *input.json* dosyasındaki yanlış değerler den kaynaklanır. Uygun düzeltmeleri yapın ve dağıtımı yeniden deneyin.

## <a name="high-cpu-usage"></a>Yüksek CPU kullanımı

**Hata**: **Yüksek CPU Kullanım Uyarısı'na**başvuran bir e-posta uyarısı alırsınız. 

**Düzeltici eylem**: 
1. FarmBeats Datahub kaynak grubunuza gidin.
2. Uygulama **hizmetini**seçin.  
3. [Uygulama Hizmeti fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/app-service/windows/)ölçeklendir'e gidin ve ardından uygun bir fiyatlandırma katmanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Hala FarmBeats sorunlarıyla karşı karşıyaysanız, [Destek Forumu'muza](https://aka.ms/farmbeatssupport)başvurun.
