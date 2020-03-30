---
title: Uygulamaları ve verileri havuz düğümlerine kopyalama
description: Uygulamaları ve verileri havuz düğümlerine kopyalamayı öğrenin.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.openlocfilehash: 226a0d69ac387142ecf580537e35f8754ac848a6
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385590"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>Uygulamaları ve verileri havuz düğümlerine kopyalama

Azure Toplu İşlem, veri ve uygulamaların görevler tarafından kullanılabilmesi için veri ve uygulamaları bilgi işlem düğümlerine getirmenin çeşitli yollarını destekler. Tüm işi çalıştırmak için veri ve uygulama gerekebilir ve bu nedenle her düğüme yüklenmesi gerekir. Bazıları yalnızca belirli bir görev için gerekli olabilir veya iş için yüklenmesi gerekebilir, ancak her düğümde olması gerekmez. Toplu iş bu senaryoların her biri için araçlara sahiptir.

- **Havuz başlangıç görev kaynağı dosyaları**: Havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için. Yükleme komutu gerçekleştirmek için bir uygulama paketi veya başlangıç görevinin kaynak dosyası koleksiyonuyla birlikte bu yöntemi kullanın.  

Örnekler: 
- Uygulamaları taşımak veya yüklemek için başlangıç görev komut satırını kullanın

- Azure depolama hesabında belirli dosyaların veya kapsayıcıların listesini belirtin. Daha fazla bilgi için [REST belgelerinde add#resourcefile'a](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile) bakın

- Havuzda çalışan her iş, öncelikle MyApplication.msi ile yüklenmesi gereken MyApplication.exe'yi çalıştırAr. Bu mekanizmayı kullanırsanız, başlangıç görevinin başarı özelliğinin **doğru** **olmasını beklemesini** ayarlamanız gerekir. Daha fazla bilgi için [REST belgelerinde add#starttask'a](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)bakın.

- Havuzdaki **uygulama paketi başvuruları:** Havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için. Bir uygulama paketiyle ilişkili yükleme komutu yoktur, ancak herhangi bir yükleme komutunu çalıştırmak için başlangıç görevini kullanabilirsiniz. Uygulamanız yükleme gerektirmiyorsa veya çok sayıda dosyadan oluşuyorsa, bu yöntemi kullanabilirsiniz. Çok sayıda dosya başvurularını küçük bir yükte birleştirdikleri için uygulama paketleri çok sayıda dosya için uygundur. Tek bir göreve 100'den fazla ayrı kaynak dosyası eklemeye çalışırsanız, Toplu İşlem hizmeti tek bir görev için dahili sistem sınırlamaları ile karşılanabilir. Ayrıca, aynı uygulamanın birçok farklı sürümüne sahip olabileceğiniz ve aralarında seçim yapmanız gereken zorlu sürüm gereksinimleriniz varsa uygulama paketlerini kullanın. Daha fazla bilgi [için, Toplu uygulama paketleri yle düğümleri hesaplamak için uygulamaları dağıt'ı](https://docs.microsoft.com/azure/batch/batch-application-packages)okuyun.

- **İş hazırlama görev kaynağı dosyaları**: İşin çalışması için yüklenmesi gereken, ancak tüm havuza yüklenmesi gerekmeyen uygulamalar veya veriler için. Örneğin: havuzunuzda çok sayıda farklı türde iş varsa ve yalnızca bir iş türünün çalışması için MyApplication.msi'ye ihtiyacı varsa, yükleme adımını iş hazırlama görevine koymak mantıklıdır. İş hazırlama görevleri hakkında daha fazla bilgi için, [Toplu işlem düğümlerinde iş hazırlama ve iş bırakma görevlerini çalıştır'a](https://azure.microsoft.com/documentation/articles/batch-job-prep-release/)bakın.

- **Görev kaynağı dosyaları**: Bir uygulama veya veri yalnızca tek bir görevle alakalı olduğunda. Örneğin: Beş göreviniz var, her biri farklı bir dosyayı işler ve çıktıyı blob depolama alanına yazar.  Bu durumda, her görevin kendi giriş dosyası olduğundan, giriş dosyası **görevler kaynak dosyaları** koleksiyonunda belirtilmelidir.

## <a name="determine-the-scope-required-of-a-file"></a>Dosyanın gerekli kapsamını belirleme

Bir dosyanın kapsamını belirlemeniz gerekir - bir havuz, iş veya görev için gereken dosyadır. Havuza giren dosyalar, havuz uygulama paketlerini veya başlangıç görevini kullanmalıdır. İş kapsamına giren dosyalar bir iş hazırlama görevi kullanmalıdır. Havuz veya iş düzeyinde kapsamlı dosyaların iyi bir örneği uygulamalardır. Göreve kapsamlı dosyalar görev kaynağı dosyalarını kullanmalıdır.

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>Toplu işlem düğümlerine veri almanın diğer yolları

Toplu İşlem API'sine resmi olarak entegre olmayan Toplu işlem düğümlerine veri almanın başka yolları da vardır. Azure Toplu İş düğümleri üzerinde denetiminiz olduğundan ve özel yürütülebilir leri çalıştırabildiğinizden, Toplu İşlem düğümü hedefe bağlandığı ve bu kaynağa ait kimlik bilgilerini Azure Toplu İş düğümüne sahip olduğunuz sürece istediğiniz sayıda özel kaynaktan veri çekebilirsiniz. Birkaç yaygın örnek:

- SQL'den veri indirme
- Diğer web hizmetlerinden/özel konumlardan veri indirme
- Ağ paylaşımını eşleme

### <a name="azure-storage"></a>Azure depolama alanı

Blob depolama indirme ölçeklenebilirlik hedefleri vardır. Azure depolama dosyası paylaşımı ölçeklenebilirlik hedefleri tek bir blob için aynıdır. Boyut, ihtiyacınız olan düğüm ve havuz sayısını etkiler.

