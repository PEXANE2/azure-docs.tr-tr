---
title: Uygulamaları ve verileri havuz düğümlerine kopyalama
description: Uygulama ve verileri havuz düğümlerine kopyalamayı öğrenin.
ms.topic: how-to
ms.date: 02/18/2021
ms.openlocfilehash: 0109171fd78dc11058daa30bf4604bebc1eeb857
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703656"
---
# <a name="copy-applications-and-data-to-pool-nodes"></a>Uygulamaları ve verileri havuz düğümlerine kopyalama

Azure Batch, görevler tarafından kullanılabilir olmaları için veri ve uygulamaların işlem düğümlerine alınması için çeşitli yollar destekler.

Seçtiğiniz yöntem, dosyanızın veya uygulamanızın kapsamına bağlı olabilir. Tüm işi çalıştırmak için veri ve uygulamalar gerekli olabilir ve bu nedenle her düğümde yüklü olması gerekir. Bazı dosyalar veya uygulamalar yalnızca belirli bir görev için gerekli olabilir. İş için başkalarının yüklenmesi gerekebilir, ancak her düğümde olması gerekmez. Batch, bu senaryoların her biri için araçlara sahiptir.

## <a name="determine-the-scope-required-of-a-file"></a>Bir dosyanın gerekli kapsamını belirleme

Bir dosyanın kapsamını belirlemeniz gerekir-bir havuz, iş veya bir görev için gereken dosyadır. Havuz kapsamındaki dosyalar havuz uygulama paketlerini veya bir başlangıç görevini kullanmalıdır. İş kapsamındaki dosyaların iş hazırlama görevi kullanması gerekir. Havuz veya iş düzeyinde kapsamlı dosyalara yönelik iyi bir örnek, uygulamalardır. Görev kapsamındaki dosyaların görev kaynak dosyalarını kullanması gerekir.

## <a name="pool-start-task-resource-files"></a>Havuz başlangıç görevi kaynak dosyaları

Havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için havuz başlangıç görevi kaynak dosyalarını kullanın. Bir Install komutu gerçekleştirmek için bu yöntemi bir [uygulama paketi](batch-application-packages.md) veya başlangıç görevinin kaynak dosyası koleksiyonuyla birlikte kullanın.  

Örneğin, uygulamaları taşımak veya yüklemek için başlangıç görevi komut satırını kullanabilirsiniz. Ayrıca, bir Azure depolama hesabındaki dosyaların veya kapsayıcıların listesini de belirtebilirsiniz. Daha fazla bilgi için bkz. [rest belgelerinde # ResourceFile ekleme](/rest/api/batchservice/pool/add#resourcefile).

Havuz üzerinde çalışan her iş, önce bir. msi dosyası ile yüklenmesi gereken bir uygulama (. exe) çalıştırıyorsa, başlangıç görevinin başarılı ' i **bekleme** özelliğini **doğru** olarak ayarlamanız gerekir. Daha fazla bilgi için bkz. [rest belgelerinde # StartTask ekleme](/rest/api/batchservice/pool/add#starttask).

## <a name="application-package-references"></a>Uygulama paketi başvuruları

Havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için [uygulama paketlerini](batch-application-packages.md)kullanmayı göz önünde bulundurun. Uygulama paketiyle ilişkili bir Install komutu yoktur, ancak herhangi bir Install komutunu çalıştırmak için bir başlangıç görevi kullanabilirsiniz. Uygulamanız yükleme gerektirmiyorsa veya çok sayıda dosya içeriyorsa, bu yöntemi kullanabilirsiniz.

Uygulama paketleri çok sayıda dosyanız olduğunda faydalıdır, çünkü birçok dosya başvurusu küçük bir yükte birleştirebilirler. Tek bir görevde 100 ' den fazla ayrı kaynak dosyası eklemeyi denerseniz, Batch hizmeti tek bir görevde iç sistem sınırlamalarına karşı gelebilir. Aynı uygulamanın birçok farklı sürümüne sahipseniz ve aralarında seçim yapmanız gerektiğinde uygulama paketleri de kullanışlıdır.

## <a name="job-preparation-task-resource-files"></a>İş hazırlama görevi kaynak dosyaları

İşin çalışması için yüklenmesi gereken, ancak tüm havuza yüklenmesi gerekmeyen uygulamalar veya veriler için, [iş hazırlama görevi kaynak dosyalarını](./batch-job-prep-release.md)kullanmayı göz önünde bulundurun.

Örneğin, havuzunuzun birçok farklı iş türü olması ve yalnızca bir iş türünün çalıştırılabilmesi için bir. msi dosyasına ihtiyacı varsa, yükleme adımını bir iş hazırlama görevine koymak mantıklı olur.

## <a name="task-resource-files"></a>Görev kaynak dosyaları

Görev kaynak dosyaları, uygulamanız veya verileriniz yalnızca tek bir görevle ilgili olduğunda uygundur.

Örneğin, her biri farklı bir dosyayı işleyen ve sonra çıktıyı bu durumda blob depolamaya yazan beş göreviniz olabilir. her görevin kendi giriş dosyası bulunduğundan, giriş dosyası görev kaynak dosyaları koleksiyonunda belirtilmelidir.

## <a name="additional-ways-to-get-data-onto-nodes"></a>Düğümlere veri almanın diğer yolları

Azure Batch düğümleri üzerinde denetime sahip olduğunuzdan ve özel yürütülebilir dosyalar çalıştırabildiğinden, herhangi bir sayıda özel kaynaktan veri çekebilirsiniz. Batch düğümünün hedefle bağlantı olduğundan ve düğümdeki bu kaynak için kimlik bilgilerine sahip olduğunuzdan emin olun.

Toplu Iş düğümlerine veri aktarma yollarına birkaç örnek verilmiştir:

- SQL 'den veri yükleme
- Diğer Web hizmetlerinden/özel konumlardan veri yükleme
- Ağ paylaşımından eşleme

## <a name="azure-storage"></a>Azure depolama

Blob depolaması 'nın ölçeklenebilirlik hedeflerini indirdiğini aklınızda bulundurun. Azure depolama dosya paylaşma ölçeklenebilirliği hedefleri, tek bir Blobun ile aynıdır. Boyut, ihtiyacınız olan düğüm ve havuzların sayısını etkileyecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Batch ile uygulama paketlerini](batch-application-packages.md)kullanma hakkında bilgi edinin.
- [Düğümler ve havuzlarla çalışma](nodes-and-pools.md)hakkında daha fazla bilgi edinin.
