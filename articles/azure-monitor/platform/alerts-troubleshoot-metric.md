---
title: Azure ölçüm uyarıları sorunlarını giderme
description: Azure Izleyici ölçüm uyarıları ve olası çözümlerle ilgili yaygın sorunlar.
author: harelbr
ms.author: harelbr
ms.topic: reference
ms.date: 07/15/2020
ms.subservice: alerts
ms.openlocfilehash: 0d569facb6c2b58222980cfa1488de3b1f5fb60f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515776"
---
# <a name="troubleshooting-problems-in-azure-monitor-metric-alerts"></a>Azure Izleyici ölçüm uyarılarında sorun giderme sorunları 

Bu makalede, Azure Izleyici [ölçüm uyarılarında](alerts-metric-overview.md) sık karşılaşılan sorunlar ve bunların nasıl giderileceği anlatılmaktadır.

İzleme verilerinizde önemli koşullar bulunduğunda Azure Izleyici uyarıları size önceden bildirimde bulunur. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. Uyarı hakkında daha fazla bilgi için bkz. [Microsoft Azure uyarılara genel bakış](alerts-overview.md).

## <a name="metric-alert-should-have-fired-but-didnt"></a>Ölçüm uyarısı tetiklendi ancak yoktu 

Bir ölçüm uyarısının tetiklenmesi gerektiğini ancak Azure portal başlatmadıysanız ve bulunamadığını düşünüyorsanız, aşağıdaki adımları deneyin:

1. **Yapılandırma** -uygun şekilde yapılandırıldığından emin olmak için ölçüm uyarı kuralı yapılandırmasını gözden geçirin:
    - **Toplama türü**, **toplama ayrıntı düzeyi (süre)** ve **eşik değerinin** veya **Duyarlığın** beklenen şekilde yapılandırılıp yapılandırılmadığını denetleyin
    - Dinamik eşikleri kullanan bir uyarı kuralı için, gelişmiş ayarların yapılandırılıp yapılandırılmadığını denetleyin. bu durum, **eşiklerin** nasıl hesaplanacağını etkilemeden önce uyarıları filtreleyebilir ve **verileri yoksayabilir** .

       > [!NOTE] 
       > Dinamik eşikler, etkin hale gelmeden önce en az 3 gün ve 30 ölçüm örneği gerektirir.

