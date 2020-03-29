---
title: Genel bakış oluşturma - Azure Toplu İş
description: İşleme için Azure'u kullanma nın tanıtımı ve Azure Toplu İşlem oluşturma özelliklerine genel bakış
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: d4423b22c4c8afea5afa9c7040e081665b17ba87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60774038"
---
# <a name="rendering-using-azure"></a>Azure ile işleme

Render, 3B modelleri alıp 2B görüntülere dönüştürme işlemidir. 3D sahne dosyaları Autodesk 3ds Max, Autodesk Maya ve Blender gibi uygulamalarda yazılmaktadır.  Autodesk Maya, Autodesk Arnold, Chaos Group V-Ray ve Blender Cycles gibi render uygulamaları 2B görüntüler üretir.  Bazen sahne dosyalarından tek görüntüler oluşturulur. Ancak, birden çok görüntüyü modellemek ve işlemek ve bunları bir animasyonda birleştirmek yaygındır.

Render iş yükü, Medya ve Eğlence endüstrisinde özel efektler (VFX) için yoğun olarak kullanılır. İşleme reklamcılık, perakende, petrol ve doğalgaz ve üretim gibi diğer birçok sektörde de kullanılır.

İşleme süreci hesaplama açısından yoğundur; üretmek için birçok kare /görüntü olabilir ve her görüntünün işlenmesi saatler sürebilir.  Bu nedenle oluşturma, birçok işlemeyi paralel olarak çalıştırmak için Azure ve Azure Toplu İş birliğini kaldırabilen mükemmel bir toplu iş yüküdür.

## <a name="why-use-azure-for-rendering"></a>Neden oluşturma için Azure'u kullanıyor?

Birçok nedenden dolayı, oluşturma, Azure ve Azure Toplu İş için mükemmel bir şekilde uygun bir iş yüküdür:

* İş oluşturma işleri, birden çok VM kullanılarak paralel olarak çalıştırılabilen birçok parçaya ayrılabilir:
  * Animasyonlar birçok kareden oluşur ve her kare paralel olarak işlenebilir.  Her kareyi işlemek için ne kadar çok VM kullanılabilirse, tüm kareler ve animasyon o kadar hızlı üretilebilir.
  * Bazı işleme yazılımları, tek çerçevelerin kutucuklar veya dilimler gibi birden çok parçaya ayrılmasını sağlar.  Her parça ayrı ayrı işlenebilir, sonra tüm parçalar tamamlandığında son görüntüye birleştirilebilir.  Ne kadar çok VM kullanılabilirse, bir çerçeve o kadar hızlı işlenebilir.
* Proje oluşturma büyük ölçekli gerektirebilir:
  * Tek tek çerçeveler karmaşık olabilir ve üst düzey donanımda bile işlemek için saatlerce zaman gerektirebilir; animasyonlar yüz binlerce çerçeveden oluşabilir.  Yüksek kaliteli animasyonları makul bir süre içinde işlemek için çok büyük miktarda işlem gerekir.  Bazı durumlarda, 100.000'den fazla çekirdek paralel olarak binlerce kare işlemek için kullanılmıştır.
* Renderprojeleri proje tabanlıdır ve farklı miktarda işlem gerektirir:
  * Gerektiğinde işlem ve depolama kapasitesini ayırın, proje sırasında ki yüke göre yukarı veya aşağı ölçeklendirin ve proje tamamlandığında kaldırın.
  * Tahsis edildiğinde kapasite için ödeme yapmayın, ancak projeler arasında olduğu gibi yük olmadığında ödemeyin.
  * Beklenmeyen değişiklikler nedeniyle patlamalar için hitap; bir projede beklenmeyen değişiklikler varsa ve bu değişikliklerin sıkı bir zamanlamayla işlenmesi gerekiyorsa daha yüksek ölçeklendirin.
* Uygulamaya, iş yüküne ve zaman aralığına göre çok çeşitli donanımlar arasından seçim yapın:
  * Azure'da Toplu Iş ile ayrılabilen ve yönetilebilen çok çeşitli donanımlar vardır.
  * Projeye bağlı olarak, gereksinim en iyi fiyat/performans veya en iyi genel performans olabilir.  Farklı sahneler ve/veya render uygulamaları farklı bellek gereksinimlerine sahip olacaktır.  Bazı işleme uygulamaları en iyi performans veya belirli özellikler için GPU'lerden yararlanabilir. 
