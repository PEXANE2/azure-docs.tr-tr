---
title: Örnek - 800-53 NIST SP R4 şema - dağıtma adımları
description: NIST SP adımları 800-53 R4 şema örnek dağıtın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 206763e2d17f4ad711ff5fd897f1429814e61837
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228908"
---
# <a name="deploy-the-nist-sp-800-53-r4-blueprint-sample"></a>SP NIST 800-53 R4 şema örnek dağıtma

SP Azure şemaları NIST 800-53 R4 şema örnek dağıtmak için aşağıdaki adım atılmalıdır:

> [!div class="checklist"]
> - Örnekten yeni blueprint oluşturma
> - Örnek olarak kopyanızın işaretlemek **yayımlandı**
> - Blueprint kopyasını mevcut bir aboneliğe atayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten Blueprint oluşturma

İlk olarak, şema örnek bir başlangıç örneğini kullanarak ortamınızda yeni bir şema oluşturarak uygulayın.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **NIST SP 800-53 R4** şema örnek altında _diğer örnekleri_ seçip **Bu örneği kullanmak**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: SP NIST 800-53 R4 şema örnek kopyası için bir ad sağlayın.
   - **Tanım konumu**: Üç nokta kullanın ve örneğe kopyasını kaydetmek için yönetim grubunu seçin.

1. Seçin _Yapıtları_ sayfanın üst kısmındaki sekme veya **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Şema örnek değişiklikleri yapıtların listesini gözden geçirin. Yapıtlar birçoğu, daha sonra tanımlarız parametrelere sahip. Seçin **Taslağı Kaydet** bittiğinde şema örnek gözden geçirme.

## <a name="publish-the-sample-copy"></a>Örnek kopyalama yayımlama

Şema örnek kopyanızın artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve olmalıdır **yayımlanan** , atanan ve dağıtılan kullanılmadan önce. Değişiklik, NIST SP ile aynı doğrultuda uzağa 800-53 denetimleri taşımak ancak bu şema kopyasını gereksinimlerine ve ortam için özelleştirilebilir.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni sayfa sağlayan bir **sürüm** şema örnek kopyası için. Bu özellik, daha sonra bir değişikliği yapmak için yararlıdır. Sağlamak **notları değiştirmek** "ilk sürüm 800-53 R4 şema örnek NIST SP yayımlanan gibi." Ardından **Yayımla** sayfanın alt kısmındaki.

## <a name="assign-the-sample-copy"></a>Örnek kopya atama

