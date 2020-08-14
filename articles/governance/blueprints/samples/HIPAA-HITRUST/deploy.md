---
title: HIPAA itrust şema örneğini dağıt
description: Bluezrust şema örneği için, şema yapıt parametresi ayrıntıları dahil olmak üzere adımları dağıtın.
ms.date: 08/03/2020
ms.topic: sample
ms.openlocfilehash: f41827d687beb583ea6402d517e88b8cf69903a0
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88211532"
---
# <a name="deploy-the-hipaa-hitrust-blueprint-sample"></a>HIPAA HiTRUST şema örneğini dağıtma

HIPAA HiTRUST şema örneğini dağıtmak için aşağıdaki adımların alınması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. **Tüm hizmetler** ' i seçin ve sol bölmedeki **ilke** ara ' yı seçin. **İlke** sayfasında, **planlar**' ı seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **HIPAA HiTRUST** şema örneğini bulun ve **Bu örneği kullan**' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:

   - **Şema adı**: şema örneğinin kopyasına bir ad verin.
   - **Tanım konumu**: üç noktayı kullanın ve örneğin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya sonraki: sayfanın en altındaki **yapıtları** seçin.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik bu değişikliği standart dışında taşıyabilir.

1. **Tüm hizmetler** ' i seçin ve sol bölmedeki **ilke** ara ' yı seçin. **İlke** sayfasında, **planlar**' ı seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "HiTRUST HIPAA şema örneğinden yayımlanan ilk sürüm" gibi **değişiklik notları** sağlayın. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. **Tüm hizmetler** ' i seçin ve sol bölmedeki **ilke** ara ' yı seçin. **İlke** sayfasında, **planlar**' ı seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel bilgiler

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

Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Önizleme \] : Linux VM 'ler için Log Analytics aracısı dağıtma |İlke ataması |Linux VM 'Ler için Log Analytics çalışma alanı |Daha fazla bilgi için [Azure portal Log Analytics çalışma alanı oluşturma](../../../../azure-monitor/learn/quick-create-workspace.md)bölümüne bakın. |
|\[Önizleme \] : Linux VM 'ler için Log Analytics aracısı dağıtma |İlke ataması |İsteğe bağlı: kapsama eklemek için desteklenen Linux işletim sistemini destekleyen VM görüntülerinin listesi |Boş bir dizi, isteğe bağlı parametre olmadığını göstermek için kullanılabilir: `[]` |
|\[Önizleme \] : Windows VM 'leri için Log Analytics aracısı dağıtma |İlke ataması |İsteğe bağlı: kapsama eklemek için desteklenen Windows işletim sistemini destekleyen VM görüntülerinin listesi |Boş bir dizi, isteğe bağlı parametre olmadığını göstermek için kullanılabilir: `[]` |
|\[Önizleme \] : Windows VM 'leri için Log Analytics aracısı dağıtma |İlke ataması |Windows VM 'Leri için Log Analytics çalışma alanı |Daha fazla bilgi için [Azure portal Log Analytics çalışma alanı oluşturma](../../../../azure-monitor/learn/quick-create-workspace.md)bölümüne bakın. |
|\[Önizleme \] : denetim gereksinimlerini desteklemek IÇIN ITRUST HIPAA denetimlerini denetleme ve belırlı VM uzantılarını dağıtma |İlke ataması |VM 'Lerin yapılandırılması gereken Log Analytics çalışma alanı KIMLIĞI |Bu, VM 'Lerin için yapılandırılması gereken Log Analytics çalışma alanının KIMLIĞIDIR (GUID). |
|\[Önizleme \] : denetim gereksinimlerini desteklemek IÇIN ITRUST HIPAA denetimlerini denetleme ve belırlı VM uzantılarını dağıtma |İlke ataması |Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi |Tanılama günlüğü ayarı etkinleştirilmemişse denetlenecek kaynak türleri listesi. Kabul edilebilir değerler, [Azure izleyici tanılama günlükleri şemalarında](../../../../azure-monitor/platform/resource-logs-schema.md#service-specific-schemas)bulunabilir. |
|\[Önizleme \] : denetim gereksinimlerini desteklemek IÇIN ITRUST HIPAA denetimlerini denetleme ve belırlı VM uzantılarını dağıtma |İlke ataması |Yöneticiler grubu |Grubu. Örnek: `Administrator; myUser1; myUser2` |
|\[Önizleme \] : denetim gereksinimlerini desteklemek IÇIN ITRUST HIPAA denetimlerini denetleme ve belırlı VM uzantılarını dağıtma |İlke ataması |Windows VM yöneticileri grubuna dahil edilecek kullanıcıların listesi |Yöneticiler yerel grubuna dahil edilecek üyelerin noktalı virgülle ayrılmış listesi. Örnek: `Administrator; myUser1; myUser2` |
|Depolama hesaplarında Gelişmiş tehdit koruması dağıtma |İlke ataması |Etki |İlke etkileri hakkında daha fazla bilgi için bkz. [Azure Ilke efektlerini anlama](../../../policy/concepts/effects.md). |
|SQL Server 'lar üzerinde denetim dağıtma |İlke ataması |Bekletme döneminin gün cinsinden değer (0 sınırsız saklama anlamına gelir) |Bekletme günleri (belirtilmemişse, _180_ gün) |
|SQL Server 'lar üzerinde denetim dağıtma |İlke ataması |SQL Server denetimi için depolama hesabının kaynak grubu adı |Denetim, veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne yazar (bir depolama hesabı, bu bölgedeki tüm sunucular tarafından paylaşılan bir SQL Server oluşturulduğu her bölgede oluşturulur). Önemli-denetimin düzgün çalışması için kaynak grubunu veya depolama hesaplarını silmeyin veya yeniden adlandırmayın. |
|Ağ güvenlik grupları için tanılama ayarlarını dağıtma |İlke ataması |Ağ güvenlik grubu Tanılama için depolama hesabı öneki |Bu ön ek, oluşturulan depolama hesabı adını biçimlendirmek için ağ güvenlik grubu konumuyla birleştirilir. |
|Ağ güvenlik grupları için tanılama ayarlarını dağıtma |İlke ataması |Ağ güvenlik grubu Tanılama için depolama hesabının kaynak grubu adı (var olmalıdır) |Depolama hesabının oluşturulduğu kaynak grubu. Bu kaynak grubu zaten var olmalıdır. |
|\[Önizleme \] : LINUX VM Ölçek Kümeleri için Log Analytics aracısı dağıtma (VMSS) |İlke ataması |Linux VM Ölçek Kümeleri için Log Analytics çalışma alanı (VMSS) |Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl KIMLIĞINE ' Log Analytics katkıda bulunan ' izinleri (veya benzeri) el ile vermeniz gerekir. |
|\[Önizleme \] : WINDOWS VM Ölçek Kümeleri için Log Analytics aracısı dağıtma (VMSS) |İlke ataması |Linux VM Ölçek Kümeleri için Log Analytics çalışma alanı (VMSS) |Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl KIMLIĞINE ' Log Analytics katkıda bulunan ' izinleri (veya benzeri) el ile vermeniz gerekir. |

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, HITRUST HIPAA örneğini dağıtma adımlarını gözden geçirdiğinize göre, genel bakış ve denetim eşlemesi hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [HiTRUST HIPAA şemaları-genel bakış](./index.md) 
>  [HiTRUST HIPAA şemaları-denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
