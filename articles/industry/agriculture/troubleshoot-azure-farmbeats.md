---
title: Azure FarmBeats sorunlarını giderme
description: Bu makalede, Azure Farmtts ile ilgili sorunların nasıl giderileceği açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: c66d3d6fd3ee0bcba01db61183f40cd3ccf3f39d
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88797947"
---
# <a name="troubleshoot-azure-farmbeats"></a>Azure FarmBeats sorunlarını giderme

Bu makalede, yaygın Azure Farmtts sorunlarına yönelik çözümler sağlanmaktadır. Ek Yardım için [soru-cevap destek forumu](https://aka.ms/farmbeatssupport) veya bize e-posta&başvurun farmbeatssupport@microsoft.com .

> [!NOTE]
  > Nisan sırasında Farmtts yüklediyseniz ve işleriniz boş bir hata iletisiyle başarısız oluyorsa, yüklemeniz kritik sistem durumu ve güvenlik kuruluşları için destek önceliklendirmesine yönelik herhangi bir Batch kotası ayrılmamış olabilir. Daha fazla bilgi için [buraya](https://azure.microsoft.com/blog/update-2-on-microsoft-cloud-services-continuity/) bakın. İşleri başarıyla çalıştırmak için Batch hesabına ayrılacak VM 'Leri istemeniz gerekir.

## <a name="install-issues"></a>Yüklemeleri sorunları

  > [!NOTE]
  > Yüklemeyi bir hata nedeniyle yeniden başlatıyorsunuz, yüklemeyi yeniden tetiklemeden önce **kaynak** grubunu silmek veya kaynak grubundaki tüm kaynakları silmek emin olun.

### <a name="invalid-sentinel-credentials"></a>Geçersiz Sentinel kimlik bilgileri

Yüklemesi sırasında belirtilen Sentinel kimlik bilgileri yanlış. Yüklemeyi doğru kimlik bilgileriyle yeniden başlatın.

### <a name="the-regional-account-quota-of-batch-accounts-for-the-specified-subscription-has-been-reached"></a>Belirtilen aboneliğe yönelik Batch hesaplarının bölgesel hesap kotasına ulaşıldı

Kotayı artırın veya kullanılmayan Batch hesaplarını silin ve yüklemeyi yeniden başlatın.

### <a name="invalid-resource-group-location"></a>Geçersiz kaynak grubu konumu

**Kaynak grubunun** , yükleme sırasında belirtilen **bölgeyle** aynı konumda olduğundan emin olun.

### <a name="other-install-issues"></a>Diğer yüklemeleri sorunları

Aşağıdaki ayrıntılarla bizimle iletişim kurun:

- Abonelik KIMLIĞINIZ
- Kaynak grubu adı
- Dağıtım hatasına ait günlük dosyasını eklemek için aşağıdaki adımları izleyin:

    1. Azure portal **kaynak grubuna** gidin.

    2. Sol taraftaki **Ayarlar** bölümünde **dağıtımlar** ' ı seçin.

    3. **Başarısız**' ı gösteren her dağıtım için, ayrıntıları seçin ve dağıtım ayrıntılarını indirin. Bu dosyayı e-postaya ekleyin.

## <a name="sensor-telemetry"></a>Algılayıcı telemetrisi

### <a name="cant-view-telemetry-data"></a>Telemetri verileri görüntülenemiyor

**Belirti**: cihazlar veya algılayıcılar dağıtılır ve cihaz iş ortağınızla birlikte farmtilerini bağladınız, ancak bu verileri farmsts 'de alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**

1. Farmtts kaynak grubunuza gidin.
2. **Olay Hub 'ı** ad alanını ("algılayıcı-iş ortağı-Eh-Namespace-xxxx") seçin, "Event Hubs" düğmesine tıklayın ve sonra iş ortağına atanan olay hub 'ında gelen ileti sayısını denetleyin
3. Aşağıdakilerden birini yapın:

   - *Gelen ileti*yoksa cihaz iş ortağınızla iletişim kurun.  
   - *Gelen iletiler*varsa, veri hub 'ınız ve Hızlandırıcı günlükleriniz ve yakalanan telemetri ile bizimle iletişim kurun.

Günlüklerin nasıl indirileceği anlamak için ["günlükleri El Ile topla"](#collect-logs-manually) bölümüne gidin.  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerden geçmiş/akış verileri alındıktan sonra telemetri verileri görüntülenemiyor

**Belirti**: cihazlar veya algılayıcılar dağıtılır ve bu cihaz/sensörler ve EventHub üzerinde telemetri/algılayıcılar oluşturdunuz, ancak bu verileri, farmınts üzerinde telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**

1. İş ortağı kaydını doğru bir şekilde gerçekleştirdiğinizden emin olun. bunu, veri hub 'ının Swagger 'ınızla gidip/partner API 'sine giderek bir get yapın ve ortağın kayıtlı olup olmadığını kontrol edebilirsiniz. Aksi takdirde, iş ortağı eklemek için aşağıdaki [adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.

2. Doğru telemetri ileti biçimini kullandığınızdan emin olun:

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
          "<sensor measure name (as defined in the Sensor Model)>":<value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

### <a name="dont-have-the-azure-event-hubs-connection-string"></a>Azure Event Hubs bağlantı dizesine sahip değil

**Düzeltici eylem**

1. Datahub Swagger ' de Iş ortağı API 'sine gidin.
2. **Al**  >  **deneyin**  >  **Çalıştır**' ı seçin.

> [!NOTE]
> İlgilendiğiniz algılayıcı ortağının iş ortağı KIMLIĞI.

3. Iş ortağı API 'sine dönün ve **Al/ \<ID> Al**' ı seçin.
4. 3. adımdaki iş ortağı KIMLIĞINI belirtip **Yürüt**' ü seçin.

   API yanıtının Event Hubs bağlantı dizesi olmalıdır.

### <a name="device-appears-offline"></a>Cihaz çevrimdışı görünüyor

**Belirtiler**: cihazlar yüklenir ve Farmtts 'yi cihaz iş ortağınızla bağladınız. Cihazlar çevrimiçi ve telemetri verileri gönderiyor, ancak çevrimdışı görünüyor.

**Düzeltici eylem** Raporlama aralığı bu cihaz için yapılandırılmadı. Raporlama aralığını ayarlamak için cihaz üreticinize başvurun. 

### <a name="error-deleting-a-device"></a>Cihaz silinirken hata oluştu

Bir cihazı silerken, aşağıdaki genel hata senaryolarından biriyle karşılaşabilirsiniz:  

**İleti**: "cihaz sensörlerle başvuruluyor: cihazla ilişkili bir veya daha fazla algılayıcı var. Algılayıcıları silip cihazı silin. "  

**Anlamı**: cihaz, grupta dağıtılan birden çok sensörle ilişkilendirilir.

**Düzeltici eylem**  

1. Hızlandırıcı aracılığıyla cihazla ilişkili algılayıcıları silin.  
2. Algılayıcıları farklı bir cihazla ilişkilendirmek istiyorsanız, cihaz iş ortağınızdan aynı olduğunu sorun.  
3. Bir çağrı kullanarak cihazı silin `DELETE API` ve zorla parametresini *doğru*olarak ayarlayın.  

**İleti**: "cihaz cihazlarda parentdeviceıd olarak başvuruluyor: alt cihazlar olarak bu cihazla ilişkili bir veya daha fazla cihaz var. Bunları silin ve sonra bu cihazı silin. "  

**Anlamı**: cihazınız ile ilişkili başka cihazlar vardır.  

**Düzeltici eylem**

1. Bu belirli cihazla ilişkili cihazları silin.  
2. Belirli bir cihazı silin.  

    > [!NOTE]
    > Sensörlerle ilişkiliyse bir cihazı silemezsiniz. İlişkili algılayıcıları silme hakkında daha fazla bilgi için, [algılayıcı iş ortaklarından algılayıcı verilerini alma](get-sensor-data-from-sensor-partner.md)konusunun **algılayıcı silme** bölümüne bakın.
    > İş ortaklarının bir cihazı veya algılayıcıyı silme izni yok. Yalnızca yöneticilerin silme izni vardır.

## <a name="issues-with-jobs"></a>İşlerle ilgili sorunlar

### <a name="farmbeats-internal-error"></a>Farmtts iç hatası

**İleti**: "diğer ayrıntılar için sorun giderme kılavuzu ' na bakın.

**Düzeltici eylem** Bu sorun, veri ardışık düzeninde geçici bir hatadan kaynaklanabilir. İşi tekrar oluşturun. Hata devam ederse, hata iletisi/günlükleri ile bizimle iletişim kurun.

## <a name="accelerator-troubleshooting"></a>Hızlandırıcı sorunlarını giderme

### <a name="access-control"></a>Erişim denetimi

**Sorun**: bir rol ataması eklerken bir hata alıyorsunuz.

**İleti**: "eşleşen kullanıcı bulunamadı."

**Düzeltici eylem** Rol ataması eklemeye çalıştığınız e-posta KIMLIĞINI denetleyin. E-posta KIMLIĞI, Active Directory bu kullanıcı için kayıtlı olan KIMLIğIN tam eşleşmesi olmalıdır. Hata devam ederse, hata iletisi/günlükleri ile bizimle iletişim kurun.

### <a name="unable-to-log-in-to-accelerator"></a>Hızlandırıcıda oturum açılamıyor

**İleti**: "hata: hizmeti çağırma yetkiniz yok. Yetkilendirme için yöneticiye başvurun. "

**Düzeltici eylem** Yöneticinin Farmtts dağıtımına erişim yetkisini vermesini isteyin. Bu işlem, Roleatama API 'lerinin bir SONRASı veya hızlandırıcı 'daki **Ayarlar** bölmesinde Access Control aracılığıyla yapılabilir.  

Zaten erişim verdiyseniz ve bu hataya sahipseniz, sayfayı yenileyerek yeniden deneyin. Hata devam ederse, hata iletisi/günlükleri ile bizimle iletişim kurun.

![Proje Farmtları](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Hızlandırıcı sorunları  

**Sorun**: belirlenemeyen bir neden Hızlandırıcı hatası aldınız.

**İleti**: "hata: bilinmeyen bir hata oluştu."

**Düzeltici eylem** Bu hata, sayfayı çok uzun süre boşta bıraktığınızda oluşur. Sayfayı yenileyin. Hata devam ederse, hata iletisi/günlükleri ile bizimle iletişim kurun.

**Sorun**: FarmBeatsDeployment 'ı yükselttikten sonra bile, farmtts Hızlandırıcısı en son sürümü gösterilmiyor.

**Düzeltici eylem** Bu hata, tarayıcıda hizmet çalışanı kalıcılığı nedeniyle oluşur. Şunları yapın:

1. Hızlandırıcı açık olan tüm tarayıcı sekmelerini kapatın ve tarayıcı penceresini kapatın.
2. Tarayıcının yeni bir örneğini başlatın ve Hızlandırıcı URI 'sini yeniden yükleyin. Bu eylem, hızlandırıcının yeni sürümünü yükler.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: Imagery ile ilgili sorunlar

### <a name="wrong-username-or-password"></a>Yanlış Kullanıcı adı veya parola

**İş hatası iletisi**: "Bu kaynağa erişmek için tam kimlik doğrulaması gerekiyor."

**Düzeltici eylem**: aşağıdakilerden birini yapın:

- Aşağıdaki adımları kullanarak Farmttları doğru Kullanıcı adı/parolayla güncelleştirin ve işi yeniden deneyin.

  **Sentinel Kullanıcı adını Güncelleştir**

    1. [Azure Portal](https://portal.azure.com)oturum açın.
    2. **Arama** kutusunda, Farmrets Datahub kaynak grubunu arayın.
    3. Depolama hesabı depolamayı seçin * * * * * > **kapsayıcılar**  >  **Batch-Prep-dosyalar**  >  **to_vm**  >  **config.ini**
    4. **Düzenle** 'yi seçin
    5. Sentinel_account bölümündeki Kullanıcı adını güncelleştirin

  **Sentinel parolasını Güncelleştir**

    1. [Azure Portal](https://portal.azure.com)oturum açın.
    2. **Arama** kutusunda, Farmrets Datahub kaynak grubunu arayın.
    3. Keykasasını seçin-* * * * *
    4. Ayarlar altında erişim Ilkeleri ' ni seçin
    5. **Erişim Ilkesi Ekle** ' yi seçin
    6. Şablondan yapılandırma için **gizli dizi yönetimini** kullanın ve kendinizi sorumluyla ekleyin
    7. **Ekle**' yi seçin ve ardından **erişim ilkeleri** sayfasında **Kaydet** ' i seçin.
    8. **Ayarlar** altındaki **gizli** dizileri seçin
    9. **Sentinel-Password** seçeneğini belirleyin
    10. Değerin yeni bir sürümünü oluşturun ve etkinleştirin.

- Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Sentinel Hub: yanlış URL veya site erişilebilir değil

**İş hatası iletisi**: "bir sorun oluştu. Erişmeye çalıştığınız sayfa (geçici olarak) kullanılamıyor. "

**Düzeltici eylem**:

1. Web sitesinin erişilebilir olup olmadığını görmek için tarayıcınızda [Sentinel](https://scihub.copernicus.eu/dhus/) 'i açın.
2. Web sitesi erişilebilir değilse, herhangi bir güvenlik duvarının, şirket ağının veya diğer engelleyici yazılımların Web sitesine erişimi engelleyip engellemediğini denetleyin ve sonra Sentinel URL 'sine izin vermek için gerekli adımları uygulayın. 
3. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel sunucu: bakım için aşağı

**İş hatası iletisi**: "Copernicus açık erişim merkezi yakında yeniden sunulacaktır! Ne yazık ki bu sırada bazı bakımda bakım yapıyoruz. Kısa süre içinde yeniden çevrimiçi olacak! " 

**Düzeltici eylem**:

Bu sorun, Sentinel sunucusunda herhangi bir bakım etkinliği yapıldığında meydana gelebilir.

1. Bakım gerçekleştirildiğinden herhangi bir iş veya işlem hattı başarısız olursa, işi bir süre sonra yeniden gönderin. 

   Planlı veya planlanmamış Sentinel bakım etkinlikleri hakkında daha fazla bilgi için [Copernicus Open Access hub Haberler](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: en fazla bağlantı sayısına ulaşıldı

**İş hatası iletisi**: "' ' kullanıcısı tarafından en fazla iki eş zamanlı akış elde edilir \<username> ."

**Anlamı**: en fazla bağlantı sayısına ulaşıldığından bir iş başarısız olursa, birden fazla Işte aynı Sentinel hesabı kullanılıyor.

**Düzeltici eylem**: aşağıdakilerden birini deneyin:

* Başarısız işi yeniden çalıştırmadan önce diğer işlerin bitmesini bekleyin.
* Yeni bir Sentinel hesabı oluşturun ve sonra, Farmcts 'de Sentinel Kullanıcı adı ve parolasını güncelleştirin.

### <a name="sentinel-server-refused-connection"></a>Sentinel sunucu: bağlantı reddedildi

**İş hatası iletisi**: "sunucu bağlantıyı reddetti: http://172.30.175.69:8983/solr/dhus ."

**Düzeltici eylem**: Bu sorun, Sentinel sunucusunda herhangi bir bakım etkinliği yapıldığında meydana gelebilir.

1. Bakım gerçekleştirildiğinden herhangi bir iş veya işlem hattı başarısız olursa, işi bir süre sonra yeniden gönderin.

   Planlı veya planlanmamış Sentinel bakım etkinlikleri hakkında daha fazla bilgi için [Copernicus Open Access hub Haberler](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını denetleyin.

### <a name="soil-moisture-map-has-white-areas"></a>SOIL Moisture haritasında beyaz alan vardır

**Sorun**: **SOIL Moisture eşlemesi** oluşturuldu, ancak haritada çoğunlukla beyaz alan vardır.

**Düzeltici eylem**: Bu sorun, eşlemenin istendiği süre için üretilen uydu dizinlerinizin 0,3 ' den küçük olan ndvı değerleri varsa meydana gelebilir. Daha fazla bilgi için [Sentinel adresinden teknik kılavuzu](https://earth.esa.int/web/sentinel/technical-guides/sentinel-2-msi/level-2a/algorithm)ziyaret edin.

1. İşi farklı bir tarih aralığı için yeniden çalıştırın ve uydu dizininizdeki NDVı değerlerinin 0,3 'den fazla olup olmadığını denetleyin.

## <a name="collect-logs-manually"></a>Günlükleri el ile topla

[Azure Depolama Gezgini yükleyip dağıtın]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

### <a name="collect-azure-data-factory-job-logs-or-app-service-logs-in-datahub"></a>Veri hub 'ında Azure Data Factory iş günlüklerini veya App Service günlüklerini toplayın

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmrets Datahub kaynak grubunu arayın.
3. **Kaynak grubu** panosunda, *datahublogs \* * depolama hesabı ' nı arayın. Örneğin, *datahublogsmvxmq*.  
4. **Ad** sütununda **, depolama hesabı panosunu görüntülemek** için depolama hesabını seçin.
5. **Açık Azure Depolama Gezgini** uygulamasını görüntülemek için **datahubbloglar \* ** bölmesinde **Explorer 'da aç** ' ı seçin.
6. Sol bölmede, **BLOB kapsayıcıları**' nı seçin ve ardından Azure Data Factory günlükleri için **iş günlükleri** ' ni veya App Service Günlükler için **appınsights günlüklerini** seçin.
7. **Yükle** ' yi seçin ve günlükleri makinenizde yerel bir klasöre indirin.

    ![Proje Farmtları](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-or-app-service-logs-for-accelerator"></a>Hızlandırıcı için Azure Data Factory iş günlüklerini veya App Service günlüklerini toplayın

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmtempts Hızlandırıcı kaynak grubunu arayın.
3. **Kaynak grubu** panosunda, *depolama \* * depolama hesabı ' nı arayın. Örneğin, *storagedop4k \* *.
4. **Depolama hesabı** panosunu görüntülemek için **ad** sütunundaki depolama hesabını seçin.
5. **Depolama \* ** bölmesinde, Azure Depolama Gezgini uygulamasını açmak Için **Explorer 'da aç** ' ı seçin.
6. Sol bölmede, **BLOB kapsayıcıları**' nı seçin ve ardından Azure Data Factory günlükleri için **iş günlükleri** ' ni veya App Service Günlükler için **appınsights günlüklerini** seçin.
7. **Yükle** ' yi seçin ve günlükleri makinenizde yerel bir klasöre indirin.

## <a name="high-cpu-usage"></a>Yüksek CPU kullanımı

**Hata**: **yüksek CPU kullanımı uyarısına**başvuran bir e-posta uyarısı alırsınız.

**Düzeltici eylem**:

1. Farmrets Datahub kaynak grubuna gidin.
2. **App Service**'i seçin.  
3. Ölçek [App Service fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/app-service/windows/)gidin ve uygun bir fiyatlandırma katmanını seçin.

## <a name="weather-data-job-failures"></a>Hava durumu verileri işi sorunları

**Hata**: Hava durumu verilerini almak için işleri çalıştıralım, ancak iş başarısız

### <a name="collect-logs-to-troubleshoot-weather-data-job-failures"></a>Hava durumu verileri iş hatalarında sorun gidermek için günlükleri toplayın

1. Azure portal içindeki Farmtempts kaynak grubuna gidin.
2. Kaynak grubunun parçası olan Data Factory hizmetine tıklayın. Hizmetin "SKU: Datahub" etiketi olacaktır

> [!NOTE]
> Kaynak grubu içindeki hizmetlerin etiketlerini görüntülemek için, "Sütunları Düzenle" seçeneğine tıklayın ve kaynak grubu görünümüne "Etiketler" ekleyin

:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-1.png" alt-text="Proje Farmtları":::

3. Veri fabrikasının Genel Bakış sayfasında **Yazar ve izleyici**' ye tıklayın. Tarayıcınızda yeni bir sekme açılır. **Monitöre** tıklayın

:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-2.png" alt-text="Proje Farmtları":::

4. Hava durumu işi yürütmesinin parçası olan işlem hattı çalıştırmalarının bir listesini görürsünüz. Günlüklerini toplamak istediğiniz Işe tıklayın
 
:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-3.png" alt-text="Proje Farmtları":::

5. İşlem hattı Genel Bakış sayfasında, etkinlik çalıştırmalarının listesini görürsünüz. Günlükleri toplamak istediğiniz etkinliklerin çalıştırma kimliklerini bir yere unutmayın
 
:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-4.png" alt-text="Proje Farmtları":::

6. Azure portal ' de Farmrekts kaynak grubunuza dönün ve **datahublogs-xxxx** adlı depolama hesabına tıklayın
 
:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-5.png" alt-text="Proje Farmtları":::

7. **Kapsayıcılar**  ->  **adfjobs**' a tıklayın. Arama kutusuna, yukarıdaki 5. adımda not ettiğiniz iş çalıştırma KIMLIĞINI girin.
 
:::image type="content" source="./media/troubleshoot-Azure-farmbeats/weather-log-6.png" alt-text="Proje Farmtları":::

8. Arama sonucu, işle ilgili günlüklere sahip olan klasörü içerir. Sorunu gidermek için günlükleri indirin ve yardım almak üzere gönderin farmbeatssupport@microsoft.com .