2. **Başlatıldı ancak bildirim yok** -tetiklenen uyarıyı bulup [bulacağınızı görmek için tetiklenen uyarılar listesini](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/alertsV2) gözden geçirin. Uyarıyı listede görebilir, ancak bazı eylemleri veya bildirimleriyle ilgili bir sorun varsa daha fazla bilgi için [buraya](./alerts-troubleshoot.md#action-or-notification-on-my-alert-did-not-work-as-expected)bakın.

3. **Zaten etkin** -bir uyarı almayı beklediğiniz ölçüm zaman serisinde zaten başlatılmış bir uyarı olup olmadığını kontrol edin. Ölçüm uyarıları durum bilgisine sahiptir, yani belirli bir ölçüm zaman serisinde bir uyarı tetiklendiğinde, bu zaman serisinde ek uyarıların, sorun artık gözlemlenene kadar tetiklenmeyeceği anlamına gelir. Bu tasarım seçeneği paraziti azaltır. Uyarı koşulu üç ardışık değerlendirme için karşılanmazsa uyarı otomatik olarak çözümlenir.

4. **Kullanılan boyutlar** - [bir ölçüm için boyut değerlerini](./alerts-metric-overview.md#using-dimensions)seçtiyseniz, uyarı kuralı her bir ölçüm zaman serisini (boyut değerlerinin birleşimiyle tanımlanan şekilde) bir eşik ihlali için izler. Ayrıca, toplam ölçüm zaman serisini (Seçili boyutlar olmadan) izlemek için, boyutları seçmeden ölçüm üzerinde ek bir uyarı kuralı yapılandırın.

5. **Toplama ve zaman ayrıntı düzeyi** : ölçümü, ölçüm [grafiklerini](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/metrics)kullanarak görselleştirmeniz durumunda aşağıdakileri doğrulayın:
    * Ölçüm grafiğindeki seçili **toplama** , uyarı kuralınızın **toplama türüyle** aynı
    * Seçilen **zaman ayrıntı düzeyi** , uyarı kuralınızın (ve ' otomatik ' olarak ayarlanmamış) **toplama ayrıntı düzeyi (dönemi)** ile aynıdır.

## <a name="metric-alert-fired-when-it-shouldnt-have"></a>Ölçüm uyarısı, sahip olmaması durumunda tetiklendi

Ölçüm uyarılarınızın tetiklenmemesi, ancak bunu yaptığı düşünüyorsanız, aşağıdaki adımlar sorunu çözmeye yardımcı olabilir.

1. Tetiklenen uyarıyı bulmak için [tetiklenen uyarılar listesini](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/alertsV2) gözden geçirin ve ayrıntılarını görüntülemek için tıklayın. Uyarının tetiklendiği sırada ölçüm grafiğini, **ölçüm değerini**ve **eşik değerini** görmek için **Bu uyarının neden başlatıldığı konusunda** belirtilen bilgileri gözden geçirin.

    > [!NOTE] 
    > Dinamik Eşik koşul türünü kullanıyor ve kullanılan eşiklerin doğru olmadığını düşünüyorsanız lütfen kaş çatma simgesini kullanarak geri bildirim sağlayın. Bu geri bildirim Machine Learning algoritmik araştırmasını etkiler ve gelecekteki algılamaları artırmaya yardımcı olur.

2. Bir ölçüm için birden çok boyut değeri seçtiyseniz **, bir ölçüm** zaman serisinden (boyut değerlerinin birleşimi tarafından tanımlanan) buna ulaştığında uyarı tetiklenir. Ölçüm uyarılarındaki boyutları kullanma hakkında daha fazla bilgi için [buraya](./alerts-metric-overview.md#using-dimensions) bakın.

3. Düzgün yapılandırıldığından emin olmak için uyarı kuralı yapılandırmasını gözden geçirin:
    - **Toplama türü**, **toplama ayrıntı düzeyi (süre)** ve **eşik değerinin** veya **Duyarlığın** beklenen şekilde yapılandırılıp yapılandırılmadığını denetleyin
    - Dinamik eşikleri kullanan bir uyarı kuralı için, gelişmiş ayarların yapılandırılıp yapılandırılmadığını denetleyin. bu durum, **eşiklerin** nasıl hesaplanacağını etkilemeden önce uyarıları filtreleyebilir ve **verileri yoksayabilir** .

   > [!NOTE]
   > Dinamik eşikler, etkin hale gelmeden önce en az 3 gün ve 30 ölçüm örneği gerektirir.

4. Ölçüm [grafiğini](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/metrics)kullanarak ölçümü görselleştirmeniz halinde şunları doğrulayın:
    - Ölçüm grafiğindeki seçili **toplama** , uyarı kuralınızın **toplama türüyle** aynı
    - Seçilen **zaman ayrıntı düzeyi** , uyarı kuralınızın (ve ' otomatik ' olarak ayarlanmamış) **toplama ayrıntı düzeyi (dönemi)** ile aynıdır.

5. Uyarı, aynı ölçütleri izleyen (çözümlenmeyen) uyarılar varken harekete geçirildiğinde, uyarı kuralının *otomatik azaltma* özelliği **false** olarak ayarlanmış şekilde yapılandırılıp yapılandırılmadığını denetleyin (Bu özellik yalnızca Rest/PowerShell/CLI aracılığıyla yapılandırılabilir, bu nedenle uyarı kuralını dağıtmak için kullanılan betiği denetleyin). Böyle bir durumda, uyarı kuralı tetiklenen uyarıları oto çözmez ve yeniden başlatılmadan önce tetiklenen bir uyarının çözülmesini gerektirmez.


## <a name="cant-find-the-metric-to-alert-on---virtual-machines-guest-metrics"></a>Sanal makinelerin Konuk ölçümlerini uyarmak için ölçüm bulunamıyor

Sanal makinelerin (örneğin, bellek, disk alanı) Konuk işletim sistemi ölçümleri hakkında uyarı almak için, bu verileri Azure Izleyici ölçümlerine toplamak üzere gerekli aracıyı yüklediğinizden emin olun:
- [Windows VM'leri için](./collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux VM'leri için](./collect-custom-metrics-linux-telegraf.md)

Bir sanal makinenin Konuk işletim sisteminden veri toplama hakkında daha fazla bilgi için [buraya](../insights/monitor-vm-azure.md#guest-operating-system)bakın.
    
> [!NOTE] 
> Konuk ölçümlerini bir Log Analytics çalışma alanına gönderilmek üzere yapılandırdıysanız, ölçümler Log Analytics çalışma alanı kaynağı altında görünür ve **yalnızca** bunları izleyen bir uyarı kuralı oluşturduktan sonra verileri göstermeye başlayacaktır. Bunu yapmak için, [günlükler için ölçüm uyarısı yapılandırma](./alerts-metric-logs.md#configuring-metric-alert-for-logs) adımlarını izleyin.

## <a name="cant-find-the-metric-to-alert-on"></a>Uyarı almak için ölçüm bulunamıyor

Belirli bir ölçüm üzerinde uyarı almak istiyorsanız ancak kaynak için herhangi bir ölçüm göremiyorsanız, [kaynak türünün ölçüm uyarıları için desteklenip desteklenmediğini denetleyin](./alerts-metric-near-real-time.md).
Kaynak için bazı ölçümler görülebiliyorsanız ancak belirli bir ölçümü bulamıyorsanız, [Bu ölçümün kullanılabilir](./metrics-supported.md)olup olmadığını denetleyin ve varsa, yalnızca belirli sürümlerde veya kaynak sürümlerinde olup olmadığını görmek için ölçüm açıklamasına bakın.

## <a name="cant-find-the-metric-dimension-to-alert-on"></a>Uyarı almak için ölçüm boyutu bulunamıyor

[Bir ölçümün belirli boyut değerlerini](./alerts-metric-overview.md#using-dimensions)uyarmak istiyorsanız ancak bu değerleri bulamazsanız, aşağıdakilere göz önünde bulunmaktadır:

1. Boyut değerlerinin **Boyut değerleri** listesinde görünmesi birkaç dakika sürebilir
1. Görüntülenen boyut değerleri son üç günde toplanan ölçüm verilerini temel alır
1. Boyut değeri henüz yayınlanmazsa, özel değer eklemek için ' + ' işaretine tıklayın
1. Bir boyutun tüm olası değerlerini (gelecekteki değerler dahil) uyarmak isterseniz, ' Seç * ' onay kutusunu işaretleyin

## <a name="metric-alert-rules-still-defined-on-a-deleted-resource"></a>Ölçüm uyarı kuralları silinen bir kaynakta hala tanımlı 

Bir Azure kaynağını sildiğinizde o kaynakla ilişkilendirilmiş olan ölçüm uyarı kuralları otomatik olarak silinmez. Silinmiş bir kaynakla ilişkili uyarı kurallarını silmek için:

1. Silinen kaynağın tanımlandığı kaynak grubunu açın
1. Kaynakların görüntülendiği listede **Gizli türleri göster** onay kutusunu işaretleyin
1. Listeye şu filtreyi uygulayın: Tür == **microsoft.insights/metricalerts**
1. İlgili uyarı kurallarını seçin ve **Sil** ' i seçin.

## <a name="make-metric-alerts-occur-every-time-my-condition-is-met"></a>Koşulumun her karşılanışında ölçüm uyarılarını meydana getirme

Ölçüm uyarıları varsayılan olarak durum bilgisine sahiptir ve bu nedenle, belirli bir zaman serisinde zaten tetiklenen bir uyarı varsa ek uyarılar tetiklenmez. Belirli bir ölçüm uyarısı kuralını durum bilgisiz yapmak ve uyarı koşulunun karşılandığı her değerlendirmede uyarı almak istiyorsanız, uyarı kuralını programlı bir şekilde oluşturun (örneğin, [Kaynak Yöneticisi](./alerts-metric-create-templates.md), [PowerShell](/powershell/module/az.monitor/?view=azps-3.6.1), [rest](/rest/api/monitor/metricalerts/createorupdate), [CLI](/cli/azure/monitor/metrics/alert?view=azure-cli-latest)) ve *oto hafifletmek* özelliğini ' false ' olarak ayarlayın.

> [!NOTE] 
> Ölçüm uyarısı kuralının durum bilgisiz olması, tetiklenen uyarıların çözümlenmesini önler, bu nedenle, koşulun artık karşılanmaması durumunda, başlatılan uyarılar 30 gün Bekletme süresine kadar tetiklenen bir durumda kalır.


## <a name="metric-alert-rules-quota-too-small"></a>Ölçüm uyarı kuralları kotası çok küçük

Abonelik başına izin verilen ölçüm uyarısı kuralı sayısı, [kota sınırlarına](../service-limits.md)tabidir.

Kota sınırına ulaştıysanız aşağıdaki adımlar sorunu çözmenize yardımcı olabilir:
1. Artık kullanılmayan ölçüm uyarısı kurallarını silmeyi veya devre dışı bırakmayı deneyin.

2. Birden fazla kaynağı izleyen ölçüm uyarı kurallarına geçiş yapın. Bu özellik sayesinde, tek bir uyarı kuralı, kotayla sayılan tek bir uyarı kuralını kullanarak birden çok kaynağı izleyebilir. Bu özellik ve desteklenen kaynak türleri hakkında daha fazla bilgi için [buraya](./alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) bakın.

3. Kota sınırının artırılması gerekiyorsa, bir destek isteği açın ve aşağıdaki bilgileri sağlayın:

    - Kota sınırının artırılması gereken abonelik kimlikleri
    - Kota artışı için kaynak türü: **ölçüm uyarıları** veya **ölçüm uyarıları (klasik)**
    - İstenen kota sınırı

## <a name="check-total-number-of-metric-alert-rules"></a>Toplam ölçüm uyarısı kuralı sayısını denetle

Ölçüm uyarı kurallarının geçerli kullanımını denetlemek için aşağıdaki adımları izleyin.

### <a name="from-the-azure-portal"></a>Azure portalından

1. **Uyarılar** ekranını açıp **Uyarı kurallarını yönetin**'e tıklayın
2. **Abonelik** açılan denetimini kullanarak ilgili aboneliğe filtre uygulayın
3. Belirli bir kaynak grubuna, kaynak türüne veya kaynağa filtre uyguladığınızdan emin olun
4. **Sinyal türü** açılan kutusunda **ölçümler** ' i seçin.
5. **Durum** açılan denetiminin **etkin** olarak ayarlandığını doğrulama
6. Ölçüm uyarı kurallarının toplam sayısı, kurallar listesinin üzerinde görüntülenir

### <a name="from-api"></a>API'den

- PowerShell - [Get-AzMetricAlertRuleV2](/powershell/module/az.monitor/get-azmetricalertrulev2?view=azps-3.7.0)
- REST API - [Aboneliğe göre listeleme](/rest/api/monitor/metricalerts/listbysubscription)
- Azure CLI - [az monitor metrics alert list](/cli/azure/monitor/metrics/alert?view=azure-cli-latest#az-monitor-metrics-alert-list)

## <a name="managing-alert-rules-using-resource-manager-templates-rest-api-powershell-or-azure-cli"></a>Kaynak Yöneticisi şablonları, REST API, PowerShell veya Azure CLı kullanarak uyarı kurallarını yönetme

Kaynak Yöneticisi şablonları, REST API, PowerShell veya Azure komut satırı arabirimi (CLı) kullanarak ölçüm uyarıları oluşturma, güncelleştirme, alma veya silme sorunları yaşıyorsanız, aşağıdaki adımlar sorunu çözmeye yardımcı olabilir.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

- [Yaygın Azure dağıtım hataları](../../azure-resource-manager/templates/common-deployment-errors.md) listesini gözden geçirin ve ilgili sorun giderme adımlarını izleyin
- Tüm parametreleri doğru şekilde geçirdiğinizden emin olmak için [ölçüm uyarıları Azure Resource Manager şablon örneklerine](./alerts-metric-create-templates.md) bakın

### <a name="rest-api"></a>REST API

Tüm parametreleri doğru geçirdiğinizi onaylamak için [REST API kılavuzu](/rest/api/monitor/metricalerts/) gözden geçirin

### <a name="powershell"></a>PowerShell

Ölçüm uyarıları için doğru PowerShell cmdlet 'lerini kullandığınızdan emin olun:

- Ölçüm uyarılarına yönelik PowerShell cmdlet’leri [Az.Monitor modülünde](/powershell/module/az.monitor/?view=azps-3.6.1) de sunulur
- Yeni (klasik olmayan) ölçüm uyarıları için ' v2 ' ile biten cmdlet 'leri kullandığınızdan emin olun (örneğin, [Add-AzMetricAlertRuleV2](/powershell/module/az.monitor/add-azmetricalertrulev2?view=azps-3.6.1))

### <a name="azure-cli"></a>Azure CLI’si

Ölçüm uyarıları için doğru CLı komutlarını kullandığınızdan emin olun:

- Ölçümlere yönelik CLI komutları `az monitor metrics alert` ile başlar. Söz dizimi hakkında bilgi edinmek için [Azure CLI başvurusunu](/cli/azure/monitor/metrics/alert?view=azure-cli-latest) gözden geçirin.
- [Ölçüm uyarısı CLI komutlarını kullanmayı gösteren örneğe](./alerts-metric.md#with-azure-cli) bakabilirsiniz
- Özel bir ölçümde uyarı tetiklemek için ölçüm adını ilgili ölçümün alan adına ön ek olarak getirdiğinizden emin olun: NAMESPACE.METRIC

### <a name="general"></a>Genel

- `Metric not found` hatası alıyorsanız:

   - Platform ölçümü için: ölçüm **görünen adı** değil, [Azure izleyici tarafından desteklenen ölçümler sayfasından](./metrics-supported.md) **ölçüm** adını kullandığınızdan emin olun

   - Özel bir ölçüm için: ölçümün zaten yayıldığından (henüz mevcut olmayan bir özel ölçüm üzerinde bir uyarı kuralı oluşturamazsınız) ve özel ölçümün ad alanını sağladığınızdan emin olun ( [burada](./alerts-metric-create-templates.md#template-for-a-static-threshold-metric-alert-that-monitors-a-custom-metric)bir ARM şablonu örneğine bakın)

- [Günlüklerle ilgili ölçüm uyarıları](./alerts-metric-logs.md) oluşturuyorsanız uygun bağımlılıkların dahil edildiğinden emin olun. Bkz. [örnek şablon](./alerts-metric-logs.md#resource-template-for-metric-alerts-for-logs).

- Birden çok ölçüt içeren bir uyarı kuralı oluşturuyorsanız, aşağıdaki kısıtlamalara göz önünde bulabilirsiniz:

   - Her bir ölçüt içinde boyut başına yalnızca bir değer seçebilirsiniz
   - Boyut değeri olarak "\*" kullanamazsınız
   - Farklı criterions ' de yapılandırılan ölçümler aynı boyutu destekledikleri zaman, yapılandırılan bir boyut değeri bu ölçümler için açıkça aynı şekilde ayarlanmalıdır ( [burada](./alerts-metric-create-templates.md#template-for-a-static-threshold-metric-alert-that-monitors-multiple-criteria)bir kaynak yöneticisi şablon örneğine bakın)


## <a name="no-permissions-to-create-metric-alert-rules"></a>Ölçüm uyarı kuralları oluşturmak için izin yok

Ölçüm uyarısı kuralı oluşturmak için aşağıdaki izinlere sahip olmanız gerekir:

- Uyarı kuralının hedef kaynağında Oku izni
- Uyarı kuralının oluşturulduğu kaynak grubu üzerinde yazma izni (Azure portal uyarı kuralı oluşturuyorsanız, uyarı kuralı hedef kaynağın bulunduğu aynı kaynak grubunda oluşturulur)
- Uyarı kuralıyla ilişkili tüm eylem grupları üzerinde okuma izni (varsa)


## <a name="naming-restrictions-for-metric-alert-rules"></a>Ölçüm uyarı kuralları için adlandırma kısıtlamaları

Ölçüm uyarı kuralı adları için aşağıdaki kısıtlamalara göz önünde olun:

- Ölçüm uyarısı kural adları, oluşturulduktan sonra değiştirilemez (yeniden adlandırılamaz)
- Ölçüm uyarısı kural adları, bir kaynak grubu içinde benzersiz olmalıdır
- Ölçüm uyarısı kural adları şu karakterleri içeremez: * # & +:  < > ? @ % { } \ / 
- Ölçüm uyarısı kural adları şu karakterle bitemez:.


## <a name="restrictions-when-using-dimensions-in-a-metric-alert-rule-with-multiple-conditions"></a>Birden çok koşula sahip bir ölçüm uyarı kuralında boyutlar kullanırken kısıtlamalar

Ölçüm uyarıları çok boyutlu ölçümler üzerinde uyarı vermeyi ve birden çok koşul tanımlamayı destekler (uyarı kuralı başına 5 koşula kadar).

Birden çok koşul içeren bir uyarı kuralında boyutları kullanırken lütfen aşağıdaki kısıtlamalara göz önünde unutmayın:
1. Her bir koşul içinde yalnızca boyut başına bir değer seçebilirsiniz.
2. "Tüm geçerli ve gelecekteki değerleri Seç" seçeneğini (Select \* ) kullanamazsınız.
3. Farklı koşullarda yapılandırılan ölçümler aynı boyutu destekledikleri zaman, yapılandırılmış bir boyut değerinin tüm bu ölçümler için (ilgili koşullarda) aynı şekilde ayarlanması gerekir.
Örneğin:
    - Bir depolama hesabında tanımlanan ölçüm uyarısı kuralını düşünün ve iki koşulu izler:
        * Toplam **işlem** sayısı > 5
        * Ortalama **SuccessE2ELatency** > 250 MS
    - İlk koşulu güncelleştirmek istiyorum ve yalnızca **Apiname** boyutunun *"GetBlob"* değerine eşit olduğu işlemleri izle
    - Hem **işlemler** hem de **SuccessE2ELatency** ölçümleri bir **apiname** boyutunu destekledikleri için her iki koşulu da güncelleştirmem gerekir ve her ikisine de bir *"GetBlob"* değeri ile **apiname** boyutunu belirtmektir.


## <a name="next-steps"></a>Sonraki adımlar

- Uyarılar ve bildirimler hakkında genel sorun giderme bilgileri için bkz. [Azure izleyici uyarılarında sorun giderme sorunları](alerts-troubleshoot.md).
