---
title: İngiltere'de RESMI & İngiltere NHS plan örnekleri dağıtmak
description: Plan artefakt parametre ayrıntıları da dahil olmak üzere İngiltere RESMİ ve İngiltere NHS plan örnekleri için adımları dağıtın.
ms.date: 06/26/2019
ms.topic: sample
ms.openlocfilehash: 3d69f4477163618aa225150ac6352a16e901f826
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74546391"
---
# <a name="deploy-the-uk-official-and-uk-nhs-blueprint-samples"></a>İngiltere RESMİ ve İngiltere NHS plan örneklerini dağıtın

İngiltere RESMİ ve İngiltere NHS plan örneklerini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ altında **İngiltere RESMİ** veya **İngiltere NHS** plan örneği bulun ve bu **örneği kullanın**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: Plan örneğinin kopyası için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu standarttan uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "İngiltere RESMİ veya İngiltere NHS plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

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

Artefakt adı|Artefakt türü|Parametre adı|Açıklama|
|-|-|-|-|
|İngiltere RESMİ veya İngiltere NHS için plan girişimi|İlke ataması |Tanılama günlüklerini denetlemek için kaynak türleri (Politika: İngiltere OFFICIAL veya UK NHS için plan girişimi) |Tanılama günlüğü ayarı not etkinse denetlenir kaynak türlerinin listesi.  Kabul edilebilir değerler [için, Azure Tanı Günlükleri için Desteklenen hizmetler, şemalar ve kategorilere](../../../../azure-monitor/platform/diagnostic-logs-schema.md)bakın. |
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın |İlke ataması |İsteğe bağlı: Linux işletim sistemi'ni destekleyen VM \[görüntülerinin listesi kapsamına eklemek için (İlke: Önizleme:\]Linux VM'leri için Log Analytics Agent'ı dağıtın) |(İsteğe bağlı) Varsayılan değer _yok._ Daha fazla bilgi için bkz. Azure [portalında Bir Günlük Analizi çalışma alanı oluştur.](../../../../azure-monitor/learn/quick-create-workspace.md) |
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt |İlke ataması |İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen \[\]VM görüntülerinin listesi (İlke: Önizleme : Windows VM'ler için Log Analytics Agent'ı dağıt) |(İsteğe bağlı) Varsayılan değer _yok._ Daha fazla bilgi için bkz. Azure [portalında Bir Günlük Analizi çalışma alanı oluştur.](../../../../azure-monitor/learn/quick-create-workspace.md) |

## <a name="next-steps"></a>Sonraki adımlar

Şimdi İngiltere'de RESMİ ve İngiltere NHS plan örnekleri dağıtmak için adımları gözden geçirdim, genel bakış ve kontrol haritalama hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [İngiltere RESMİ ve İngiltere NHS planları - Genel Bakış](./index.md)
> [İngiltere RESMİ ve İngiltere NHS planları - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.