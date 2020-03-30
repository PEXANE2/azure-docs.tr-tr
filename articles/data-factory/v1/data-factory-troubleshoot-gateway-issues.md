---
title: Sorun Giderme Veri Yönetimi Ağ Geçidi sorunları
description: Veri Yönetimi Ağ Geçidi ile ilgili sorunları gidermek için ipuçları sağlar.
services: data-factory
author: nabhishek
manager: anandsub
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/01/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 09d51de3ae0bd4baca585d2abdd936b1a29567d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065022"
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi kullanımıyla ilgili sorunları giderme
Bu makalede, Veri Yönetimi Ağ Geçidi'ni kullanmayla ilgili sorun giderme sorunları hakkında bilgi verilmektedir.

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nda kendi kendine barındırılan tümleştirme çalışma süresine](../create-self-hosted-integration-runtime.md)bakın.

Ağ geçidi hakkında ayrıntılı bilgi için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın. Ağ geçidini kullanarak şirket içi BIR SQL Server veritabanından Microsoft Azure Blob depolamasına veri taşıma nın bir walkthrough için [şirket içi ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşıy veri bakın.

## <a name="failed-to-install-or-register-gateway"></a>Ağ geçidini yüklemek veya kaydettirmek için başarısız olunması
### <a name="1-problem"></a>1. Sorun
Ağ geçidi yükleme dosyasını indirirken, özellikle bir ağ geçidi yüklerken ve kaydederken bu hata iletisini görürsünüz.

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Nedeni
Ağ geçidini yüklemeye çalıştığınız makine, ağ sorunu nedeniyle indirme merkezinden en son ağ geçidi yükleme dosyasını karşıdan yüklemeyi başaramadı.

#### <a name="resolution"></a>Çözüm
Ayarların bilgisayardan [indirme merkezine](https://download.microsoft.com/)ağ bağlantısını engelleyip engellemediğini görmek ve ayarları buna göre güncelleştirmek için güvenlik duvarı proxy sunucu ayarlarınızı kontrol edin.

Alternatif olarak, yükleme merkezine erişebilen diğer makinelerde [indirme merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) en son ağ geçidi için yükleme dosyasını indirebilirsiniz. Daha sonra yükleyici dosyasını ağ geçidi ana bilgisayarına kopyalayabilir ve ağ geçidini yüklemek ve güncelleştirmek için el ile çalıştırabilirsiniz.

### <a name="2-problem"></a>2. Sorun
Azure portalındaki bu **bilgisayara doğrudan yükle'yi** tıklatarak bir ağ geçidi yüklemeye çalışırken bu hatayı görürsünüz.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Nedeni
Makineye zaten bir ağ geçidi yüklenmiş.

#### <a name="resolution"></a>Çözüm
Makinedeki varolan ağ geçidini kaldırın ve **doğrudan bu bilgisayar bağlantısına** yükleyin'i yeniden tıklatın.

### <a name="3-problem"></a>3. Sorun
Yeni bir ağ geçidi kaydederken bu hatayı görebilirsiniz.

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a>Nedeni
Bu iletiyi aşağıdaki nedenlerden biri için görebilirsiniz:

* Ağ geçidi anahtarının biçimi geçersizdir.
* Ağ geçidi anahtarı geçersiz kılındı.
* Ağ geçidi anahtarı portaldan yenilendi.  

#### <a name="resolution"></a>Çözüm
Portaldan doğru ağ geçidi anahtarını kullanıp kullanmadığınızı doğrulayın. Gerekirse, bir anahtarı yeniden oluşturun ve ağ geçidini kaydetmek için anahtarı kullanın.

### <a name="4-problem"></a>4. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![Anahtarın içeriği veya biçimi geçersizdir](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Nedeni
Giriş ağ geçidi anahtarının içeriği veya biçimi yanlıştır. Bunun nedenlerinden biri, anahtarın yalnızca bir kısmını portaldan kopyalamamanız veya geçersiz bir anahtar kullanıyor olsanız.

#### <a name="resolution"></a>Çözüm
Portalda bir ağ geçidi tuşu oluşturun ve tüm anahtarı kopyalamak için kopyalama düğmesini kullanın. Ardından ağ geçidini kaydetmek için bu pencereye yapıştırın.

### <a name="5-problem"></a>5. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Ağ geçidi anahtarı geçersiz veya boş](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi anahtarı yeniden oluşturuldu veya Azure portalında ağ geçidi silindi. Veri Yönetimi Ağ Geçidi kurulumu en son değilse de gerçekleşebilir.

#### <a name="resolution"></a>Çözüm
Veri Yönetimi Ağ Geçidi kurulumunun en son sürüm olup olmadığını kontrol edin, Microsoft [indirme merkezinde](https://go.microsoft.com/fwlink/p/?LinkId=271260)en son sürümü bulabilirsiniz.

Kurulum güncel/ en son ve ağ geçidi hala Portal'da varsa, Azure portalındaki ağ geçidi anahtarını yeniden oluşturun ve tüm anahtarı kopyalamak için kopyalama düğmesini kullanın ve ağ geçidini kaydetmek için bu pencereye yapıştırın. Aksi takdirde, ağ geçidini yeniden oluşturun ve baştan başlayın.

### <a name="6-problem"></a>6. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Ağ geçidi anahtarı geçersiz veya boş](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi silindiğinden veya ilişkili ağ geçidi anahtarı yeniden oluşturulduğundan bu hata olabilir.

#### <a name="resolution"></a>Çözüm
Ağ geçidi silinmişse, portaldan ağ geçidini yeniden oluşturun, **Kaydol'u**tıklatın, anahtarı portaldan kopyalayın, yapıştırın ve ağ geçidini kaydetmeye çalışın.

Ağ geçidi hala varsa ancak anahtarı yeniden oluşturulduysa, ağ geçidini kaydetmek için yeni anahtarı kullanın. Anahtar sizde yoksa, anahtarı portaldan yeniden oluşturun.

### <a name="7-problem"></a>7. Sorun
Bir ağ geçidi kaydederken, sertifika için yol ve parola girmeniz gerekebilir.

![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi daha önce diğer makinelere kaydedilmiştir. Bir ağ geçidinin ilk kaydı sırasında, bir şifreleme sertifikası ağ geçidi ile ilişkilendirilmiştir. Sertifika ağ geçidi tarafından oluşturulan veya kullanıcı tarafından sağlanabilir.  Bu sertifika, veri deposunun (bağlantılı hizmet) kimlik bilgilerini şifrelemek için kullanılır.  

![Sertifikayı dışarı aktarma](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Kayıt sihirbazı, farklı bir ana bilgisayardaki ağ geçidini geri alırken, bu sertifikanın daha önce bu sertifikayla şifrelenmiş kimlik bilgilerinin şifresini çözmesini ister.  Bu sertifika olmadan kimlik bilgileri yeni ağ geçidi tarafından çözülemez ve bu yeni ağ geçidiyle ilişkili sonraki kopyalama etkinliği yürütmeleri başarısız olur.  

#### <a name="resolution"></a>Çözüm
Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ndeki **Ayarlar** sekmesindeki **Dışa Aktarma** düğmesini kullanarak orijinal ağ geçidi makinesinden kimlik bilgisi sertifikasını dışa aktardıysanız, sertifikayı buradan kullanın.

Bir ağ geçidini kurtarırken bu aşamayı atlayamazsınız. Sertifika eksikse, portaldan ağ geçidini silmeniz ve yeni bir ağ geçidi oluşturmanız gerekir.  Ayrıca, kimlik bilgilerini yeniden girerek ağ geçidiyle ilgili tüm bağlantılı hizmetleri güncelleştirin.

### <a name="8-problem"></a>8. Sorun
Aşağıdaki hata iletisini görebilirsiniz.

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Nedeni
Bu hata, ağ geçidiniz Internet kaynaklarına erişmek için bir HTTP proxy'si gerektiren bir ortamda olduğunda veya proxy'nizin kimlik doğrulama parolası değiştirildiğinde ancak ağ geçidinizde buna göre güncelleştirilmediğinde olur.

#### <a name="resolution"></a>Çözüm
Bu makalenin Proxy sunucusu değerlendirmeleri bölümündeki yönergeleri izleyin ve proxy ayarlarını Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi ile yapılandırın.

## <a name="gateway-is-online-with-limited-functionality"></a>Ağ Geçidi sınırlı işlevsellik ile çevrimiçi
### <a name="1-problem"></a>1. Sorun
Ağ geçidinin durumunu sınırlı işlevsellikle çevrimiçi olarak görürsünüz.

#### <a name="cause"></a>Nedeni
Ağ geçidinin durumunu aşağıdaki nedenlerden biriyle sınırlı işlevsellik ile çevrimiçi olarak görürsünüz:

* Ağ Geçidi, Azure Servis Veri Servisi aracılığıyla bulut hizmetine bağlanamaz.
* Bulut hizmeti, Hizmet Veri Servisi aracılığıyla ağ geçidine bağlanamaz.

Ağ geçidi sınırlı işlevsellikle çevrimiçi olduğunda, şirket içi veri depolarına veya şirket içi veri depolarına veri kopyalamak için veri ardışık lıkları oluşturmak için Veri Fabrikası Kopyalama Sihirbazı'nı kullanamayabilirsiniz. Geçici çözüm olarak, Portal, Visual Studio veya Azure PowerShell'de Veri Fabrikası Düzenleyicisi'ni kullanabilirsiniz.

#### <a name="resolution"></a>Çözüm
Bu sorunun çözümü (sınırlı işlevsellikle çevrimiçi) ağ geçidinin bulut hizmetine veya başka bir şekilde bağlanıp bağlanamayacağına bağlıdır. Aşağıdaki bölümlerde bu kararlar sağlar.

### <a name="2-problem"></a>2. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Gateway cannot connect to cloud service through service bus`

![Ağ geçidi bulut hizmetine bağlanamıyor](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni
Ağ Geçidi, Servis Veri Servisi aracılığıyla bulut hizmetine bağlanamaz.

#### <a name="resolution"></a>Çözüm
Ağ geçidini tekrar çevrimiçi duruma getirmek için aşağıdaki adımları izleyin:

1. Ağ geçidi makinesinde ve şirket güvenlik duvarında IP adresi giden kurallara izin verin. WINDOWS Olay Günlüğü'nden IP adreslerini bulabilirsiniz (ID == 401): Bir sokete erişim izinleri XX tarafından yasaklanmış bir şekilde erişmeye çalışılmıştır. Xx. Xx. XX:9350.
1. Ağ geçidinde proxy ayarlarını yapılandırın. Ayrıntılar için Proxy sunucusu nun göz önünde bulundurulması bölümüne bakın.
1. Ağ geçidi makinesinde ve şirket güvenlik duvarında hem Windows Güvenlik Duvarı'nda 5671 ve 9350-9354 bağlantı noktalarını etkinleştirin. Ayrıntılar için Bağlantı Noktaları ve güvenlik duvarı bölümüne bakın. Bu adım isteğe bağlıdır, ancak performans değerlendirmesi için öneririz.

### <a name="3-problem"></a>3. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a>Nedeni
Ağ bağlantısında geçici bir hata.

#### <a name="resolution"></a>Çözüm
Ağ geçidini tekrar çevrimiçi duruma getirmek için aşağıdaki adımları izleyin:

1. Birkaç dakika bekleyin, hata gittiğinde bağlantı otomatik olarak kurtarılır.
1. Hata devam ederse, ağ geçidi hizmetini yeniden başlatın.

## <a name="failed-to-author-linked-service"></a>Bağlantılı hizmeti yazarda başarısız oldu
### <a name="problem"></a>Sorun
Yeni bir bağlantılı hizmet için kimlik bilgilerini giriş yapmak veya varolan bir bağlantılı hizmetin kimlik bilgilerini güncelleştirmek için portalda Kimlik Bilgisi Yöneticisi'ni kullanmaya çalıştığınızda bu hatayı görebilirsiniz.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Bu hatayı gördüğünüzde, Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nin ayarlar sayfası aşağıdaki ekran görüntüsügibi görünebilir.

![Veritabanına ulaşılamıyor](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Nedeni
TLS/SSL sertifikası ağ geçidi makinesinde kaybolmuş olabilir. Ağ geçidi bilgisayarı, TLS şifrelemesi için kullanılan sertifikayı şu anda yükleyemez. Olay günlüğünde aşağıdaki iletiye benzer bir hata iletisi de görebilirsiniz.

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Çözüm
Sorunu çözmek için aşağıdaki adımları izleyin:

1. Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni başlatın.
2. **Ayarlar** sekmesine geçin.  
3. TLS/SSL sertifikasını değiştirmek için **Değiştir** düğmesini tıklatın.

   ![Sertifika yı değiştir düğmesi](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. TLS/SSL sertifikası olarak yeni bir sertifika seçin. Sizin veya herhangi bir kuruluş tarafından oluşturulan herhangi bir TLS/SSL sertifikasını kullanabilirsiniz.

   ![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopyalama etkinliği başarısız oldu
### <a name="problem"></a>Sorun
Portalda bir ardışık hat lar kurduktan sonra aşağıdaki "UserErrorFailedToConnectToSqlserver" hatasını fark edebilirsiniz.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a>Nedeni
Bu farklı nedenlerle olabilir ve azaltma buna göre değişir.

#### <a name="resolution"></a>Çözüm
SQL veritabanına bağlanmadan önce Veri Yönetimi Ağ Geçidi istemci tarafında TCP/1433 bağlantı noktası üzerinden giden TCP bağlantılarına izin verin.

Hedef veritabanı bir Azure SQL veritabanıysa, Azure için SQL Server güvenlik duvarı ayarlarını da kontrol edin.

Şirket içi veri deposuna bağlantıyı test etmek için aşağıdaki bölüme bakın.

## <a name="data-store-connection-or-driver-related-errors"></a>Veri deposu bağlantısı veya sürücüyle ilgili hatalar
Veri deposu bağlantısı veya sürücüyle ilgili hatalar görürseniz, aşağıdaki adımları tamamlayın:

1. Ağ geçidi makinesinde Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni başlatın.
2. **Tanılama** sekmesine geçin.
3. **Test Bağlantısı'nda**ağ geçidi grup değerlerini ekleyin.
4. Bağlantı bilgilerini ve kimlik bilgilerini kullanarak ağ geçidi makinesinden şirket içi veri kaynağına bağlanıp bağlanamayacağınızı görmek için **Test'i** tıklatın. Bir sürücü yükledikten sonra bağlantı testi yine başarısız olursa, ağ geçidini son değişiklikleri alması için yeniden başlatın.

![Tanılama sekmesinde Test Bağlantısı](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Ağ geçidi günlükleri
### <a name="send-gateway-logs-to-microsoft"></a>Ağ geçidi günlüklerini Microsoft'a gönderme
Sorun giderme ağ geçidi sorunlarıyla ilgili yardım almak için Microsoft Desteği'ne başvurduğunuzda, ağ geçidi günlüklerinizi paylaşmanız istenebilir. Ağ geçidinin serbest bırakılması ile, gerekli ağ geçidi günlüklerini Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde iki düğme tıklamasıyla paylaşabilirsiniz.    

1. Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde **Tanılama** sekmesine geçin.

    ![Veri Yönetimi Ağ Geçidi Tanılama sekmesi](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Aşağıdaki iletişim kutusunu görmek için **Günlük gönder'i** tıklatın.

    ![Veri Yönetimi Ağ Geçidi Gönder günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (İsteğe bağlı) Olay görüntüleyicisindeki günlükleri incelemek için **günlükleri görüntülemeyi** tıklatın.
4. (İsteğe bağlı) Microsoft web hizmetleri gizlilik bildirimini incelemek için **gizliliği** tıklatın.
5. Yüklemek üzere olduğunuz şeyden memnun olduğunuzda, son yedi güne ait günlükleri sorun giderme için Microsoft'a göndermek için **Günlük gönder'i** tıklatın. Aşağıdaki ekran görüntüsünde gösterildiği gibi gönder günlükleri işleminin durumunu görmeniz gerekir.

    ![Veri Yönetimi Ağ Geçidi Gönder günlükleri durumu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. İşlem tamamlandıktan sonra, aşağıdaki ekran görüntüsünde gösterildiği gibi bir iletişim kutusu görürsünüz.

    ![Veri Yönetimi Ağ Geçidi Gönder günlükleri durumu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Rapor **Kimliğini** kaydedin ve Microsoft Desteği ile paylaşın. Rapor kimliği, sorun giderme için yüklediğiniz ağ geçidi günlüklerini bulmak için kullanılır.  Rapor kimliği de olay görüntüleyiciye kaydedilir.  Olay kimliği "25"e bakarak bulabilir ve tarih ve saati kontrol edebilirsiniz.

    ![Veri Yönetimi Ağ Geçidi Gönder günlükleri rapor kimliği](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Ağ geçidi ana bilgisayar makinesinde ağ geçidi günlüklerini arşivleme
Ağ geçidi sorunlarınız olan ve ağ geçidi günlüklerini doğrudan paylaşamadığınız bazı senaryolar vardır:

* Ağ geçidini el ile yükler ve ağ geçidini kaydedersiniz.
* Ağ geçidini Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde yeniden oluşturulmuş bir anahtarla kaydetmeye çalışırsınız.
* Günlükgöndermeye çalışırsınız ve ağ geçidi ana bilgisayar hizmeti bağlanamaz.

Bu senaryolar için ağ geçidi günlüklerini zip dosyası olarak kaydedebilir ve Microsoft desteğine başvururken paylaşabilirsiniz. Örneğin, ağ geçidini aşağıdaki ekran görüntüsünde gösterildiği gibi kaydederken bir hata alırsanız.   

![Veri Yönetimi Ağ Geçidi Kaydı hatası](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Günlükleri arşivlemek ve kaydetmek için **Arşiv ağ geçidi günlükleri** bağlantısını tıklatın ve ardından zip dosyasını Microsoft desteğiyle paylaşın.

![Veri Yönetimi Ağ Geçidi Arşivi günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Ağ geçidi günlüklerini bulma
Windows olay günlüklerinde ayrıntılı ağ geçidi günlüğü bilgilerini bulabilirsiniz.

1. Windows **Olay Görüntüleyicisi'ni**başlatın.
2. **Uygulama ve Hizmetler Günlükleri** > Veri Yönetimi**Ağ Geçidi** klasöründe günlükleri bulun.

   Ağ geçidiyle ilgili sorunları sorun giderirken, olay görüntüleyicisinde hata düzeyi olayları arayın.

![Olay görüntüleyicide Veri Yönetimi Ağ Geçidi günlükleri](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