Blueprint kopyasını başarıyla silindikten sonra **yayımlanan**, yönetim grubu için kaydedildi dahilinde bir aboneliğe atanabilir. Bu adım, burada parametreler şema kopyasını, her dağıtım benzersiz olacak şekilde sağlanır.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: Bir veya daha fazla yönetim grubuna olduğunuz abonelikleri için şema örnek kopyanızın kaydedilen seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
     - **Ödev adı**: Şema adını temel alarak, önceden doldurulmuş adıdır.
       Gerektiği gibi değiştirin ya da olduğu gibi bırakın.
     - **Konum**: Yönetilen kimlikle oluşturulması için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Şema tanımı sürümü**: Çekme bir **yayımlanan** şema örnek kopyanızın sürümü.

   - Kilit atama

     Ortamınızı ayarlama şema kilidi seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan değeri bırakın _sistem tarafından atanan_ yönetilen kimlik seçeneği.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Tam bir liste veya yapıt parametrelerin ve Tanımlamaların için bkz. [Yapıt parametreleri tablo](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki. Şema atamasını oluşturulur ve yapıt dağıtımı başlar. Dağıtım gereken yaklaşık bir saat. Dağıtım durumunu denetlemek için şema atamasını açın.

> [!WARNING]
> Azure şemaları hizmet ve yerleşik şema örnekleri **ücretsiz olarak sunulmaktadır**. Azure kaynaklarıdır [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Kullanım [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) çalıştıran bu şema örnek tarafından dağıtılan kaynakların maliyetini tahmin etmek için.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri Tablo

Aşağıdaki tabloda, yapıt parametreleri şema listesi sağlar:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Önizleme\]: SP NIST 800-53 R4 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma|İlke ataması|VM'ler için yapılandırılmalıdır log Analytics çalışma alanı kimliği|VM'ler için yapılandırılmalıdır Log Analytics çalışma alanı Kimliğini (GUID) budur.|
|\[Önizleme\]: SP NIST 800-53 R4 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma|İlke ataması|Tanılama günlüklerinin etkin olması gereken kaynak türleri listesi|Tanılama Günlüğü ayarı etkin değilse denetim için kaynak türleri listesi. Kabul edilebilir değerler bulunabilir [Azure İzleyici tanılama günlükleri şemaları](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type).|
|\[Önizleme\]: SP NIST 800-53 R4 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma|İlke ataması|Windows VM Yöneticileri grubundan bırakılması gereken kullanıcılar listesi|Yerel Administrators grubunda dışlanan üyeleri noktalı virgülle ayrılmış listesi. Örn: Yönetici; myUser1; myUser2|
|\[Önizleme\]: SP NIST 800-53 R4 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma|İlke ataması|Windows VM Administrators grubuna eklenmesi gereken kullanıcıların listesi|Yerel Administrators grubuna eklenmesi gereken üyeleri noktalı virgülle ayrılmış listesi. Örn: Yönetici; myUser1; myUser2|
|\[Önizleme\]: Linux VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|Linux VM ölçek kümeleri (VMSS) için log Analytics çalışma alanı|Bu çalışma alanı atama kapsamı dışında ise 'Log Analytics katkıda bulunan' izinleri elle vermeniz gerekir (veya benzeri) asıl ilke atama kimliği|
|\[Önizleme\]: Linux VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Kapsama eklemek için Linux işletim sistemi desteklenen bir VM görüntüsü listesi|Boş bir dizi, isteğe bağlı parametre göstermek için kullanılabilir: \[\]|
|\[Önizleme\]: Linux Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|Linux Vm'leri için log Analytics çalışma alanı|Bu çalışma alanı atama kapsamı dışında ise 'Log Analytics katkıda bulunan' izinleri elle vermeniz gerekir (veya benzeri) asıl ilke atama kimliği|
|\[Önizleme\]: Linux Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Kapsama eklemek için Linux işletim sistemi desteklenen bir VM görüntüsü listesi|Boş bir dizi, isteğe bağlı parametre göstermek için kullanılabilir: \[\]|
|\[Önizleme\]: Windows VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|Windows VM ölçek kümeleri (VMSS) için log Analytics çalışma alanı|Bu çalışma alanı atama kapsamı dışında ise 'Log Analytics katkıda bulunan' izinleri elle vermeniz gerekir (veya benzeri) asıl ilke atama kimliği|
|\[Önizleme\]: Windows VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Windows işletim sistemi kapsamına eklenecek desteklenen bir VM görüntüsü listesi|Boş bir dizi, isteğe bağlı parametre göstermek için kullanılabilir: \[\]|
|\[Önizleme\]: Windows Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|Windows Vm'leri için log Analytics çalışma alanı|Bu çalışma alanı atama kapsamı dışında ise 'Log Analytics katkıda bulunan' izinleri elle vermeniz gerekir (veya benzeri) asıl ilke atama kimliği|
|\[Önizleme\]: Windows Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Windows işletim sistemi kapsamına eklenecek desteklenen bir VM görüntüsü listesi|Boş bir dizi, isteğe bağlı parametre göstermek için kullanılabilir: \[\]|
|Gelişmiş tehdit koruması depolama hesaplarında dağıtma|İlke ataması|Etki|İlke etkileri hakkında daha fazla bilgi bulunabilir [Azure İlkesi etkilerini anlama](../../../policy/concepts/effects.md)|
|SQL sunucularında denetim dağıtma|İlke ataması|Gün (0, sınırsız bekletme gösterir) saklama süresi değeri|Saklanacağı gün sayısı (isteğe bağlı, 180 belirtilmezse gün)|
|SQL sunucularında denetim dağıtma|İlke ataması|SQL server denetimi için depolama hesabı için kaynak grubu adı|Denetim veritabanı olayları bir denetim (bir depolama hesabı, bu bölgedeki tüm sunucular tarafından paylaşılan bir SQL Server'ın oluşturulduğu her bölgede oluşturulur), Azure depolama hesabınızdaki günlüğüne yazar. Önemli - denetim düzgün çalışması için silmeyin veya kaynak grubu veya depolama hesaplarını yeniden adlandırın.|
|Ağ güvenlik grupları için tanılama ayarları dağıtma|İlke ataması|Ağ güvenlik grubu tanılama depolama hesabı ön ekini|Bu ön ek, oluşturduğunuz depolama hesabı adı oluşturmak için ağ güvenlik grubu konumu ile birleştirilir.|
|Ağ güvenlik grupları için tanılama ayarları dağıtma|İlke ataması|Kaynak grubu adı için ağ güvenlik grubu tanılama için depolama hesabı (mevcut olması gerekir)|Depolama hesabının oluşturulacağı kaynak grubu. Bu kaynak grubu mevcut olmalıdır.|

## <a name="next-steps"></a>Sonraki adımlar

SP NIST 800-53 R4 şema örnek dağıtma adımları gözden geçirdikten sonra şema ve denetimi eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [NIST SP 800-53 R4 şema - genel bakış](./index.md)
> [NIST SP 800-53 R4 şema - denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
