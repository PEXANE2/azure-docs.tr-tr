---
title: Örnek-PCI-DSS v 3.2.1 şema-Deploy adımları
description: Ödeme kartı sektör verileri güvenliği standart v 3.2.1 şema örneğinin adımlarını dağıtın.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
ms.openlocfilehash: f95f9a592085fd93fba5e6b11a1a75609eb70295
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980936"
---
# <a name="deploy-the-pci-dss-v321-blueprint-sample"></a>PCI DSS v 3.2.1 şema örneğini dağıtma

Azure planları PCI-DSS v 3.2.1 şema örneğini dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist"]
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **PCI DSS v 3.2.1** şema örneğini bulun ve **Bu örneği kullan**' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:

   - **Şema adı**: PCI-DSS v 3.2.1 şema örneğinin kopyası için bir ad sağlayın.
   - **Tanım konumu**: üç noktayı kullanın ve örneğin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya sonraki: sayfanın en altındaki **yapıtları** seçin.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik, PCI-DSS v 3.2.1 standardına göre bir yere taşınabilir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "PCI-DSS v 3.2.1 şema örneğinden yayınlanan ilk sürüm" gibi **değişiklik notları** sağlayın. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: şema örneğinin kopyasını kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: yönetilen kimliğin oluşturulacağı bölgeyi seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - Şema **tanımı sürümü**: şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.

   - Kilit ataması

     Ortamınız için BLUEPRINT Lock ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistem tarafından _atanmış_ yönetilen kimlik seçeneğini bırakın.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlanan yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra sayfanın alt kısmındaki **ata** ' yı seçin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım kabaca bir saat sürer. Dağıtımın durumunu denetlemek için, BLUEPRINT atamasını açın.

> [!WARNING]
> Azure şemaları hizmeti ve yerleşik şema örnekleri **ücretsiz olarak ücretsizdir**. Azure kaynakları [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan çalışan kaynakların maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıt parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Preview @ no__t-1 denetim PCI v 3.2.1: denetim gereksinimlerini desteklemek için belirli VM uzantılarını 2018 denetimleri ve dağıtımı|İlke Ataması|Kaynak türleri listesi | Seçili kaynak türleri için tanılama ayarını denetleyin. Varsayılan değer tüm kaynaklar seçilidir| 
|İzin verilen konumlar|İlke Ataması|Izin verilen konumların listesi|Herhangi bir kaynağın dağıtılması için izin verilen veri merkezi konumları listesi. Bu liste, istenen Azure konumlarına küresel olarak özelleştirilebilir. İzin vermek istediğiniz konumları seçin.| 
|Kaynak grupları için izin verilen konumlar|İlke Ataması |İzin verilen konum |Bu ilke, kuruluşunuzun içinde kaynak grupları oluşturabileceğiniz konumları kısıtlamanıza olanak sağlar. Coğrafi uyumluluk gereksinimlerinizi zorlamak için kullanın.| 
|SQL Server 'lar üzerinde denetim dağıtma|İlke Ataması|Bekletme günleri|Gün sayısı cinsinden veri randevu. Varsayılan değer 180, ancak PCI 365 gerektirir.| 
|SQL Server 'lar üzerinde denetim dağıtma|İlke Ataması|Depolama hesabı için kaynak grubu adı|Denetim, veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne yazar (bir depolama hesabı, bu bölgedeki tüm sunucular tarafından paylaşılacak bir SQL Server oluşturulduğu her bölgede oluşturulur).| 

## <a name="next-steps"></a>Sonraki adımlar

Bu aşamada, PCI-DSS v 3.2.1 şema örneğini dağıtma adımlarını gözden geçirdiğinize göre, genel bakış ve denetim eşlemesi hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [PCI-DSS v 3.2.1 şema-genel bakış](./index.md)
> [PCI-DSS v 3.2.1 şema-Control Mapping](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
