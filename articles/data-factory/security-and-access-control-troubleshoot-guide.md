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
ms.openlocfilehash: 21a1523016502bd7b0a8682461f1fc16acda2ebb
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96008743"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Azure Data Factory güvenlik ve erişim denetimi sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki güvenlik ve erişim denetimi için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve mesajlar

### <a name="invalid-or-empty-authentication-key-issue-after-disabling-public-network-access"></a>Ortak ağ erişimi devre dışı bırakıldıktan sonra geçersiz veya boş kimlik doğrulama anahtarı sorunu

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan tümleştirme çalışma zamanı, Data Factory için genel ağ erişimini devre dışı bıraktıktan sonra "kimlik doğrulama anahtarı geçersiz veya boş" hatası veriyor.

#### <a name="cause"></a>Nedeni

Sorun büyük olasılıkla DNS çözümleme sorunundan kaynaklanır, ancak genel bağlantıyı devre dışı bırakıp özel bir uç nokta kurarak yeniden bağlanma konusunda yardım yoktur.

#### <a name="resolution"></a>Çözüm

1. ADF 'nin FQDN 'sine yönelik bir sorun oluştu ve arabelleğin devre dışı bırakıldıktan sonra bile, ADF 'nin genel bir uç noktasına gittiğini buldu.

1. Özel IP 'yi FQDN ile eşlemek için VM 'deki konak dosyasını değiştirin ve bir kez daha çalıştırın. Arabellek, ADF 'nin doğru özel IP 'sini daha sonra gidebilecek.

1. Şirket içinde barındırılan tümleştirme çalışma zamanını yeniden kaydedin ve çalışır duruma gelir.
 

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>Özel bağlantı nedeniyle, şirket içinde barındırılan VM 'Lere IR kimlik doğrulama anahtarı kaydedilemiyor

#### <a name="symptoms"></a>Belirtiler

Özel bağlantı etkin olduğundan, şirket içinde barındırılan VM 'ye IR kimlik doğrulama anahtarı kaydedilemiyor.

Hata bilgileri aşağıda gösterildiği gibi gösterilmektedir:

`
Failed to get service token from ADF service with key *************** and time cost is: 0.1250079 seconds, the error code is: InvalidGatewayKey, activityId is: XXXXXXX and detailed error message is Client IP address is not valid private ip Cause Data factory couldn’t access the public network thereby not able to reach out to the cloud to make the successful connection.
`

#### <a name="cause"></a>Nedeni

Bu sorun, SHıR 'nin yüklenmeye çalıştığı VM 'den kaynaklanabilir. Buluta bağlanmak için genel ağ erişimi etkinleştirilmelidir.

#### <a name="resolution"></a>Çözüm

 **Çözüm 1:** Sorunu çözmek için aşağıdaki adımları izleyebilirsiniz:

1. Aşağıdaki bağlantıya gidin: 
    
    https://docs.microsoft.com/rest/api/datafactory/Factories/Update

1. **Dene** seçeneğine tıklayın ve tüm gerekli ayrıntıları doldurabilirsiniz. Aşağıdaki özelliği de **gövdeye** yapıştırın:

    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. İşlevi çalıştırmak için sayfanın sonundaki **Çalıştır** ' a tıklayın. Yanıt kodu 200 ' i aldığınızdan emin olun. Yapıştırdığımız Özellik JSON tanımında de gösterilir.

1. Daha sonra, tümleştirme çalışma zamanına IR kimlik doğrulama anahtarını yeniden eklemeyi deneyebilirsiniz.


**Çözüm 2:** Çözüm için aşağıdaki makaleye başvurabilirsiniz:

https://docs.microsoft.com/azure/data-factory/data-factory-private-link

Kullanıcı arabirimiyle genel ağ erişimini etkinleştirmeyi deneyin.

![Ortak ağ erişimini etkinleştir](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory için özel bağlantı](data-factory-private-link.md)
*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)