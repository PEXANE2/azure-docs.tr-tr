---
title: Uygulama ve verileri havuz düğümlerine kopyalama
description: Uygulama ve verileri havuz düğümlerine kopyalamayı öğrenin.
ms.topic: article
ms.date: 02/17/2020
ms.openlocfilehash: 700e9b80f8420266c0300b47bdd30bc271f8421c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115593"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>Uygulama ve verileri havuz düğümlerine kopyalama

Azure Batch, veri ve uygulamaların görevler tarafından kullanıma hazır olması için işlem düğümlerine veri ve uygulama almanın çeşitli yollarını destekler. Tüm işleri çalıştırmak için veri ve uygulamalar gerekli olabilir ve bu nedenle her düğümde yüklü olması gerekir. Bazıları yalnızca belirli bir görev için gerekli olabilir veya iş için yüklenmelidir, ancak her düğümde olması gerekmez. Batch, bu senaryoların her biri için araçlara sahiptir.

- **Havuz başlangıç görevi kaynak dosyaları**: havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için. Bir Install komutu gerçekleştirmek için bu yöntemi bir uygulama paketi veya başlangıç görevinin kaynak dosyası koleksiyonuyla birlikte kullanın.  

Örnekler: 
- Uygulamaları taşımak veya yüklemek için başlangıç görevi komut satırını kullanma

- Bir Azure depolama hesabındaki belirli dosya veya kapsayıcıların listesini belirtin. Daha fazla bilgi için bkz. [rest belgelerinde # resourceFile ekleme](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile)

- Havuz üzerinde çalışan her iş, önce MyApplication. msi ile yüklenmesi gereken MyApplication. exe ' yi çalıştırır. Bu mekanizmayı kullanırsanız, başlangıç görevinin **Success için bekle** özelliğini **true**olarak ayarlamanız gerekir. Daha fazla bilgi için [rest belgelerine # startTask ekleme](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)bölümüne bakın.

- Havuzdaki **uygulama paketi başvuruları** : havuzdaki her düğüme yüklenmesi gereken uygulamalar veya veriler için. Uygulama paketiyle ilişkili bir Install komutu yoktur, ancak herhangi bir Install komutunu çalıştırmak için bir başlangıç görevi kullanabilirsiniz. Uygulamanız yükleme gerektirmiyorsa veya çok sayıda dosya içeriyorsa, bu yöntemi kullanabilirsiniz. Uygulama paketleri, çok sayıda dosya başvurularını küçük bir yük içine birleştirdiğinden çok sayıda dosya için idealdir. Tek bir görevde 100 ' den fazla ayrı kaynak dosyası eklemeyi denerseniz, Batch hizmeti tek bir görevde iç sistem sınırlamalarına karşı gelebilir. Ayrıca, aynı uygulamanın birçok farklı sürümüne sahip olabileceğiniz ve aralarında seçim yapmanız gereken durumlarda, uygulama paketlerini kullanın. Daha fazla bilgi için [batch uygulama paketleriyle işlem düğümlerine uygulama dağıtma](https://docs.microsoft.com/azure/batch/batch-application-packages)makalesini okuyun.

- **İş hazırlama görevi kaynak dosyaları**: işin çalışması için yüklenmesi gereken uygulamalar veya veriler için, ancak tüm havuzda yüklü olması gerekmez. Örneğin: havuzunuzun birçok farklı iş türü olması ve yalnızca bir iş türünün çalıştırmak için MyApplication. msi ihtiyacı varsa, yükleme adımını iş hazırlama görevine koymak mantıklı olur. İş hazırlama görevleri hakkında daha fazla bilgi için bkz. [Batch işlem düğümlerinde iş hazırlama ve iş bırakma görevlerini çalıştırma](https://azure.microsoft.com/documentation/articles/batch-job-prep-release/).

- **Görev kaynak dosyaları**: bir uygulama veya veriler yalnızca tek bir görevle ilgili olduğunda. Örneğin: beş göreviniz vardır, her biri farklı bir dosyayı işler ve sonra çıktıyı blob depolamaya yazar.  Bu durumda, her görevin kendi giriş dosyası bulunduğundan, giriş dosyası **Görevler kaynak dosyaları** koleksiyonunda belirtilmelidir.

## <a name="determine-the-scope-required-of-a-file"></a>Bir dosyanın gerekli kapsamını belirleme

Bir dosyanın kapsamını belirlemeniz gerekir-bir havuz, iş veya bir görev için gereken dosyadır. Havuz kapsamındaki dosyalar havuz uygulama paketlerini veya bir başlangıç görevini kullanmalıdır. İş kapsamındaki dosyaların iş hazırlama görevi kullanması gerekir. Havuz veya iş düzeyinde kapsamlı dosyalara yönelik iyi bir örnek, uygulamalardır. Görev kapsamındaki dosyaların görev kaynak dosyalarını kullanması gerekir.

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>Toplu işlem düğümlerine veri almanın diğer yolları

Toplu işlem düğümlerine resmi olarak tümleşik olmayan toplu işlem düğümlerine veri almanın başka yolları vardır REST API. Azure Batch düğümleri üzerinde denetime sahip olduğunuzdan ve özel yürütülebilir dosyalar çalıştırabildiğinden, Batch düğümünün hedefle bağlantısı olduğu ve Azure Batch düğümü üzerinde bu kaynak için kimlik bilgileri olduğu sürece herhangi bir sayıda özel kaynaktan veri çekebilirsiniz. Yaygın olarak karşılaşılan birkaç örnek şunlardır:

- SQL 'den veri yükleme
- Diğer Web hizmetlerinden/özel konumlardan veri yükleme
- Ağ paylaşımından eşleme

### <a name="azure-storage"></a>Azure Storage

BLOB depolama, ölçeklenebilirlik hedeflerini indirir. Azure depolama dosya paylaşma ölçeklenebilirliği hedefleri, tek bir Blobun ile aynıdır. Boyut, ihtiyacınız olan düğüm ve havuzların sayısını etkileyecektir.

