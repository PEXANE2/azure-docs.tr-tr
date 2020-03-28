---
title: FedRAMP Yüksek plan örneğini dağıtın
description: Plan artefakt parametre ayrıntıları da dahil olmak üzere FedRAMP Yüksek plan örneği için adımları dağıtın.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 91f208105a07770c557b1f273f3420c03b3d6dcb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77087307"
---
# <a name="deploy-the-fedramp-high-blueprint-sample"></a>FedRAMP Yüksek plan örneğini dağıtın

Azure Planları FedRAMP Yüksek plan örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ altında **FedRAMP Yüksek** plan örneğini bulun ve bu örneği **kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: FedRAMP High plan örneğinin kopyası için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu FedRAMP Yüksek kontrolleriyle hizalamadan uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "FedRAMP High plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

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
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|VM'lerin yapılandırılması gereken Log Analytics çalışma alanı kimliği|Bu, VM'lerin yapılandırılması gereken Log Analytics çalışma alanının kimliğidir (GUID).|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi|Tanılama günlüğü ayarı etkin değilse denetlenenekaynak türlerinin listesi. Kabul edilebilir değerler [Azure Monitor tanı günlükleri şemalarında](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type)bulunabilir.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Windows VM Yöneticileri grubundan dışlanacak kullanıcıların listesi|Yöneticiler yerel grubunda dışlanmalıdır üyelerin yarı sütun ayrılmış listesi. Örn: Yönetici; myUser1; myUser2|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Windows VM Yöneticileri grubuna dahil edilmesi gereken kullanıcıların listesi|Yöneticiler yerel grubuna dahil edilmesi gereken yarı sütunlu üyeler listesi. Örn: Yönetici; myUser1; myUser2|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM Ölçek Setleri (VMSS) için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|Linux VM'leri için Log Analytics çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM Ölçek Setleri (VMSS) için Günlük Analizi çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|Windows VM'ler için Günlük Analitik çalışma alanı|Bu çalışma alanı atama kapsamının dışındaysa, ilke atamasının asıl kimliğine 'Log Analytics Katılımcısı' izinlerini (veya benzeri) el ile vermeniz gerekir.|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|İsteğe bağlı parametreleri belirtmek için boş bir dizi kullanılabilir:\[\]|
|Depolama Hesaplarına Gelişmiş Tehdit Koruması Dağıtma|İlke ataması|Etki|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|DENETIMI SQL sunucularında dağıtma|İlke ataması|Bekletme döneminin günlerinin değeri (0 sınırsız bekletme gösterir)|Bekletme günleri (isteğe bağlı, belirtilmemişse 180 gün)|
|DENETIMI SQL sunucularında dağıtma|İlke ataması|SQL sunucu denetimi için depolama hesabı için kaynak grubu adı|Denetim, veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne yazar (o bölgedeki tüm sunucular tarafından paylaşılacak bir SQL Server'ın oluşturulduğu her bölgede bir depolama hesabı oluşturulur). Önemli - Denetimin düzgün çalışması için kaynak grubunu veya depolama hesaplarını silmeyin veya yeniden adlandırmayın.|
|Ağ Güvenlik Grupları için tanılama ayarlarını dağıtma|İlke ataması|Ağ güvenliği grubu tanılama için depolama hesabı öneki|Bu önek, oluşturulan depolama hesabı adını oluşturmak için ağ güvenlik grubu konumuyla birleştirilir.|
|Ağ Güvenlik Grupları için tanılama ayarlarını dağıtma|İlke ataması|Ağ güvenlik grubu tanılama için depolama hesabı için kaynak grubu adı (var olmalıdır)|Depolama hesabının oluşturulacağı kaynak grubu. Bu kaynak grubu zaten var olmalıdır.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Kaynaklar ve kaynak grupları için izin verilen konumlar|Kuruluşunuzun kaynakları dağıtırken belirtebileceği Azure konumları listesi. Bu sağlanan değer, ilke girişimi içinde 'İzin Verilen konumlar' ilkesi tarafından da kullanılır.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Sanal Makinelerde güvenlik açığı değerlendirmesi etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Depolama Hesapları için coğrafi yedekli depolama etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|MariaDB için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|MySQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|PostgreSQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Sanal makinelere bakan internet için Ağ Güvenliği Grubu kuralları sertleştirilmiş olmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|CORS her kaynağın Web Uygulamanıza erişmesine izin vermemelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|
|\[Önizleme\]: Denetim FedRAMP Yüksek kontrolleri ve denetim gereksinimlerini desteklemek için belirli VM Uzantıları dağıtmak|İlke ataması|Azure SQL Veritabanları için uzun vadeli coğrafi yedekli yedekleme etkinleştirilmelidir|İlke efektleri hakkındaki bilgileri [Azure İlke Efektlerini Anla'da](../../../policy/concepts/effects.md)bulabilirsiniz.|


## <a name="next-steps"></a>Sonraki adımlar

Şimdi FedRAMP Yüksek plan örneği dağıtmak için adımları gözden geçirdim, plan ve kontrol haritalama hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [FedRAMP Yüksek planı - Genel Bakış](./index.md)
> [FedRAMP Yüksek planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.