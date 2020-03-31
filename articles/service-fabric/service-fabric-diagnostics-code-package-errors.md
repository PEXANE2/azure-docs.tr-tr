---
title: Service Fabric kullanarak sık karşılaşılan kod paketi hatalarını tanılama
description: Azure Hizmet Kumaşı ile sık karşılaşılan kod paketi hatalarını nasıl gidereceklerini öğrenin
author: grzuber
ms.topic: article
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 344fef70522240da2236a020c96308c472c9c545
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463105"
---
# <a name="diagnose-common-code-package-errors-by-using-service-fabric"></a>Service Fabric kullanarak sık karşılaşılan kod paketi hatalarını tanılama

Bu makalede, bir kod paketinin beklenmedik şekilde sonlandırılmasının ne anlama geldiğini açıklanır. Sorun giderme adımlarıyla birlikte yaygın hata kodlarının olası nedenleri hakkında bilgi sağlar.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Bir işlem veya kapsayıcı ne zaman beklenmedik bir şekilde sonlandırır?

Azure Hizmet Kumaşı bir kod paketi başlatmak için bir istek aldığında, Uygulama ve Hizmet bildirimlerinde ayarlanan seçeneklere göre yerel sistemdeki ortamı hazırlamaya başlar. Bu hazırlıklar, ağ uç noktalarını veya kaynaklarını ayırmayı, güvenlik duvarı kurallarını yapılandırmayı veya kaynak yönetimi kısıtlamaları ayarlamayı içerebilir. 

Ortam düzgün bir şekilde yapılandırıldıktan sonra, Service Fabric kod paketini getirmeye çalışır. İşletim sistemi veya kapsayıcı çalışma zamanı, işlemin veya kapsayıcının başarıyla etkinleştirildiğini bildirirse, bu adım başarılı olarak kabul edilir. Etkinleştirme başarısız olursa, SFX'te aşağıdakilere benzeyen bir sistem durumu iletisi görmeniz gerekir:

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Kod paketi başarıyla etkinleştirildikten sonra, Service Fabric ömrünü izlemeye başlar. Bu noktada, bir işlem veya kapsayıcı birkaç nedenden dolayı herhangi bir zamanda sonlandırılabilir. Örneğin, bir DLL'yi başlatmayı başaramamış olabilir veya işletim sistemi masaüstü yığın alanı tükenebilir. Kod paketiniz sonlandırıldıysa, SFX'te aşağıdaki sağlık iletisini görmeniz gerekir:

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

Bu sistem durumu iletisindeki çıkış kodu, işlemin veya kapsayıcının neden sonlandırıldığının tek ipucudur. Yığının herhangi bir düzeyi tarafından oluşturulabilir. Örneğin, bu çıkış kodu bir işletim sistemi hatası veya bir .NET sorunuyla ilişkili olabilir veya kodunuz tarafından yükseltilmiş olabilir. Bu makaleyi, sonlandırma çıkış kodlarının kaynağını ve olası çözümleri tanılamaiçin bir başlangıç noktası olarak kullanın. Ancak bunların yaygın senaryolara genel çözümler olduğunu ve gördüğünüz hata için geçerli olmayabilir.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Service Fabric'in kod paketimi sonlandırıp sonlandırmadığını nasıl anlayabilirim?

Service Fabric, çeşitli nedenlerle kod paketinizi sonlandırmaktan sorumlu olabilir. Örneğin, kod paketini yük dengeleme amacıyla başka bir düğüme yerleştirmeye karar verebilir. Aşağıdaki tabloda çıkış kodlarından birini görürseniz, Hizmet Kumaşı'nın kod paketinizi sonlandırdığını doğrulayabilirsiniz.

>[!NOTE]
> İşleminiz veya kapsayıcınız aşağıdaki tablodaki kodlar dışında bir çıkış koduyla sonlanırsa, Servis Kumaşı sonlandırmaişleminden sorumlu değildir.

Çıkış kodu | Açıklama
--------- | -----------
7147 | Service Fabric'in Ctrl+C sinyali göndererek işlemi veya kapsayıcıyı zarif bir şekilde kapattığını gösterir.
7148 | Hizmet Kumaşı'nın işlemi veya kapsayıcıyı sonlandırdığını gösterir. Bazen, bu hata kodu, ctrl+C sinyali gönderdikten sonra işlemin veya kapsayıcının zamanında yanıt vermediğini ve sonlandırılması gerektiğini gösterir.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Diğer yaygın hata kodları ve bunların olası düzeltmeleri

Çıkış kodu | Hexadecimal değer | Kısa açıklama | Kök neden | Olası düzeltme
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Bu hata bazen makinenin masaüstü yığın alanı tükendi anlamına gelir. Bu nedenle, özellikle düğüm üzerinde çalışan uygulamanız ait çok sayıda işlem varsa muhtemeldir. | Programınız Ctrl+C sinyallerine yanıt verecek şekilde oluşturulmadıysa, Küme bildiriminde **EtkinleştirmePenceresi** ayarını etkinleştirebilirsiniz. Bu ayarı etkinleştirmek, kod paketinizin GUI penceresi olmadan çalışacağı ve Ctrl+C sinyalleri alamayacağı anlamına gelir. Bu eylem, her işlemin tükettiği masaüstü yığın alanı miktarını da azaltır. Kod paketinizin Ctrl+C sinyallerini alması gerekiyorsa, düğümünüzün masaüstü yığınının boyutunu artırabilirsiniz.
3762504530 | 0xe0434352 | Yok | Bu değer, yönetilen koddan (yani .NET) işlenmemiş bir özel durum için hata kodunu temsil eder. | Bu çıkış kodu, uygulamanızın işlenmemiş ve işlemi sonlandıran bir özel durum ortaya çıkardığını gösterir. Bu hatayı neyin tetiklediğini belirlemenin ilk adımı olarak, uygulamanızın günlüklerini ayıklave dosyalarını boşaltın.

## <a name="next-steps"></a>Sonraki adımlar

* [Diğer yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)hakkında daha fazla bilgi edinin.
* Azure Monitor günlüklerine ve sunduklarına daha ayrıntılı bir genel bakış alın ve [Azure Monitor'a genel bakışı](../operations-management-suite/operations-management-suite-overview.md)okuyarak neler sunduklarını elde edin.
* Algılama ve tanılama konusunda yardım için [uyarı sağlayan](../log-analytics/log-analytics-alerts.md) Azure Monitor günlükleri hakkında daha fazla bilgi edinin.
* Azure Monitor günlüklerinin bir parçası olarak sunulan [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özelliklerini yakından bilin.