* Düşük öncelikli VM'ler maliyetleri düşürür:
  * Düşük öncelikli VM'ler, düzenli isteğe bağlı VM'lere kıyasla büyük bir indirim için kullanılabilir ve bazı iş tipleri için uygundur.
  * Düşük öncelikli VM'ler Azure Toplu İş bölümü tarafından tahsis edilebilir ve Toplu Iş, geniş bir gereksinim kümesini karşılamak için nasıl kullanıldıkları konusunda esneklik sağlar.  Toplu havuzlar, vm türlerinin karışımını her zaman değiştirmek mümkün olduğundan, hem özel hem de düşük öncelikli VM'lerden oluşabilir.

## <a name="options-for-rendering-on-azure"></a>Azure'da oluşturma seçenekleri

İş yüklerini işlemek için kullanılabilecek çeşitli Azure özellikleri vardır.  Hangi yeteneklerin kullanılacağı, varolan ortama ve gereksinimlere bağlıdır.

### <a name="existing-on-premises-rendering-environment-using-a-render-management-application"></a>Render yönetimi uygulamasını kullanarak şirket içi işleme ortamının mevcut

En yaygın durum, PipelineFX Qube, Royal Render veya Thinkbox Deadline gibi bir render yönetimi uygulaması tarafından yönetilen mevcut bir şirket içi render çiftliği nin olmasıdır.  Gereksinim, Azure VM'leri kullanarak şirket içi render farm kapasitesini genişletmektir.

