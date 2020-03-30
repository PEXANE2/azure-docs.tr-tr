---
title: İş ve görev çıktısını veri deposunda kalıcı olarak saklı taçıklayın - Azure Toplu İş
description: Toplu işlerden ve işlerden çıktı verilerini kalıcı olarak sürdürmek için farklı seçenekler hakkında bilgi edinin. Verileri Azure Depolama'da veya başka bir veri deposunda sürebilirsiniz.
services: batch
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/14/2018
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0809a838f1d34491eb4e276ce356eded9b98756e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022843"
---
# <a name="persist-job-and-task-output"></a>İş ve görev çıktılarını kalıcı hale getirme

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev çıktısının bazı yaygın örnekleri şunlardır:

- Görev giriş verilerini işlerken oluşturulan dosyalar.
- Görev yürütmeyle ilişkili dosyaları günlüğe kaydedin.

Bu makalede, görev çıktısını kalıcı olarak sürdürmek için çeşitli seçenekler açıklanmaktadır.

## <a name="options-for-persisting-output"></a>Kalıcı çıktı seçenekleri

Senaryonuza bağlı olarak, görev çıktısını kalıcı olarak sürdürmek için kullanabileceğiniz birkaç farklı yaklaşım vardır:

- [Toplu Hizmet API'sini kullanın.](batch-task-output-files.md)  
- [.NET için Toplu Dosya Kuralları kitaplığını kullanın.](batch-task-output-file-conventions.md)  
- Uygulamanızda Toplu Dosya Kuralları standardını uygulayın.
- Özel bir dosya hareketi çözümü uygulayın.

Aşağıdaki bölümlerde, her yaklaşımın yanı sıra kalıcı çıktı için genel tasarım konuları kısaca açıklayınız.

### <a name="use-the-batch-service-api"></a>Toplu Iş hizmeti API'sini kullanma

Toplu İşlem hizmeti, [bir göreve görev eklediğinizde](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) veya bir [göreve görev koleksiyonu eklediğinizde](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job)görev verileri için Azure Depolama'da çıktı dosyalarını belirtmeyi destekler.

Toplu Iş hizmeti API'si ile görev çıktısını kalıcı hale getirmek hakkında daha fazla bilgi için, [Toplu Iş hizmeti API'si ile Azure Depolama'da görev verilerini sataş'a](batch-task-output-files.md)bakın.

### <a name="use-the-batch-file-conventions-library-for-net"></a>.NET için Toplu Dosya Kuralları kitaplığını kullanma

Toplu İşlem, Görev Çıktısı dosyalarını Azure Depolama'da adlandırmak için isteğe bağlı bir kural kümesi tanımlar. [Toplu Dosya Kuralları standardı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions) bu sözleşmeleri açıklar. Dosya Kuralları standardı, iş ve görev adlarını temel alan belirli bir çıktı dosyası için Azure Depolama'daki hedef kapsayıcı ve blob yolunun adlarını belirler.

Çıktı veri dosyalarınızı adlandırmak için Dosya Kuralları standardını kullanmaya karar verip vermeniz size kalmış. Ayrıca hedef konteyner ve blob istediğiniz şekilde adlandırabilirsiniz. Çıktı dosyalarını adlandırmak için Dosya Kuralları standardını kullanıyorsanız, çıktı dosyalarınız [Azure portalında][portal]görüntülenmek üzere kullanılabilir.

C# ve .NET ile Toplu Iş çözümleri oluşturan geliştiriciler, [Toplu Dosya Kuralları standardına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions)göre görev verilerini bir Azure Depolama hesabında kalıcı olarak sağlamak için [.NET için Dosya Kuralları kitaplığını][nuget_package] kullanabilir. Dosya Kuralları kitaplığı, çıktı dosyalarını Azure Depolama'ya taşımayı ve hedef kapsayıcıları ve lekeleri iyi bilinen bir şekilde adlandırmayı işler.

