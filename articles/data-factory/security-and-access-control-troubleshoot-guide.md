---
title: Güvenlik ve erişim denetimi sorunlarını giderme
description: Azure Data Factory güvenlik ve erişim denetimi sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/19/2020
ms.author: lle
ms.reviewer: craigg
ms.openlocfilehash: 99c03ae4430d1a4caf575bdb9900200af0217bf1
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97109780"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Azure Data Factory güvenlik ve erişim denetimi sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki güvenlik ve erişim denetimi için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve mesajlar


### <a name="connectivity-issue-of-copy-activity-in-cloud-data-store"></a>Bulut veri deposundaki kopyalama etkinliğinin bağlantı sorunu

#### <a name="symptoms"></a>Belirtiler

Kaynak/havuz veri deposu için bağlantı sorunu oluştuğunda çeşitli hata iletileri döndürülebilir.

#### <a name="cause"></a>Nedeni 

Sorun çoğunlukla aşağıdaki faktörlerden kaynaklanır:

1. Şirket içinde barındırılan IR düğümündeki ara sunucu ayarı (Şirket içinde barındırılan IR kullanıyorsanız)

1. Şirket içinde barındırılan IR düğümündeki güvenlik duvarı ayarı (Şirket içinde barındırılan IR kullanıyorsanız)

1. Bulut veri deposundaki güvenlik duvarı ayarı

#### <a name="resolution"></a>Çözüm

1. Sorunun bağlantı sorunundan kaynaklanmasından emin olmak için öncelikle aşağıdaki noktaları kontrol edin:

   - Hata, kaynak/havuz bağlayıcılarından oluşur.

   - Etkinlik, kopyanın başlangıcında başarısız olur

   - Düğümlerin yalnızca bir bölümünde sorun varsa, birden çok düğümlü şirket içinde barındırılan IR için rastgele bir hata olduğundan, tek düğümlü Azure IR veya şirket içinde barındırılan IR için tutarlı bir hata olabilir.

