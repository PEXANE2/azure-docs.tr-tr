---
title: Azure FarmBeats sorunlarını giderme
description: Bu makalede, Azure Farmtts ile ilgili sorunların nasıl giderileceği açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 20d07be99aa2f9881218f8d581ac8d429a1fe4d0
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79298810"
---
# <a name="troubleshoot"></a>Sorun giderme

Bu makalede, yaygın Azure Farmtts sorunlarına yönelik çözümler sağlanmaktadır.

Ek Yardım için farmbeatssupport@microsoft.comadresinden bizimle iletişim kurun. **Deployer. log** dosyasını e-postanıza dahil etmek için emin olun.

**Deployer. log** dosyasını indirmek için aşağıdakileri yapın:

1. **Azure Portal** için oturum açın ve aboneliğinizi ve Azure AD kiracınızı seçin.
2. Cloud Shell’i Azure portalında en üst gezinti bölmesinden başlatın.
3. Tercih edilen Cloud Shell deneyimi olarak **Bash** ' i seçin.
4. Vurgulanan simgeyi seçin ve ardından açılan listede **İndir**' i seçin.

    ![Proje Farmtları](./media/troubleshoot-azure-farmbeats/download-deployer-log-1.png)

5. Sonraki bölmede, **Deployer. log** dosyanızın yolunu girin. Örneğin, **farmbeats-Deployer. log**yazın.

## <a name="sensor-telemetry"></a>Algılayıcı telemetrisi

### <a name="cant-view-telemetry-data"></a>Telemetri verileri görüntülenemiyor

**Belirti**: cihazlar veya algılayıcılar dağıtılır ve cihaz iş ortağınızla birlikte farmtilerini bağladınız, ancak bu verileri farmsts 'de alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. Farmrets Datahub kaynak grubuna gidin.   
2. **Olay Hub 'ını** (datafeedeventhubnamespace) seçin ve ardından gelen ileti sayısını kontrol edin.
3. Aşağıdakilerden birini yapın:   
   - *Gelen ileti*yoksa cihaz iş ortağınızla iletişim kurun.  
   - *Gelen iletiler*varsa farmbeatssupport@microsoft.combaşvurun. Veri hub 'ınızı ve Hızlandırıcı günlüklerinizi ve yakalanan Telemetriyi ekleyin.