.NET için Dosya Kuralları kitaplığıyla görev çıktısını kalıcı hale getirmek hakkında daha fazla bilgi [için, .NET için Toplu Dosya Kuralları kitaplığıyla Azure Depolama'ya devam iş ve görev verilerine](batch-task-output-file-conventions.md)bakın.

### <a name="implement-the-batch-file-conventions-standard"></a>Toplu Dosya Kuralları standardını uygulayın

.NET dışında bir dil kullanıyorsanız, toplu [dosya kuralları standardını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions) kendi uygulamanızda uygulayabilirsiniz.

Kanıtlanmış bir adlandırma düzeni istediğinizde veya Azure portalında görev çıktısını görüntülemek istediğinizde Dosya Kuralları adlandırma standardını kendiniz uygulamak isteyebilirsiniz.

### <a name="implement-a-custom-file-movement-solution"></a>Özel bir dosya hareketi çözümü uygulama

Kendi tam dosya hareketi çözümünüzü de uygulayabilirsiniz. Şu anda bu yaklaşımı kullanın:

- Görev verilerini Azure Depolama dışında bir veri deposunda kalıcı olarak sürdürmek istiyorsunuz. Azure SQL veya Azure DataLake gibi bir veri deposuna dosya yüklemek için özel bir komut dosyası oluşturabilir veya bu konuma yüklemek için çalıştırılabilir. Daha sonra birincil yürütülebilir çalıştırdıktan sonra komut satırında arayabilirsiniz. Örneğin, bir Windows düğümünde şu iki komut çağırabilirsiniz:`doMyWork.exe && uploadMyFilesToSql.exe`
- İlk sonuçların denetim işaretlemasını veya erken yüklenmesini gerçekleştirmek istiyorsunuz.
- Hata işleme üzerinde parçalı denetimi korumak istiyorsunuz. Örneğin, belirli görev çıkış kodlarını temel alan belirli yükleme eylemlerini yapmak için görev bağımlılık eylemlerini kullanmak istiyorsanız, kendi çözümünüzü uygulamak isteyebilirsiniz. Görev bağımlılık eylemleri hakkında daha fazla bilgi için bkz: [Diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluştur.](batch-task-dependencies.md)

## <a name="design-considerations-for-persisting-output"></a>Kalıcı çıktı için tasarım konuları

Toplu İşlem çözümünüzü tasarlarken, iş ve görev çıktıları ile ilgili aşağıdaki faktörleri göz önünde bulundurun.

- **İşlem düğümü ömrü**: İşlem düğümleri özellikle otomatik ölçek etkin havuzlarda genellikle geçicidir. Düğümüzerinde çalışan bir görevden çıktı, yalnızca düğüm varken ve yalnızca görev için ayarladığınız dosya bekletme süresi içinde kullanılabilir. Görev tamamlandıktan sonra gerekli olabilecek çıktı üretirse, görevin çıktı dosyalarını Azure Depolama gibi dayanıklı bir depoya yüklemesi gerekir.

- **Çıktı depolama**: Azure Depolama görev çıktısı için veri deposu olarak önerilir, ancak herhangi bir dayanıklı depolama kullanabilirsiniz. Görev çıktısını Azure Depolama'ya yazma, Toplu Iş hizmeti API'sine entegre edilmiştir. Başka bir dayanıklı depolama biçimi kullanıyorsanız, görev çıktısını kendiniz sürdürmek için uygulama mantığını yazmanız gerekir.

- **Çıktı alma**: Görev çıktısını doğrudan havuzunuzdaki bilgi işlem düğümlerinden veya görev çıktısında ısrar ettiyseniz Azure Depolama'dan veya başka bir veri deposundan alabilirsiniz. Bir görevin çıktısını doğrudan bir işlem düğümünden almak için, dosya adı ve düğümüzerindeki çıktı konumugerekir. Görev çıktısını Azure Depolama'da kalıcı olarak sürdürüyorsanız, çıktı dosyalarını Azure Depolama SDK ile indirmek için Azure Depolama'daki dosyaya tam yol gerekir.

- **Çıktıyı Görüntüleme**: Azure portalında toplu iş görevine gidince ve **düğümüzerindeki Dosyalar'ı**seçtiğinizde, yalnızca ilgilendiğiniz çıktı dosyalarıyla değil, görevle ilişkili tüm dosyalarla birlikte sunulur. Yine, işlem düğümlerindeki dosyalar yalnızca düğüm varken ve yalnızca görev için ayarladığınız dosya bekletme süresi içinde kullanılabilir. Azure Depolama'da kalıcı olarak sürediğiniz görev çıktısını görüntülemek için Azure portalını veya [Azure Depolama Gezgini][storage_explorer]gibi bir Azure Depolama istemcisi uygulamasını kullanabilirsiniz. Çıktı verilerini Azure Depolama'da portal veya başka bir araçla görüntülemek için dosyanın konumunu bilmeniz ve doğrudan dosyaya yönlendirmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Toplu Iş hizmeti API'si ile Azure [Depolama'da görev verilerini kalıcı](batch-task-output-files.md)olarak sürdürmek için Toplu Iş hizmeti API'sindeki yeni özellikleri kullanarak keşfedin.
- .NET için Toplu [Dosya Kuralları kitaplığı ile Azure Depolama'da Kalıcı iş ve görev verilerinde](batch-task-output-file-conventions.md).NET için Toplu Dosya Kuralları kitaplığını kullanma hakkında bilgi edinin.
- Görev çıktısını kalıcı depolamaya devam etmek için .NET için Toplu istemci kitaplığı ve .NET için Dosya Kuralları kitaplığı nasıl kullanılacağını gösteren GitHub'daki [PersistOutputs][github_persistoutputs] örnek projesine bakın.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: https://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 