1. Aynı veri deposuna çalıştırma Azure IR başarılı bir şekilde, **Şirket içinde BARıNDıRıLAN IR** kullanıyorsanız, ara sunucu, güvenlik duvarı ve ağ ayarlarınızı kontrol edin. Sorun gidermek için lütfen aşağıdaki bağlantılara bakın:

   [Şirket içinde BARıNDıRıLAN IR bağlantı noktaları ve güvenlik duvarları](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#ports-and-firewalls) 
    [ADLS Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store)
  
1. **Azure IR** kullanıyorsanız, lütfen veri deposunun güvenlik duvarı ayarını devre dışı bırakmayı deneyin. Bu şekilde, aşağıdaki iki duruma yönelik sorun düzeltilebilir:
  
   * [Azure IR IP adresleri](https://docs.microsoft.com/azure/data-factory/azure-integration-runtime-ip-addresses) izin verilenler listesinde değil.

   * *Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver* özelliği [Azure Blob depolama](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#supported-capabilities) ve [ADLS Gen 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#supported-capabilities)için kapalı.

   * *Azure hizmetlerine erişim izni* ADLS 1. için etkinleştirilmemiş.

1. Yukarıdaki yöntemler çalışmadıysanız yardım için lütfen Microsoft 'a başvurun.


### <a name="invalid-or-empty-authentication-key-issue-after-public-network-access-is-disabled"></a>Genel ağ erişimi devre dışı bırakıldıktan sonra geçersiz veya boş kimlik doğrulama anahtarı sorunu

#### <a name="symptoms"></a>Belirtiler

Data Factory için genel ağ erişimini devre dışı bıraktıktan sonra, şirket içinde barındırılan tümleştirme çalışma zamanı şu hatayı veriyor: "kimlik doğrulama anahtarı geçersiz veya boş."

#### <a name="cause"></a>Nedeni

Sorun büyük olasılıkla DNS çözümleme sorunundan kaynaklanır, ancak genel bağlantıyı devre dışı bırakıp özel bir uç nokta kurarak yeniden bağlanma konusunda yardım yoktur.

Data Factory FQDN 'sinin genel IP adresi olarak çözümlenip çözümlenmediğini doğrulamak için aşağıdaki adımları izleyebilirsiniz:

1. Azure VM 'yi Data Factory özel uç noktayla aynı VNET 'te oluşturdunuz olduğunu doğrulayın.

2. Azure VM 'den Data Factory FQDN 'sine yönelik PsPing ve PING komutunu çalıştırın:

   `ping <dataFactoryName>.<region>.datafactory.azure.net`

   `psping.exe <dataFactoryName>.<region>.datafactory.azure.net:443`

   > [!Note]
   > PsPing komutu için bir bağlantı noktası belirtilmesi gerekir, ancak 443 bağlantı noktası bir değer değildir.

3. Her iki komutun da belirtilen bölgeye göre ADF genel IP 'ye çözümlendiğini denetleyin (biçim xxx. xxx. xxx. 0).

#### <a name="resolution"></a>Çözüm

- [Azure Data Factory Için Azure özel bağlantısı](https://docs.microsoft.com/azure/data-factory/data-factory-private-link#dns-changes-for-private-endpoints)makalesine başvurabilirsiniz. Yönerge, özel DNS bölgesi/sunucusunu, Data Factory FQDN 'yi özel IP adresine çözümlemek üzere yapılandırmaya yöneliktir.

- Özel DNS bölgesini/sunucusunu Şu anda yapılandırmak istemiyorsanız, lütfen geçici çözüm olarak aşağıdaki adımları uygulayın. Ancak, özel DNS, uzun süreli çözüm olarak önerilmeye devam etmektedir.

  1. Windows 'daki konak dosyasını değiştirin ve özel IP 'yi (ADF özel uç nokta) ADF FQDN olarak eşleyin.
  
     Azure VM 'de "C:\Windows\System32\drivers\etc" yoluna gidin ve **konak** dosyasını Not defteri ile açın. Özel IP eşleme satırını dosyanın sonundaki FQDN 'ye ekleyin ve değişikliği kaydedin.
     
     ![Konağa eşleme Ekle](media/self-hosted-integration-runtime-troubleshoot-guide/add-mapping-to-host.png)

  1. Özel IP 'yi içermesi gereken yanıtı denetlemek için yukarıdaki doğrulama adımlarında aynı komutları yeniden çalıştırın.

  1. Şirket içinde barındırılan tümleştirme çalışma zamanını yeniden kaydedin ve sorun çözümlenmelidir.
 

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>Özel bağlantı nedeniyle, şirket içinde barındırılan VM 'Lere IR kimlik doğrulama anahtarı kaydedilemiyor

#### <a name="symptoms"></a>Belirtiler

Özel bağlantı etkin olduğundan, şirket içinde barındırılan VM 'ye IR kimlik doğrulama anahtarı kaydedilemiyor.

Hata bilgileri aşağıda gösterildiği gibi gösterilmektedir:

`
Failed to get service token from ADF service with key *************** and time cost is: 0.1250079 seconds, the error code is: InvalidGatewayKey, activityId is: XXXXXXX and detailed error message is Client IP address is not valid private ip Cause Data factory couldn’t access the public network thereby not able to reach out to the cloud to make the successful connection.
`

#### <a name="cause"></a>Nedeni

Bu sorun, şirket içinde barındırılan IR 'yi yüklemeye çalışırken kullandığınız VM 'nin nedeni olabilir. Buluta bağlanmak için genel ağ erişiminin etkinleştirildiğinden emin olmanız gerekir.

#### <a name="resolution"></a>Çözüm

 **Çözüm 1:** Sorunu çözmek için aşağıdaki adımları izleyebilirsiniz:

1. [Fabrikalar-Güncelleştir](https://docs.microsoft.com/rest/api/datafactory/Factories/Update) sayfasına gidin.

1. Sağ üst köşedeki **deneyin** düğmesini seçin.

1. **Parametreler** altında gerekli bilgileri doldurun. 

1. **Gövde** altında aşağıdaki özelliği yapıştırın:
    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. İşlevi çalıştırmak için **Çalıştır** ' ı seçin. 

1. **Yanıt kodu: 200** ' nin görüntülendiğini onaylayın. Yapıştırdığınız özelliğin JSON tanımında de görüntülenmesi gerekir.

1. IR kimlik doğrulama anahtarını tümleştirme çalışma zamanına yeniden ekleyin.


**Çözüm 2:** Çözüm için aşağıdaki makaleye başvurabilirsiniz:

https://docs.microsoft.com/azure/data-factory/data-factory-private-link

Aşağıdaki ekran görüntüsünde gösterildiği gibi, Kullanıcı arabiriminde ortak ağ erişimini etkinleştirmeyi deneyin:

![Ortak ağ erişimini etkinleştir](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory için özel bağlantı](data-factory-private-link.md)
*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q bir sayfa&](/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
