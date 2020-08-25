---
title: Azure Güvenlik Karşılaştırması şema örneğine genel bakış
description: Azure Güvenlik Karşılaştırması şema örneğine genel bakış. Bu şema örneği, müşterilerin belirli denetimleri değerlendirmesine yardımcı olur.
ms.date: 06/02/2020
ms.topic: sample
ms.openlocfilehash: 780d16cd52ef9ea8b5edd654d6c80b9db593ab6d
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "84740382"
---
# <a name="azure-security-benchmark-blueprint-sample"></a>Azure Güvenlik kıyaslaması şeması örneği

Azure Güvenlik Karşılaştırması şema örneği, belirli [Azure Güvenlik Karşılaştırması](../../../security/benchmarks/overview.md) denetimlerini değerlendirmenize yardımcı olan [Azure İlkesi](../../policy/overview.md)’ni kullanarak idare korumaları sağlar. Bu şema müşterilerin, Azure Güvenlik Karşılaştırması denetimlerini uygulamayı amaçladığı, Azure’da dağıtılmış herhangi bir mimari için bir dizi ilke dağıtmasına yardımcı olur.

## <a name="control-mapping"></a>Denetim eşlemesi

[Azure ilke denetimi eşleme](../../policy/samples/azure-security-benchmark.md) , bu şema içinde yer alan ilke tanımlarına ilişkin ayrıntıları ve bu Ilke tanımlarının Azure Güvenlik kıyaslamalarındaki **Uyumluluk etki alanları** ve **denetimleriyle** nasıl eşlendiğini açıklar. Bir mimariye atandığında kaynaklar, atanan ilke tanımlarına yönelik uyumsuzluk için Azure Ilkesi tarafından değerlendirilir. Daha fazla bilgi için bkz. [Azure İlkesi](../../policy/overview.md).

## <a name="deploy"></a>Dağıtma

Azure şemaları Azure Güvenlik kıyaslama şeması örneğini dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist"]
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

### <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **Azure Güvenlik kıyaslaması** şeması örneğini bulun ve bu örneği seçmek için ada tıklayın ' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:

   - **Şema adı**: Azure Güvenlik kıyaslaması şeması örneğinin kopyasına bir ad verin.
   - **Tanım konumu**: üç noktayı kullanın ve örneğin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya sonraki: sayfanın en altındaki **yapıtları** seçin.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

### <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik, Azure güvenliği kıyaslama önerileri ile hizalamadan uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "Azure Güvenlik kıyaslaması şeması örneğinden yayımlanan Ilk sürüm" gibi **değişiklik notlarını** sağlar. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

### <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: şema örneğinin kopyasını kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: yönetilen kimliğin oluşturulacağı bölgeyi seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
     - Şema **tanımı sürümü**: şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.

   - Kilit ataması

     Ortamınız için BLUEPRINT Lock ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistem tarafından _atanmış_ yönetilen kimlik seçeneğini bırakın.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlanan yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra sayfanın alt kısmındaki **ata** ' yı seçin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım kabaca bir saat sürer. Dağıtımın durumunu denetlemek için, BLUEPRINT atamasını açın.

> [!WARNING]
> Azure şemaları hizmeti ve yerleşik şema örnekleri **ücretsiz olarak ücretsizdir**. Azure kaynakları [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan çalışan kaynakların maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

### <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıt parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Windows VM Yöneticileri grubundan dışlanan kullanıcıların listesi|Yöneticiler yerel grubunda dışlanması gereken üyelerin noktalı virgülle ayrılmış listesi. Ex: yönetici; myUser1; myUser2|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Windows VM yöneticileri grubuna dahil olması gereken kullanıcıların listesi|Yöneticiler yerel grubuna dahil edilecek üyelerin noktalı virgülle ayrılmış listesi. Ex: yönetici; myUser1; myUser2|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Windows VM yöneticileri grubu 'nun *yalnızca* şunları içermesi gereken kullanıcılar listesi|Administrators yerel grubunun beklenen tüm üyelerinin noktalı virgülle ayrılmış listesi. Ex: yönetici; myUser1; myUser2|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Ağ Izleyicisi 'nin etkinleştirilmesi gereken bölgelerin listesi|Bölgelerin tam bir listesini görmek için Get-AzLocation kullanın|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|VM 'Lerin bağlanması gereken sanal ağ|Örnek:/subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroupName/providers/Microsoft.Network/virtualNetworks/Name|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Sanal ağların kullanması gereken ağ geçidi|Örnek:/subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroup/providers/Microsoft.Network/virtualNetworkGateways/Name|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Log Analytics aracılarının bağlanacağı çalışma alanı kimliklerinin listesi|Log Analytics aracısının bağlanması gereken çalışma alanı kimliklerinin noktalı virgülle ayrılmış listesi|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi|Seçili kaynak türleri için tanılama ayarını denetle|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|En son PHP sürümü|Uygulama Hizmetleri için desteklenen en son PHP sürümü|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|En son Java sürümü|Uygulama Hizmetleri için desteklenen en son Java sürümü|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|En son Windows Python sürümü|Uygulama Hizmetleri için desteklenen en son Python sürümü|
|Azure Güvenlik kıyaslama önerilerini denetleme ve belirli destekleyici VM uzantılarını dağıtma|İlke ataması|En son Linux Python sürümü|Uygulama Hizmetleri için desteklenen en son Python sürümü|

## <a name="next-steps"></a>Sonraki adımlar

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.