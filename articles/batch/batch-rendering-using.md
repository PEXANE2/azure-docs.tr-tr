---
title: Oluşturma özelliklerini kullanma - Azure Toplu İş
description: Azure Toplu İşlem oluşturma özellikleri nasıl kullanılır? Doğrudan veya istemci uygulama eklentisinden çağrılan Toplu İş Gezgini uygulamasını kullanmayı deneyin.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 03/05/2020
ms.topic: conceptual
ms.openlocfilehash: f3b2e641ab187514a7900b2ab7cc75068df00252
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672003"
---
# <a name="using-azure-batch-rendering"></a>Azure Toplu İşlem oluşturmayı kullanma

Azure Toplu İşlem oluşturmayı kullanmanın birkaç yolu vardır:

* API'ler:
  * Toplu İş API'lerinden herhangi birini kullanarak kod yazın.  Geliştiriciler, ister bulut ister şirket içi tabanlı olsun, Azure Toplu İşlem özelliklerini mevcut uygulamalarına veya iş akışlarına entegre edebilir.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya [PowerShell](https://docs.microsoft.com/powershell/azure/overview) Toplu Iş komut dosyası kullanmak için kullanılabilir.
  * Özellikle, [Toplu CLI şablon desteği](https://docs.microsoft.com/azure/batch/batch-cli-templates) havuz oluşturmayı ve iş göndermeyi çok daha kolaylaştırır.
* Toplu Gezgin UI:
  * [Toplu İşlem Gezgini,](https://github.com/Azure/BatchLabs) Toplu Iş hesaplarının yönetilmesini ve izlenmesini sağlayan bir çapraz platform istemci aracıdır.
  * Görüntüleme uygulamalarının her biri için, havuz oluşturmak ve iş göndermek için kullanılabilecek bir dizi havuz ve iş şablonu sağlanır.  Uygulama Kullanıcı Arabirimi'nde şablon dosyalarına GitHub'dan erişilen şablonlar kümesi listelenir.
  * Özel şablonlar sıfırdan yazılabilir veya GitHub'dan sağlanan şablonlar kopyalanabilir ve değiştirilebilir.
* İstemci uygulama eklentileri:
  * Toplu İşleme'nin doğrudan istemci tasarım ve modelleme uygulamaları içinde kullanılmasına olanak tanıyan eklentiler mevcuttur.  Eklentiler esas olarak geçerli 3B model hakkında bağlamsal bilgilerle Toplu İş Gezgini uygulamasını çağırır ve varlıkların yönetilmesine yardımcı olacak özellikler içerir.

Azure Toplu İşlem oluşturmayı denemenin en iyi yolu ve Azure uzmanı olmayan son kullanıcılar için en basit yol, doğrudan veya bir istemci uygulaması eklentisinden çağrılan Toplu İş Gezgini uygulamasını kullanmaktır.

## <a name="using-batch-explorer"></a>Toplu Gezgini Kullanma

Render gerçekleştirmek için Toplu Explorer'ı kullanmak için adım adım öğretici için [Blender öğreticisine](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)bakın.

### <a name="download-and-install"></a>İndir ve Yükle

Toplu Explorer indirmeleri Windows, OSX ve Linux için [kullanılabilir.](https://azure.github.io/BatchExplorer/)

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Havuz oluşturmak ve işleri çalıştırmak için şablonları kullanma

Toplu İş Gezgini ile birlikte, havuzlar, işler ve görevler oluşturmak için gereken tüm özellikleri belirtmek zorunda kalmadan havuz oluşturmayı ve çeşitli işleme uygulamaları için iş göndermeyi kolaylaştıran kapsamlı bir şablon kümesi kullanılabilir Toplu iş.  Toplu İş Gezgini'nde bulunan şablonlar Bir [GitHub deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj)depolanır ve görünür.

![Toplu Explorer Galerisi](./media/batch-rendering-using/batch-explorer-gallery.png)

PM görüntülerini işleyen Market'te bulunan tüm uygulamalariçin uygun şablonlar sağlanır.  HER uygulama için, CPU ve GPU havuzları, Windows ve Linux havuzları için karşılamak için havuz şablonları da dahil olmak üzere birden çok şablon vardır; iş şablonları tam kare veya karo Blender render ve V-Ray dağıtılmış render içerir. Sağlanan şablonlar kümesi, havuz otomatik ölçekleme gibi diğer Toplu İşlem özelliklerini karşılamak için zaman içinde genişletilir.

Özel şablonların sıfırdan veya sağlanan şablonları değiştirerek üretilmesi de mümkündür. Özel şablonlar, Toplu İş Gezgini'nin 'Galeri' bölümündeki 'Yerel şablonlar' öğesini seçerek kullanılabilir.

### <a name="file-system-and-data-movement"></a>Dosya sistemi ve veri hareketi

Toplu İşlem Gezgini'ndeki 'Veri' bölümü, dosyaların yerel bir dosya sistemi yle Azure Depolama hesapları arasında kopyalanmasına olanak tanır.

## <a name="client-application-plug-ins"></a>İstemci uygulama eklentileri

Eklentiler bazı istemci uygulamaları için kullanılabilir.  Eklentiler havuzların ve işlerin doğrudan uygulamadan oluşturulmasına veya Toplu Gezgin'i çağırmasına olanak sağlar.

* [Blender 2.79](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)
* [Blender 2.8+](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender28)
* [Autodesk 3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
* [Autodesk Maya](https://github.com/Azure/azure-batch-maya)

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme örnekleri için iki öğreticideneyin:

* [Azure CLI'yi kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Batch Explorer'ı kullanarak işleme](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)