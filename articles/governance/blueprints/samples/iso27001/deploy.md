---
title: ISO 27001 plan örneğini dağıtın
description: Plan yapı parametresi ayrıntılarını da içeren bağımsız ISO 27001 plan örneği için adımları dağıtın.
ms.date: 01/13/2020
ms.topic: sample
ms.openlocfilehash: f5920541d49c454bd6648318c04ed607eada1dfb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75922516"
---
# <a name="deploy-the-iso-27001-blueprint-sample"></a>ISO 27001 plan örneğini dağıtın

Azure Planları ISO 27001 plan örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ altında **ISO 27001** plan örneğini bulun ve **bu örneği kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: ISO 27001 plan örneğinin kopyası için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu ISO 27001 standardından uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "ISO 27001 plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

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

   - Şema parametreleri

     Bu bölümde tanımlanan parametreler tutarlılık sağlamak için plan tanımındaki birçok yapıt tarafından kullanılır.

     - **Kaynaklar ve kaynak grupları için izin verilen konum**: Kaynak grupları ve kaynakları için izin verilen konumları gösteren değer.

   - Yapı parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapı için geçerlidir. Bu parametreler, planın atanması sırasında tanımlandıkları için [dinamik parametrelerdir.](../../concepts/parameters.md#dynamic-parameters) Tam liste veya yapı parametreleri ve açıklamaları için [Yapı parametreleri tablosuna](#artifact-parameters-table)bakın.

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata'yı** seçin. Plan ataması oluşturulur ve artefakt dağıtımı başlar. Dağıtım yaklaşık bir saat sürer. Dağıtım durumunu denetlemek için plan atamasını açın.

> [!WARNING]
> Azure Planları hizmeti ve yerleşik plan örnekleri **ücretsizdir.** Azure kaynakları [ürüne göre fiyatlandırılır.](https://azure.microsoft.com/pricing/) Bu plan örneği tarafından dağıtılan kaynakların çalışma maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapı parametreleri tablosu

Aşağıdaki tablo, plan yapı parametrelerinin bir listesini sağlar:

|Artefakt adı|Artefakt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM Ölçek Setleri (VMSS) için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM'leri için Log Analytics çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM Ölçek Setleri (VMSS) için Günlük Analizi çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM'ler için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|İzin verilen depolama hesabı SKU'ları|İlke ataması|İzin verilen depolama SUS'ları listesi|Depolama hesapları için belirtilebilen SNU listesi.|
|İzin verilen sanal makine SUS'lar|İlke ataması|İzin verilen sanal makine SUS'ları listesi|Sanal makineler için belirtilebilen SNU listesi.|
|ISO 27001 için plan girişimi|İlke ataması|Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi|Tanılama günlüğü ayarı etkin değilse denetlenenekaynak türlerinin listesi. Kabul edilebilir değerler [Azure Monitor tanı günlükleri şemalarında](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type)bulunabilir.|

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 plan örneğini dağıtma adımlarını gözden geçirdiğiniz için, mimari ve denetim eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 planı - Genel Bakış](./index.md)
> [ISO 27001 planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
