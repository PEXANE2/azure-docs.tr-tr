---
title: İşleme yeteneklerini kullanma
description: Azure Batch işleme özelliklerini kullanma. Doğrudan veya bir istemci uygulama eklentisinden çağrılan Batch Explorer uygulamasını kullanmayı deneyin.
author: mscurrell
ms.author: markscu
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: f5a37ebba7dd0622db7e76ecef1c3eda24d72b27
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726409"
---
# <a name="using-azure-batch-rendering"></a>Azure Batch işleme kullanma

Azure Batch işleme kullanmanın birkaç yolu vardır:

* API'ler:
  * Batch API 'Lerinden herhangi birini kullanarak kod yazın.  Geliştiriciler Azure Batch yeteneklerini bulut ya da şirket içinde bulunan mevcut uygulamalarına veya iş akışına tümleştirebilir.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya [PowerShell](https://docs.microsoft.com/powershell/azure/overview) , toplu kullanım betiği için kullanılabilir.
  * Özellikle, [Batch CLI şablonu desteği](https://docs.microsoft.com/azure/batch/batch-cli-templates) havuzları oluşturmayı ve işleri göndermeyi çok daha kolay hale getirir.
* Batch Explorer kullanıcı arabirimi:
  * [Batch Explorer](https://github.com/Azure/BatchLabs) , Batch hesaplarının yönetilmesini ve izlenmesini sağlayan platformlar arası bir istemci aracıdır.
  * İşleme uygulamalarının her biri için, kolayca havuzlar oluşturmak ve işleri göndermek üzere kullanılabilecek bir dizi havuz ve iş şablonu sağlanır.  Şablonlar kümesi, şablon dosyaları GitHub 'dan erişildiği şekilde uygulama kullanıcı arabiriminde listelenir.
  * Özel şablonlar sıfırdan yazılabilir veya GitHub 'dan sağlanan şablonlar kopyalanabilir ve değiştirilebilir.
* İstemci uygulaması eklentileri:
  * Toplu işlemenin doğrudan istemci tasarımı ve modelleme uygulamaları içinden kullanılmasına izin veren eklentiler vardır.  Eklentiler genellikle geçerli 3B modeliyle ilgili bağlamsal bilgilerle Batch Explorer uygulamayı çağırır ve varlıkları yönetmeye yardımcı olacak özellikler içerir.

Geliştirici olmayan ve Azure uzmanlarından sorumlu olmayan son kullanıcılar için Azure Batch işleme ve en basit yolu yapmanın en iyi yolu, doğrudan veya bir istemci uygulaması eklentisiyle çağrılan Batch Explorer uygulamasını kullanmaktır.

## <a name="using-batch-explorer"></a>Batch Explorer kullanma

İşleme gerçekleştirmek için Batch Explorer kullanmaya yönelik adım adım bir öğretici için, bkz. [blender öğreticisi](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender).

### <a name="download-and-install"></a>İndir ve yükle

Batch Explorer [Indirmeleri](https://azure.github.io/BatchExplorer/) Windows, OSX ve Linux 'ta kullanılabilir.

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Havuzlar oluşturmak ve işleri çalıştırmak için şablonları kullanma

Kapsamlı bir şablon kümesi, havuzları, işleri ve görevleri doğrudan Batch ile oluşturmak için gereken tüm özellikleri belirtmek zorunda kalmadan havuzlar oluşturmayı ve çeşitli işleme uygulamaları için iş göndermeyi kolaylaştıran Batch Explorer ile kullanılabilir.  Batch Explorer bulunan şablonlar [bir GitHub deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj)depolanır ve görünür.

![Batch Explorer Galerisi](./media/batch-rendering-using/batch-explorer-gallery.png)

Şablonlar, Market işleme VM görüntülerinde bulunan tüm uygulamalar için karşılamak tarafından sağlanır.  Her uygulama için, CPU ve GPU havuzları, Windows ve Linux havuzları için havuz şablonları dahil olmak üzere birden çok şablon mevcuttur; iş şablonları tam çerçeve veya döşeli blender işleme ve V-Ray dağıtılmış işleme içerir. Sağlanan şablonlar kümesi, havuz otomatik ölçeklendirme gibi diğer toplu Iş özellikleri için zaman içinde genişletilir.

Özel şablonların, sıfırdan veya sağlanan şablonları değiştirerek üretilmesi de mümkündür. Özel şablonlar, Batch Explorer ' Galeri ' bölümündeki ' Yerel Şablonlar ' öğesi seçilerek kullanılabilir.

### <a name="file-system-and-data-movement"></a>Dosya sistemi ve veri taşıma

Batch Explorer 'daki ' Data ' bölümü, dosyaların yerel bir dosya sistemi ve Azure depolama hesapları arasında kopyalanmasını sağlar.

## <a name="client-application-plug-ins"></a>İstemci uygulaması eklentileri

Eklentiler, bazı istemci uygulamaları için kullanılabilir.  Eklentiler, havuzların ve işlerin doğrudan uygulamadan oluşturulmasını sağlar veya Batch Explorer çağırır.

* [Blender 2,79](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)
* [Blender 2.8 +](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender28)
* [Autodesk 3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
* [Autodesk Maya](https://github.com/Azure/azure-batch-maya)

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme örnekleri için iki öğreticiyi deneyin:

* [Azure CLı kullanarak işleme](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Batch Explorer'ı kullanarak işleme](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)