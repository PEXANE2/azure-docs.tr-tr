---
title: PCI-DSS v3.2.1 plan örneğini dağıtma
description: Ödeme Kartı Endüstrisi Veri Güvenliği Standardı v3.2.1 plan örneği için plan yapı parametresi ayrıntılarını içeren adımları dağıtın.
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 9df9392430fd0496ffb5e635a6fc3e31ba708208
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76905549"
---
# <a name="deploy-the-pci-dss-v321-blueprint-sample"></a>PCI-DSS v3.2.1 plan örneğini dağıtma

Azure Blueprints PCI-DSS v3.2.1 plan örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ in altında **PCI-DSS v3.2.1** plan örneğini bulun ve **bu örneği kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: PCI-DSS v3.2.1 plan örneğinin kopyası için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu PCI-DSS v3.2.1 standardından uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "PCI-DSS v3.2.1 plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Plan örneğinin kopyası başarıyla **Yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneye atanabilir. Bu adım, plan örneğinin her kopyasını benzersiz hale getirmek için parametrelerin sağlandığı adımdır.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Plan tanımı sayfasının üst **kısmındaki plan atay'ı** seçin.

1. Plan ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: Plan örneğinin kopyasını kaydettiğiniz yönetim grubundaki aboneliklerden birini veya birkaçını seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: Adı, planın adına göre sizin için önceden doldurulur.
       Gerektiği gibi değiştirin veya olduğu gibi ayrılın.
     - **Konum**: Oluşturulan yönetilen kimlik için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Blueprint tanımı sürümü**: Plan örneğinin kopyanızın **Yayınlanmış** sürümünü seçin.

   - Kilit Ataması

     Ortamınız için plan kilidi ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistemi yönetilen kimlik _seçeneğiatanmış_ bırakın.

   - Yapı parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapı için geçerlidir. Bu parametreler, planın atanması sırasında tanımlandıkları için [dinamik parametrelerdir.](../../concepts/parameters.md#dynamic-parameters) Tam liste veya yapı parametreleri ve açıklamaları için [Yapı parametreleri tablosuna](#artifact-parameters-table)bakın.

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata'yı** seçin. Plan ataması oluşturulur ve artefakt dağıtımı başlar. Dağıtım yaklaşık bir saat sürer. Dağıtım durumunu denetlemek için plan atamasını açın.

> [!WARNING]
> Azure Planları hizmeti ve yerleşik plan örnekleri **ücretsizdir.** Azure kaynakları [ürüne göre fiyatlandırılır.](https://azure.microsoft.com/pricing/) Bu plan örneği tarafından dağıtılan kaynakların çalışma maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapı parametreleri tablosu

Aşağıdaki tablo, plan yapı parametrelerinin bir listesini sağlar:

|Artefakt adı|Artefakt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Denetim\] PCI v3.2.1:2018 denetimlerini önizleme ve denetim gereksinimlerini desteklemek için belirli VM Uzantılarını dağıtma|İlke Ataması|Kaynak Türleri Listesi | Seçili kaynak türleri için tanı ayarını denetle. Varsayılan değer tüm kaynaklar seçilir| 
|İzin verilen konumlar|İlke Ataması|İzin verilen konumlar listesi|Herhangi bir kaynağın dağıtılmasına izin verilen veri merkezi konumlarının listesi. Bu liste, genel olarak istenen Azure konumlarına göre özelleştirilebilir. İzin vermek istediğiniz yerleri seçin.| 
|Kaynak grupları için İzin Verilen Konumlar|İlke Ataması |İzin Verilen Konum |Bu ilke, kuruluşunuzun kaynak grupları oluşturabileceği konumları kısıtlamanızı sağlar. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz.| 
|DENETIMI SQL sunucularında dağıtma|İlke Ataması|Bekletme günleri|Gün sayısında veri saklama. Varsayılan değer 180'dir, ancak PCI 365 gerektirir.| 
|DENETIMI SQL sunucularında dağıtma|İlke Ataması|Depolama hesabı için kaynak grubu adı|Denetim, veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne yazar (o bölgedeki tüm sunucular tarafından paylaşılacak bir SQL Server'ın oluşturulduğu her bölgede bir depolama hesabı oluşturulur).| 

## <a name="next-steps"></a>Sonraki adımlar

PCI-DSS v3.2.1 plan örneğini dağıtma adımlarını gözden geçirdiğiniz için, genel bakış ve denetim eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 planı - Genel Bakış](./index.md)
> [PCI-DSS v3.2.1 planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
