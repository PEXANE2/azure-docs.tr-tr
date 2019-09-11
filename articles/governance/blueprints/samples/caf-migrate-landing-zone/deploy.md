---
title: Örnek-CAF geçiş giriş bölgesi şeması-dağıtım adımları
description: CAF geçişi giriş bölgesi şema örneğinin adımlarını dağıtın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/20/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.custom: fasttrack-new
ms.openlocfilehash: 520eded941f625bf90204213188ac1afc94d3111
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802587"
---
# <a name="deploy-the-microsoft-cloud-adoption-framework-for-azure-migrate-landing-zone-blueprint-sample"></a>Azure geçişi giriş bölgesi şeması örneği için Microsoft Bulut benimseme çerçevesini dağıtma

Azure planları CAF geçiş giriş bölgesi şema örneğini dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist1"]
> - [CAF Foundation](../caf-foundation/index.md) şema örneğini dağıtmak için önerilir

> [!div class="checklist2"]
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **CAF geçiş giriş bölgesi** şema örneğini bulun ve **Bu örneği kullan**' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:
   - **Blueprint adı** CAF geçiş giriş bölgesi şeması örneğinin kopyasına bir ad verin.
   - **Tanım konumu** Üç noktayı kullanın ve örnek kopyanızı ' e kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya **ileri ' yi seçin: Sayfanın** alt kısmındaki yapıtlar.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik, CAF geçiş giriş bölgesi kılavuzunlarından uzaklaştırabilir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "CAF geçiş giriş bölgesinden yayınlanan ilk sürüm, şema örneğinden" gibi **değişiklik notları** sağlayın. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel
     - **Abonelikler**: Şema örneğinin kopyasına kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: Ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: Yönetilen kimliğin oluşturulacağı bölgeyi seçin.
     - Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır.
       Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Blueprint tanım sürümü**: Şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.
    
   - Atamayı Kilitle

     Ortamınız için BLUEPRINT Lock ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan _sistem tarafından atanmış_ yönetilen kimlik seçeneğini veya _Kullanıcı tarafından atanan_ kimlik seçeneğini belirleyin.

   - Şema parametreleri

     Bu bölümde tanımlanan parametreler, tutarlılığı sağlamak için şema tanımındaki birçok yapıtlar tarafından kullanılır.

       - **Kuruluş**: Contoso veya fabrikam gibi kuruluşunuzun adını girin, benzersiz olmalıdır.
       - **Azureregion**: Dağıtım için bir Azure bölgesi seçin.
       
   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlanan yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra sayfanın alt kısmındaki **ata** ' yı seçin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım yaklaşık beş dakika sürer. Dağıtımın durumunu denetlemek için, BLUEPRINT atamasını açın.

> [!WARNING]
> Azure şemaları hizmeti ve yerleşik şema örnekleri **ücretsiz olarak ücretsizdir**. Azure kaynakları [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan çalışan kaynakların maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıt parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|VNET giriş bölgesini dağıt|Resource Manager şablonu|IPAddress_Space|**Kilitli** -ilk iki sekizli örnek sağlayın, 10,0|
|Key Vault dağıt|Resource Manager şablonu|KV-AccessPolicy|Key Vault içinde izinler vermek için **kilitli** grup veya Kullanıcı nesne kimliği|
|Log Analytics dağıt|Resource Manager şablonu|LogAnalytics_DataRetention|**Kilitli** -Log Analytics verilerin tutulacağı gün sayısı|
|Log Analytics dağıt|Resource Manager şablonu|LogAnalytics_Location|Çalışma alanı oluşturulurken kullanılan **kilitli** bölge|
|Azure geçişi 'ni dağıtma|Resource Manager şablonu|Azure_Migrate_Location|**Kilitli** -Azure geçişi 'ni dağıtmak için bölgeyi seçin|

## <a name="next-steps"></a>Sonraki adımlar

CAF geçiş giriş bölgesi şeması örneğini dağıtma adımlarını gözden geçirdiğinize göre, mimari hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [CAF geçiş giriş bölgesi şeması-genel bakış](./index.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.