Render yönetim yazılımı nda yerleşik Azure desteği vardır veya Azure desteği ekleyen eklentiler kullanıma sunabiliriz. Desteklenen render yöneticileri ve işlevselliği etkin hakkında daha fazla bilgi [için, render yöneticilerinin kullanımı](https://docs.microsoft.com/azure/batch/batch-rendering-render-managers)yla ilgili makaleye bakın.

### <a name="custom-rendering-workflow"></a>Özel işleme iş akışı

Gereksinim, VM'lerin varolan bir işleme çİftliğini genişletmesidir.  Azure Toplu Toplu Toplu Havuzlar çok sayıda VM tahsis edebilir, düşük öncelikli VM'lerin tam fiyatlı VM'lerle otomatik olarak kullanılmasına ve dinamik olarak otomatik ölçeklendirmesine izin verebilir ve popüler işleme uygulamaları için kullanım için ödemeli lisans lama sağlayabilir.

### <a name="no-existing-render-farm"></a>Varolan render çiftliği yok

İstemci iş istasyonları işleme yapıyor olabilir, ancak render iş yükü artıyor ve yalnızca iş istasyonu kapasitesini kullanmak çok uzun sürüyor.  Azure Toplu İş, hem işleme çiftlik bilgisayarını isteğe bağlı olarak ayırmak hem de render işlerini Azure render çiftliğine planlamak için kullanılabilir.

## <a name="azure-batch-rendering-capabilities"></a>Azure Toplu İşlem oluşturma özellikleri

Azure Toplu İşlem, paralel iş yüklerinin Azure'da çalıştırılmasına izin verir.  Uygulamaların yüklendiği ve çalıştırıldığı çok sayıda VM oluşturulmasını ve yönetilmesini sağlar.  Ayrıca, bu uygulamaların örneklerini çalıştırmak için kapsamlı iş zamanlama yetenekleri sağlayarak, görevlerin VM'lere atanmasını, sıraya alınır, uygulama izleme ve benzeri konularda bilgi sağlar.

Azure Toplu İşlem birçok iş yükü için kullanılır, ancak iş yüklerini çalıştırmayı özellikle daha kolay ve hızlı hale getirmek için aşağıdaki özellikler kullanılabilir.

* Önceden yüklenmiş grafiklere ve işleme uygulamalarına sahip VM görüntüleri:
  * Azure Marketplace VM görüntüleri, uygulamaları kendiniz yükleme veya yüklü uygulamalarla kendi özel resimlerinizi oluşturma gereksinimini önleyerek popüler grafikler ve oluşturma uygulamaları içeren kullanılabilir. 
* Uygulamaları işlemek için kullanım başına ödeme li lisanslama:
  * Lisans satın almak ve uygulamalar için bir lisans sunucusu yapılandırmak zorunda kalmamak için bilgi işlem VM'ler için ödeme ek olarak, dakika ile uygulamalar için ödeme seçebilirsiniz.  Kullanım için ödeme yapmak, sabit sayıda lisans olmadığı için değişen ve beklenmeyen yükleri karşılamak da mümkündür.
  * Önceden yüklenmiş uygulamaları kendi lisanslarınızla kullanmak ve kullanım başına ödeme lisansını kullanmamak da mümkündür. Bunu yapmak için genellikle şirket içi veya Azure tabanlı bir lisans sunucusu yükler ve işleme havuzunu lisans sunucusuna bağlamak için bir Azure sanal ağı kullanırsınız.
* İstemci tasarımı ve modelleme uygulamaları için eklentiler:
  * Eklentiler, son kullanıcıların Otomatik Desk Maya gibi doğrudan istemci uygulamasından Azure Toplu İşlemini kullanmasına olanak sağlayarak havuz oluşturmalarına, iş göndermelerine ve daha hızlı işlemeler gerçekleştirmek için daha fazla bilgi işlem kapasitesinden yararlanabilmelerini sağlar.
* Render yöneticisi tümleştirmesi:
  * Azure Toplu İş, render yönetimi uygulamalarına entegre edilmiştir veya Azure Toplu Tümleştirme'yi sağlamak için eklentiler kullanılabilir.

Azure Toplu İş'i kullanmanın birkaç yolu vardır ve bunların tümü Azure Toplu İş oluşturma için de geçerlidir.

* API'ler:
  * [REST,](https://docs.microsoft.com/rest/api/batchservice) [.NET,](https://docs.microsoft.com/dotnet/api/overview/azure/batch) [Python,](https://docs.microsoft.com/python/api/overview/azure/batch) [Java](https://docs.microsoft.com/java/api/overview/azure/batch)veya diğer desteklenen API'leri kullanarak kod yazın.  Geliştiriciler, ister bulut ister şirket içi tabanlı olsun, Azure Toplu İşlem özelliklerini mevcut uygulamalarına veya iş akışlarına entegre edebilir.  Örneğin, [Autodesk Maya eklentisi](https://github.com/Azure/azure-batch-maya) Toplu İş çağırmak, havuzları oluşturma ve yönetmek, iş ve görevler gönderme ve izleme durumunu izlemek için Toplu Python API kullanır.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya Azure [PowerShell](https://docs.microsoft.com/powershell/azure/overview) Toplu Iş komut dosyası kullanmak için kullanılabilir.
  * Özellikle, Toplu CLI şablon desteği havuz oluşturmayı ve iş göndermeyi çok daha kolaylaştırır.
* Uıs:
  * [Toplu İşlem Gezgini,](https://github.com/Azure/BatchExplorer) Toplu İşlem hesaplarının yönetilmesine ve izlenmesine olanak tanıyan, ancak Azure portalı kullanıcı arabirimi yle karşılaştırıldığında bazı daha zengin özellikler sağlayan bir çapraz platform istemci aracıdır.  Desteklenen her uygulama için özel olarak tasarlanmış ve kolayca havuz oluşturmak ve iş göndermek için kullanılabilecek bir havuz ve iş şablonları kümesi sağlanır.
  * Azure portalı, Azure Toplu İş'i yönetmek ve izlemek için kullanılabilir.
* İstemci uygulama eklentileri:
  * Toplu İşleme'nin doğrudan istemci tasarım ve modelleme uygulamaları içinde kullanılmasına olanak tanıyan eklentiler mevcuttur. Eklentiler esas olarak geçerli 3B model hakkında bağlamsal bilgilerle Toplu Gezgin uygulamasını çağırır.
  * Aşağıdaki eklentiler mevcuttur:
    * [Maya için Azure Toplu İş](https://github.com/Azure/azure-batch-maya)
    * [3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
    * [Blender](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)

## <a name="getting-started-with-azure-batch-rendering"></a>Azure Toplu İşlem oluşturma yla başlarken

Azure Toplu İşlem oluşturmayı denemek için aşağıdaki giriş öğreticilerine bakın:

* [Blender sahnesini işlemek için Toplu Gezgin'i kullanma](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
* [Autodesk 3ds Max sahnesini işlemek için Toplu CLI'yi kullanın](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)

## <a name="next-steps"></a>Sonraki adımlar

[Bu makalede,](https://docs.microsoft.com/azure/batch/batch-rendering-applications)Azure Marketi VM görüntülerinde yer alan işleme uygulamalarının ve sürümlerinin listesini belirleyin.