---
title: SWIFT CSP-CSCF v2020 plan örneğini dağıtın
description: PLAN artefakt parametre ayrıntılarını içeren SWIFT CSP-CSCF v2020 plan örneği için adımları dağıtın.
ms.date: 09/24/2019
ms.topic: sample
ms.openlocfilehash: 63a3880d0092f1ed6fabb2b024d05eebb79fe58b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74546484"
---
# <a name="deploy-the-swift-csp-cscf-v2020-blueprint-sample"></a>SWIFT CSP-CSCF v2020 plan örneğini dağıtın

Azure Blueprints SWIFT CSP-CSCF v2020 plan örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. **Tüm hizmetleri** seçin ve sol bölmede **İlke'yi** arayın ve seçin. **İlke** sayfasında **Planlar'ı**seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ in altında **SWIFT CSP-CSCF v2020** plan örneğini bulun ve bu örneği **kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: SWIFT CSP-CSCF v2020 plan örneğinin kopyasınız için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu SWIFT CSP-CSCF v2020 kontrolleriyle hizalamadan uzaklaştırabilir.

1. **Tüm hizmetleri** seçin ve sol bölmede **İlke'yi** arayın ve seçin. **İlke** sayfasında **Planlar'ı**seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "SWIFT CSP- CSCF v2020 plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Plan örneğinin kopyası başarıyla **Yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneye atanabilir. Bu adım, plan örneğinin her kopyasını benzersiz hale getirmek için parametrelerin sağlandığı adımdır.

1. **Tüm hizmetleri** seçin ve sol bölmede **İlke'yi** arayın ve seçin. **İlke** sayfasında **Planlar'ı**seçin.

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
|\[Önizleme\]: SWIFT CSP-CSCF v2020 denetimlerini denetleyin ve denetim gereksinimlerini desteklemek için belirli VM Uzantılarını dağıtın|İlke ataması|Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi|Tanılama günlüğü ayarı etkin değilse denetlenenekaynak türlerinin listesi. Kabul edilebilir değerler [Azure Monitor tanı günlükleri şemalarında](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type)bulunabilir.|
|\[Önizleme\]: SWIFT CSP-CSCF v2020 denetimlerini denetleyin ve denetim gereksinimlerini desteklemek için belirli VM Uzantılarını dağıtın|İlke ataması|Bağlı çalışma alanı bağlantıları|Log Analytics aracısının bağlanması gereken çalışma alanı iliklerinin yarı sütunlu ayrılmış bir listesi|
|\[Önizleme\]: SWIFT CSP-CSCF v2020 denetimlerini denetleyin ve denetim gereksinimlerini desteklemek için belirli VM Uzantılarını dağıtın|İlke ataması|Windows VM Yöneticileri grubuna dahil edilmesi gereken kullanıcıların listesi|Yöneticiler yerel grubuna dahil edilmesi gereken yarı sütunlu üyeler listesi. Örn: Yönetici; myUser1; myUser2|
|\[Önizleme\]: SWIFT CSP-CSCF v2020 denetimlerini denetleyin ve denetim gereksinimlerini desteklemek için belirli VM Uzantılarını dağıtın|İlke ataması|Alan Adı (FQDN)|Windows VM'lerinin birleştirilmesi gereken tam nitelikli alan adı (FQDN)|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM Ölçek Setleri (VMSS) için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM'leri için Log Analytics çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM Ölçek Setleri (VMSS) için Günlük Analizi çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM'ler için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|Depolama Hesaplarına Gelişmiş Tehdit Koruması Dağıtma|İlke ataması|Etki|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md) bulabilirsiniz|
|DENETIMI SQL sunucularında dağıtma|İlke ataması|Bekletme döneminin günlerinin değeri (0 sınırsız bekletme gösterir)|Bekletme günleri (isteğe bağlı, belirtilmemişse 180 gün)|
|DENETIMI SQL sunucularında dağıtma|İlke ataması|SQL sunucu denetimi için depolama hesabı için kaynak grubu adı|Denetim, veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne yazar (o bölgedeki tüm sunucular tarafından paylaşılacak bir SQL Server'ın oluşturulduğu her bölgede bir depolama hesabı oluşturulur). Önemli - Denetimin düzgün çalışması için kaynak grubunu veya depolama hesaplarını silmeyin veya yeniden adlandırmayın.|
|Ağ Güvenlik Grupları için tanılama ayarlarını dağıtma|İlke ataması|Ağ güvenliği grubu tanılama için depolama hesabı öneki|Bu önek, oluşturulan depolama hesabı adını oluşturmak için ağ güvenlik grubu konumuyla birleştirilir.|
|Ağ Güvenlik Grupları için tanılama ayarlarını dağıtma|İlke ataması|Ağ güvenlik grubu tanılama için depolama hesabı için kaynak grubu adı (var olmalıdır)|Depolama hesabının oluşturulacağı kaynak grubu. Bu kaynak grubu zaten var olmalıdır.|

## <a name="next-steps"></a>Sonraki adımlar

SWIFT CSP-CSCF v2020 plan örneğini dağıtma adımlarını gözden geçirdiğiniz için, plan ve kontrol eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [SWIFT CSP-CSCF v2020 planı - Genel Bakış](./index.md)
> [SWIFT CSP-CSCF v2020 planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