Günlüklerin nasıl indirileceği anlamak için ["günlükleri El Ile topla"](#collect-logs-manually) bölümüne gidin.  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerden geçmiş/akış verileri alındıktan sonra telemetri verileri görüntülenemiyor

**Belirti**: cihazlar veya algılayıcılar dağıtılır ve bu cihaz/sensörler ve EventHub üzerinde telemetri/algılayıcılar oluşturdunuz, ancak bu verileri, farmınts üzerinde telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. İş ortağı kaydını doğru bir şekilde gerçekleştirdiğinizden emin olun. bunu, veri hub 'ının Swagger 'ınızla gidip/partner API 'sine giderek bir get yapın ve ortağın kayıtlı olup olmadığını kontrol edebilirsiniz. Aksi takdirde, iş ortağı eklemek için [buradaki adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.
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
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
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

### <a name="dont-have-the-azure-event-hubs-connection-string"></a>Azure Event Hubs bağlantı dizesine sahip değil

**Düzeltici eylem**:

1. Datahub Swagger ' de Iş ortağı API 'sine gidin.
2. **Al** ** >  > ** **Yürüt**' ü seçin.
3. İlgilendiğiniz algılayıcı ortağının iş ortağı KIMLIĞINE göz önünde olun.
4. Iş ortağı API 'sine dönün ve **Al/\<kimliği >** ' ni seçin.
5. 3\. adımdaki iş ortağı KIMLIĞINI belirtip **Yürüt**' ü seçin.

   API yanıtının Event Hubs bağlantı dizesi olmalıdır.

### <a name="device-appears-offline"></a>Cihaz çevrimdışı görünüyor

**Belirtiler**: cihazlar yüklenir ve Farmtts 'yi cihaz iş ortağınızla bağladınız. Cihazlar çevrimiçi ve telemetri verileri gönderiyor, ancak çevrimdışı görünüyor.

**Düzeltici eylem**: Bu cihaz için Raporlama aralığı yapılandırılmadı. Raporlama aralığını ayarlamak için cihaz üreticinize başvurun. 

### <a name="error-deleting-a-device"></a>Cihaz silinirken hata oluştu

Bir cihazı silerken, aşağıdaki genel hata senaryolarından biriyle karşılaşabilirsiniz:  

**İleti**: "cihaz sensörlerle başvuruluyor: cihazla ilişkili bir veya daha fazla algılayıcı var. Algılayıcıları silip cihazı silin. "  

**Anlamı**: cihaz, grupta dağıtılan birden çok sensörle ilişkilendirilir.   

**Düzeltici eylem**:  

1. Hızlandırıcı aracılığıyla cihazla ilişkili algılayıcıları silin.  
2. Algılayıcıları farklı bir cihazla ilişkilendirmek istiyorsanız, cihaz iş ortağınızdan aynı olduğunu sorun.  
3. `DELETE API` çağrısı kullanarak cihazı silin ve zorla parametresini *doğru*olarak ayarlayın.  

**İleti**: "cihaz cihazlarda parentdeviceıd olarak başvuruluyor: alt cihazlar olarak bu cihazla ilişkili bir veya daha fazla cihaz var. Bunları silin ve sonra bu cihazı silin. "  

**Anlamı**: cihazınız ile ilişkili başka cihazlar vardır.  

**Düzeltici eylem**

1. Bu belirli cihazla ilişkili cihazları silin.  
2. Belirli bir cihazı silin.  

    > [!NOTE]
    > Sensörlerle ilişkiliyse bir cihazı silemezsiniz. İlişkili algılayıcıları silme hakkında daha fazla bilgi için, [algılayıcı iş ortaklarından algılayıcı verilerini alma](get-sensor-data-from-sensor-partner.md)konusunun **algılayıcı silme** bölümüne bakın.


## <a name="issues-with-jobs"></a>İşlerle ilgili sorunlar

### <a name="farmbeats-internal-error"></a>Farmtts iç hatası

**İleti**: "diğer ayrıntılar için sorun giderme kılavuzu" bölümüne bakın.

**Düzeltici eylem**: Bu sorun, veri ardışık düzeninde geçici bir hatadan kaynaklanabilir. İşi tekrar oluşturun. Hata devam ederse, Farmtts forumundaki bir gönderiye hata iletisi ekleyin veya FarmBeatsSupport@microsoft.combaşvurun.

## <a name="accelerator-troubleshooting"></a>Hızlandırıcı sorunlarını giderme

### <a name="access-control"></a>Erişim denetimi

**Sorun**: bir rol ataması eklerken bir hata alıyorsunuz.

**İleti**: "eşleşen kullanıcı bulunamadı."

**Düzeltici eylem**: rol ataması eklemeye çalıştığınız e-posta kimliğini denetleyin. E-posta KIMLIĞI, Active Directory bu kullanıcı için kayıtlı olan KIMLIğIN tam eşleşmesi olmalıdır. Hata devam ederse, Farmtts forumundaki bir gönderiye hata iletisi ekleyin veya FarmBeatsSupport@microsoft.combaşvurun.

### <a name="unable-to-log-in-to-accelerator"></a>Hızlandırıcıda oturum açılamıyor

**İleti**: "hata: hizmeti çağırma yetkiniz yok. Yetkilendirme için yöneticiye başvurun. "

**Düzeltici eylem**: yöneticinin Farmtts dağıtımına erişim yetkisini vermesini isteyin. Bu işlem, Roleatama API 'lerinin bir SONRASı veya hızlandırıcı 'daki **Ayarlar** bölmesinde Access Control aracılığıyla yapılabilir.  

Zaten erişim verdiyseniz ve bu hatayla karşılaşırsanız, sayfayı yenileyerek tekrar deneyin. Hata devam ederse, Farmtts forumundaki bir gönderiye hata iletisi ekleyin veya FarmBeatsSupport@microsoft.combaşvurun.

![Proje Farmtları](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Hızlandırıcı sorunları  

**Sorun**: belirlenemeyen bir neden Hızlandırıcı hatası aldınız.

**İleti**: "hata: bilinmeyen bir hata oluştu."

**Düzeltici eylem**: sayfayı çok uzun süre boşta bıraktığınızda bu hata oluşur. Sayfayı yenileyin.  

Hata devam ederse, Farmtts forumundaki bir gönderiye hata iletisi ekleyin veya FarmBeatsSupport@microsoft.combaşvurun.

**Sorun**: FarmBeatsDeployment 'ı yükselttikten sonra bile, farmtts Hızlandırıcısı en son sürümü gösterilmiyor.

**Düzeltici eylem**: Bu hata, tarayıcıda hizmet çalışanı kalıcılığı nedeniyle oluşur. Şunları yapın:

1. Hızlandırıcı açık olan tüm tarayıcı sekmelerini kapatın ve tarayıcı penceresini kapatın.
2. Tarayıcının yeni bir örneğini başlatın ve Hızlandırıcı URI 'sini yeniden yükleyin. Bu eylem, hızlandırıcının yeni sürümünü yükler.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: Imagery ile ilgili sorunlar

### <a name="wrong-username-or-password"></a>Yanlış Kullanıcı adı veya parola

**İş hatası iletisi**: "Bu kaynağa erişmek için tam kimlik doğrulaması gerekiyor."

**Düzeltici eylem**:

Aşağıdakilerden birini yapın:

- Veri hub 'ını doğru Kullanıcı adı ve parolayla yükseltmek için yükleyiciyi yeniden çalıştırın.
- Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Sentinel Hub: yanlış URL veya site erişilebilir değil 

**İş hatası iletisi**: "bir sorun oluştu. Erişmeye çalıştığınız sayfa (geçici olarak) kullanılamıyor. " 

**Düzeltici eylem**:
1. Web sitesinin erişilebilir olup olmadığını görmek için tarayıcınızda [Sentinel](https://scihub.copernicus.eu/dhus/) 'i açın. 
2. Web sitesi erişilebilir değilse, herhangi bir güvenlik duvarının, şirket ağının veya diğer engelleyici yazılımların Web sitesine erişimi engelleyip engellemediğini denetleyin ve sonra Sentinel URL 'sine izin vermek için gerekli adımları uygulayın. 
3. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel sunucu: bakım için aşağı

**İş hatası iletisi**: "Copernicus açık erişim merkezi yakında yeniden sunulacaktır! Ne yazık ki bu sırada bazı bakımda bakım yapıyoruz. Kısa süre içinde yeniden çevrimiçi olacak! " 

**Düzeltici eylem**:

Bu sorun, Sentinel sunucusunda herhangi bir bakım etkinliği yapıldığında meydana gelebilir.

1. Bakım gerçekleştirildiğinden herhangi bir iş veya işlem hattı başarısız olursa, işi bir süre sonra yeniden gönderin. 

   Planlı veya planlanmamış Sentinel bakım etkinlikleri hakkında daha fazla bilgi için [Copernicus Open Access hub Haberler](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: en fazla bağlantı sayısına ulaşıldı

**İş hatası iletisi**: "Kullanıcı tarafından '\<kullanıcı adı > ' Için en fazla iki eş zamanlı akış elde edilir."

**Anlamı**: en fazla bağlantı sayısına ulaşıldığından bir iş başarısız olursa, başka bir yazılım dağıtımında aynı Sentinel hesabı kullanılıyor.

**Düzeltici eylem**: aşağıdakilerden birini deneyin:

* Yeni bir Sentinel hesabı oluşturun ve ardından yeni bir Sentinel Kullanıcı adı ve parola kullanarak veri merkezini yükseltmek için yükleyiciyi yeniden çalıştırın.  
* Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

### <a name="sentinel-server-refused-connection"></a>Sentinel sunucu: bağlantı reddedildi 

**İş hatası iletisi**: "sunucu bağlantıyı reddetti: http://172.30.175.69:8983/solr/dhus." 

**Düzeltici eylem**: Bu sorun, Sentinel sunucusunda herhangi bir bakım etkinliği yapıldığında meydana gelebilir. 
1. Bakım gerçekleştirildiğinden herhangi bir iş veya işlem hattı başarısız olursa, işi bir süre sonra yeniden gönderin. 

   Planlı veya planlanmamış Sentinel bakım etkinlikleri hakkında daha fazla bilgi için [Copernicus Open Access hub Haberler](https://scihub.copernicus.eu/news/) sitesine gidin.  

2. Başarısız işi yeniden çalıştırın veya 5 ile 7 gün arasında bir tarih aralığı için uydu dizinleri işi çalıştırın ve ardından işin başarılı olup olmadığını kontrol edin.

## <a name="collect-logs-manually"></a>Günlükleri el ile topla

[Azure Depolama Gezgini yükleyip dağıtın]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

### <a name="collect-azure-data-factory-job-logs-in-datahub"></a>Veri hub 'ında Azure Data Factory iş günlüklerini toplayın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmrets Datahub kaynak grubunu arayın.

    > [!NOTE]
    > Farmrets yüklemesi sırasında belirttiğiniz Datahub kaynak grubunu seçin.

3. **Kaynak grubu** panosunda, *datahublogs\** depolama hesabı ' nı arayın. Örneğin, **datahublogsmvxmq**araması yapın.  
4. **Ad** sütununda **, depolama hesabı panosunu görüntülemek** için depolama hesabını seçin.
5. **Açık Azure Depolama Gezgini** uygulamasını görüntülemek için **datahubbloglar\*** bölmesinde **Gezgin 'de aç** ' ı seçin.
6. Sol bölmede, **BLOB kapsayıcıları**' nı seçin ve ardından **iş-Günlükler**' i seçin.
7. **İş günlükleri** bölmesinde **İndir**' i seçin.
8. Günlükleri makinenizde yerel bir klasöre indirin.
9. İndirilen. zip dosyasını farmbeatssupport@microsoft.come-posta ile gönderin.

    ![Proje Farmtları](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-in-accelerator"></a>Hızlandırıcıda Azure Data Factory iş günlüklerini toplayın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmtempts Hızlandırıcı kaynak grubunu arayın.

    > [!NOTE]
    > Farmtempts yüklemesi sırasında belirttiğiniz Hızlandırıcı kaynak grubunu seçin.

3. **Kaynak grubu** panosunda *depolama\** depolama hesabı ' nı arayın. Örneğin, **storagedop4k\*** için arama yapın.
4. **Depolama hesabı** panosunu görüntülemek için **ad** sütunundaki depolama hesabını seçin.
5. Azure Depolama Gezgini uygulamasını açmak için **depolama\*** bölmesinde, **Explorer 'da aç** ' ı seçin.
6. Sol bölmede, **BLOB kapsayıcıları**' nı seçin ve ardından **iş-Günlükler**' i seçin.
7. **İş günlükleri** bölmesinde **İndir**' i seçin.
8. Günlükleri makinenizde yerel bir klasöre indirin.
9. İndirilen. zip dosyasını farmbeatssupport@microsoft.come-posta ile gönderin.


### <a name="collect-datahub-app-service-logs"></a>Veri hub 'ı App Service günlüklerini topla

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmrets Datahub kaynak grubunu arayın.

    > [!NOTE]
    > Farmrets yüklemesi sırasında belirttiğiniz Datahub kaynak grubunu seçin.

3. Kaynak grubunda, *datahublogs\** depolama hesabı ' nı arayın. Örneğin, **fordatahublogsmvxmq\*** aratın.
4. **Depolama hesabı** panosunu görüntülemek için **ad** sütunundaki depolama hesabını seçin.
5. **Datahubbloglar\*** bölmesinde, Azure Depolama Gezgini uygulamasını açmak için **Gezgin 'de aç** ' ı seçin.
6. Sol bölmede **BLOB kapsayıcıları**' nı seçin ve **appınsights-logs**' u seçin.
7. **Appınsights-logs** bölmesinde **İndir**' i seçin.
8. Günlükleri makinenizde yerel bir klasöre indirin.
9. İndirilen. zip dosyasını farmbeatssupport@microsoft.come-posta ile gönderin.

### <a name="collect-accelerator-app-service-logs"></a>Hızlandırıcı App Service günlüklerini toplayın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Arama** kutusunda, Farmtempts Hızlandırıcı kaynak grubunu arayın.

    > [!NOTE]
    > Farmtts yüklemesi sırasında sağlanmış olan Farmtempts Hızlandırıcı kaynak grubunu seçin.

3. Kaynak grubunda *depolama\** depolama hesabı ' nı arayın. Örneğin, **storagedop4k\*** için arama yapın.
4. **Depolama hesabı** panosunu görüntülemek için **ad** sütunundaki depolama hesabını seçin.
5. Azure Depolama Gezgini uygulamasını açmak için **depolama\*** bölmesinde, **Explorer 'da aç** ' ı seçin.
6. Sol bölmede **BLOB kapsayıcıları**' nı seçin ve **appınsights-logs**' u seçin.
7. **Appınsights-logs** bölmesinde **İndir**' i seçin.
8. Günlükleri makinenizde yerel bir klasöre indirin.
9. İndirilen klasörü farmbeatssupport@microsoft.come-posta ile gönderin.

## <a name="known-issues"></a>Bilinen sorunlar

## <a name="batch-related-issues"></a>Batch ile ilgili sorunlar

**Hata iletisi**: "belirtilen abonelik Için Batch hesaplarının bölgesel hesap kotasına ulaşıldı."

**Düzeltici eylem**: kotayı artırın veya kullanılmayan Batch hesaplarını silin ve dağıtımı yeniden çalıştırın.

### <a name="azure-active-directory-azure-ad-related-issues"></a>Azure Active Directory (Azure AD) ile ilgili sorunlar

**Hata iletisi**: "gerekli ayarları güncelleştiremedi Azure AD uygulaması d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0: işlemi gerçekleştirmek için yeterli ayrıcalıklara sahip değil. Yukarıdaki ayarların Azure AD Uygulaması için düzgün yapılandırıldığından emin olun. "

**Anlamı**: Azure AD uygulama kayıt yapılandırması düzgün tamamlanmadı.  

**Düzeltici eylem**: Azure AD uygulama kaydını oluşturmak için [BETIĞIMIZI](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect/tree/master/AppCreationScripts) kullanarak BT yöneticinize (kiracı okuma erişimi olan kişi) sorun. Bu komut dosyası otomatik olarak yapılandırma adımlarını da ele alır.

**Hata iletisi**: "Bu kiracıda," uygulama adı\>'\<yeni Active Directory uygulaması oluşturulamadı: özellik tanımlayıcısı URI 'leri için aynı değere sahip başka bir nesne zaten var. "

**Anlamı**: aynı ada sahip BIR Azure AD uygulama kaydı zaten var.

**Düzeltme eylemi**: mevcut Azure AD uygulama kaydını silin veya yükleme için yeniden kullanın. Mevcut Azure AD uygulama kaydını yeniden kullanıyorsanız, uygulama KIMLIĞINI ve istemci gizli anahtarını yükleyiciye geçirin ve yeniden dağıtın.

## <a name="issues-with-the-inputjson-file"></a>İnput. JSON dosyası ile ilgili sorunlar

**Hata**: input *. JSON* dosyasından giriş okunurken bir hata oluştu.

**Düzeltici eylem**: Bu sorun, genellikle doğru *input. JSON* dosya yolunu veya yükleyicideki adı belirtirken oluşan bir hata nedeniyle ortaya çıkar. Uygun düzeltmeleri yapın ve dağıtımı yeniden deneyin.

**Hata**: *input. JSON* dosyasındaki değerler ayrıştırılırken bir hata oluştu.

**Düzeltici eylem**: Bu sorun genellikle *Input. JSON* dosyasındaki hatalı değerler nedeniyle ortaya çıkar. Uygun düzeltmeleri yapın ve dağıtımı yeniden deneyin.

## <a name="high-cpu-usage"></a>Yüksek CPU kullanımı

**Hata**: **yüksek CPU kullanımı uyarısına**başvuran bir e-posta uyarısı alırsınız. 

**Düzeltici eylem**: 
1. Farmrets Datahub kaynak grubuna gidin.
2. **App Service**'i seçin.  
3. Ölçek [App Service fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/app-service/windows/)gidin ve uygun bir fiyatlandırma katmanını seçin.

## <a name="next-steps"></a>Sonraki adımlar

Hala planlılar sorunlarınız varsa [destek forumumuza](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats)başvurun.
