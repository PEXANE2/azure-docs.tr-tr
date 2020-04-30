---
title: İşleme genel bakış-Azure Batch
description: İşleme için Azure 'un kullanılmasına giriş ve Azure Batch işleme özelliklerine genel bakış
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 515fc92aa14c0a86746d0a97d2bc601fab553aa3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115712"
---
# <a name="rendering-using-azure"></a>Azure ile işleme

İşleme, 3B modeller alma ve bunları 2B görüntülere dönüştürme işlemidir. 3B sahne dosyaları, Autodesk 3ds Max, Autodesk Maya ve Blender gibi uygulamalarda yazılır.  Autodesk Maya, Autodesk Arnold, Chaos Group V-Ray ve Blender döngüleri gibi işleme uygulamaları 2B görüntüler üretir.  Sahne dosyalarından bazen tek görüntüler oluşturulur. Ancak, birden çok görüntü modellemek ve işlemek ve sonra bunları bir animasyonda birleştirmek yaygındır.

İşleme iş yükü, medyada ve eğlence sektöründe özel etkiler (VFX) için yoğun bir şekilde kullanılır. İşleme reklamcılık, perakende, petrol ve doğalgaz ve üretim gibi diğer birçok sektörde de kullanılır.

İşleme süreci hesaplama açısından yoğun bir işlemdir; üretilecek birçok çerçeve/görüntü olabilir ve her görüntünün işlenmesi birçok saat sürebilir.  Bu nedenle, çok sayıda oluşturmayı paralel olarak çalıştırmak için Azure ve Azure Batch yararlanan kusursuz bir toplu işlem iş yükü yer alabilir.

## <a name="why-use-azure-for-rendering"></a>Azure neden işleme için kullanılmalıdır?

Birçok nedenden dolayı işleme, Azure ve Azure Batch için mükemmel bir iş yüküdür:

* İşleme işleri, birden çok VM kullanılarak paralel şekilde çalıştırılabilen birçok parçaya ayrılabilir:
  * Animasyonlar birçok kareden oluşur ve her çerçeve paralel olarak oluşturulabilir.  Her çerçeveyi işlemek için kullanılabilir olan sanal makineler, tüm çerçeveler ve animasyon üretilebilirler.
  * Bazı işleme yazılımları, tek çerçevelerin kutucuk veya dilimler gibi birden çok parçaya bölünmelerini sağlar.  Her parça ayrı olarak oluşturulabilir ve tüm parçalar tamamlandığında nihai görüntüde birleştirilir.  Kullanılabilir VM 'Ler daha hızlı bir şekilde oluşturulabilir.
* İşleme projeleri, çok büyük ölçekli bir ölçek gerektirebilir:
  * Tek tek kareler karmaşık olabilir ve yüksek kaliteli donanımda bile işlemek için çok saat gerektirebilir; animasyonlar yüzlerce binlerce kareden oluşabilir.  Yüksek kaliteli animasyonları makul bir süre içinde işlemek için büyük miktarda işlem yapılması gerekir.  Bazı durumlarda, 100.000 çekirdeğin üzerinde çekirdekler paralel olarak binlerce çerçeveyi işlemek için kullanılmıştır.
* Projeler proje tabanlıdır ve değişen miktarlarda işlem gerektirir:
  * Gerektiğinde işlem ve depolama kapasitesi ayırır, bir proje sırasında yüküne göre ölçeği yukarı veya aşağı ölçeklendirin ve proje tamamlandığında kaldırın.
  * Tahsis edildiğinde kapasite için ödeme yapın, ancak projeler arasında bir yük olmadığında bu ücret için ödeme yapmayın.
  * Beklenmedik değişiklikler nedeniyle artışlarıyla için Cater; projede beklenmeyen değişiklikler varsa ve bu değişikliklerin sıkı bir zamanlamaya göre işlenmesi gerekiyorsa daha yüksek ölçeklendirin.
* Uygulamaya, iş yüküne ve zaman çerçevesine göre geniş bir donanım seçimi arasından seçim yapın:
  * Azure 'da kullanılabilen ve Batch ile yönetilebilecek ve yönetilebilen çok sayıda donanım vardır.
  * Projeye bağlı olarak, gereksinim en iyi fiyat/performans veya en iyi genel performans için olabilir.  Farklı sahneler ve/veya işleme uygulamaları farklı bellek gereksinimlerine sahip olur.  Bazı işleme uygulamaları, en iyi performans veya belirli özellikler için GPU 'ları kullanabilir. 
* Düşük öncelikli VM 'Ler maliyetleri azaltır:
  * Düşük öncelikli VM 'Ler, normal isteğe bağlı VM 'lere kıyasla büyük bir indirimle sağlanır ve bazı iş türlerine uygundur.
  * Düşük öncelikli VM 'Ler, Azure Batch tarafından, çok sayıda gereksinim için nasıl kullanıldıklarından daha fazla esneklik sağlayan toplu Işlem ile tahsis edilebilir.  Batch havuzları hem adanmış hem de düşük öncelikli VM 'lerden, her zaman VM türlerinin karışımını değiştirmek mümkün olduğunda olabilir.

## <a name="options-for-rendering-on-azure"></a>Azure 'da işleme seçenekleri

İş yüklerini işlemek için kullanılabilecek bir dizi Azure özelliği vardır.  Kullanılacak yetenekler, var olan herhangi bir ortama ve gereksinimlere bağlıdır.

### <a name="existing-on-premises-rendering-environment-using-a-render-management-application"></a>Bir işleme yönetimi uygulaması kullanarak mevcut şirket içi işleme ortamı

En yaygın durum,, PipelineFX Quya, Royal render veya Thinkbox son tarihi gibi bir işleme yönetimi uygulaması tarafından yönetilmekte olan mevcut bir şirket içi oluşturma grubu olması içindir.  Gereksinim, Azure VM 'Leri kullanarak şirket içi işleme grubu kapasitesini genişletmelidir.

İşleme yönetimi yazılımının Azure desteği yerleşik olarak bulunur veya Azure desteği ekleyen kullanılabilir eklentiler sunuyoruz. Desteklenen oluşturma yöneticileri ve işlevselliği hakkında daha fazla bilgi için, [oluşturma yöneticileri kullanma](https://docs.microsoft.com/azure/batch/batch-rendering-render-managers)makalesine bakın.

### <a name="custom-rendering-workflow"></a>Özel işleme iş akışı

VM, var olan bir oluşturma grubunu genişletmek için gerekli.  Azure Batch havuzları çok sayıda VM ayırabilir, düşük öncelikli VM 'lerin kullanılmasına ve tam fiyatlı VM 'lerle dinamik olarak ölçeklendirilmesine izin verebilir ve popüler işleme uygulamaları için kullanım için ödeme lisansı sağlar.

### <a name="no-existing-render-farm"></a>Mevcut işleme grubu yok

İstemci iş istasyonları işleme gerçekleştiriyor olabilir, ancak işleme iş yükü artmakta ve iş istasyonu kapasitesini yalnızca kullanması çok uzun sürüyor.  Azure Batch, hem istek üzerine işleme grubu işlemi ayırmak hem de işleme işlerini Azure oluşturma grubu 'na zamanlamak için kullanılabilir.

## <a name="azure-batch-rendering-capabilities"></a>Azure Batch işleme özellikleri

Azure Batch, paralel iş yüklerinin Azure 'da çalıştırılmasına izin verir.  Uygulamaların yüklendiği ve çalıştırıldığı çok sayıda VM 'nin oluşturulmasını ve yönetilmesini mümkün kılar.  Ayrıca, bu uygulamaların örneklerini çalıştırmak için, VM 'lere, kuyruğa alma, uygulama izlemeye vb. görev atanmasını sağlayan kapsamlı iş zamanlama özellikleri sağlar.

Azure Batch birçok iş yükü için kullanılır, ancak özellikle işleme iş yüklerini çalıştırmak daha kolay ve daha hızlı hale getirmek için aşağıdaki yetenekler mevcuttur.

* Önceden yüklenmiş grafik ve işleme uygulamaları içeren VM görüntüleri:
  * Popüler grafik ve işleme uygulamaları içeren Azure Market VM görüntüleri, uygulamaları kendiniz yükleme veya yüklü uygulamalar ile kendi özel görüntülerinizi oluşturma ihtiyacını ortadan kaldırmaktan kaçınarak kullanılabilir. 
* Uygulama işleme için kullanım başına ödeme lisansı:
  * İşlem VM 'lerinin yanı sıra, lisans satın alma ve uygulamalar için bir lisans sunucusu yapılandırma gereğini ortadan kaldırarak, uygulamalar için bir dakikaya göre ödeme yapmayı tercih edebilirsiniz.  Kullanım için ödeme, sabit sayıda lisans olmadığı için değişen ve beklenmeyen yük için bir kadüme yapılabileceği anlamına gelir.
  * Önceden yüklenmiş uygulamaların kendi lisanslarınızla kullanılması ve kullanım başına ödeme lisansını kullanmamasıyla de mümkün değildir. Bunu yapmak için genellikle şirket içi veya Azure tabanlı bir lisans sunucusu yükler ve işleme havuzunu lisans sunucusuna bağlamak için bir Azure sanal ağı kullanırsınız.
* İstemci tasarımı ve modelleme uygulamaları için Eklentiler:
  * Eklentiler, son kullanıcıların Autodesk Maya gibi istemci uygulamasından doğrudan Azure Batch kullanmasına izin verir. bu sayede, havuz oluşturma, işleri gönderme ve daha hızlı işleme gerçekleştirmek için daha fazla bilgi işlem kapasitesinden yararlanmalarını sağlar.
* Render Manager tümleştirmesi:
  * Azure Batch, Azure Batch tümleştirmesini sağlamak için işleme yönetimi uygulamalarıyla veya eklentileriyle tümleşiktir.

Azure Batch kullanmanın birkaç yolu vardır ve bunların tümü Azure Batch işleme için de geçerlidir.

* API'ler:
  * [Rest](https://docs.microsoft.com/rest/api/batchservice), [.net](https://docs.microsoft.com/dotnet/api/overview/azure/batch), [Python](https://docs.microsoft.com/python/api/overview/azure/batch), [Java](https://docs.microsoft.com/java/api/overview/azure/batch)veya diğer desteklenen API 'leri kullanarak kod yazın.  Geliştiriciler Azure Batch yeteneklerini bulut ya da şirket içinde bulunan mevcut uygulamalarına veya iş akışına tümleştirebilir.  Örneğin, [Autodesk Maya eklentisi](https://github.com/Azure/azure-batch-maya) Batch çağırmak, havuzları oluşturmak ve yönetmek, işleri ve görevleri göndermek ve durumu Izlemek Için Batch Python API 'sini kullanır.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) , Batch kullanımına betik eklemek için kullanılabilir.
  * Özellikle, Batch CLı şablonu desteği havuzları oluşturmayı ve işleri göndermeyi çok daha kolay hale getirir.
* Kullanıcı arabirimleri
  * [Batch Explorer](https://github.com/Azure/BatchExplorer) , Batch hesaplarının yönetilmesine ve izlenmesine izin veren platformlar arası bir istemci aracıdır, ancak Azure Portal Kullanıcı arabirimine kıyasla daha zengin bazı yetenekler sağlar.  Desteklenen her uygulama için uyarlanmış bir havuz ve iş şablonları kümesi sağlanır ve kolayca havuzlar oluşturmak ve işleri göndermek için kullanılabilir.
  * Azure portal, Azure Batch yönetmek ve izlemek için kullanılabilir.
* İstemci uygulaması eklentisi:
  * Toplu işlemenin doğrudan istemci tasarımı ve modelleme uygulamaları içinden kullanılmasına izin veren eklentiler vardır. Eklentiler genellikle geçerli 3B modeliyle ilgili bağlamsal bilgilerle Batch Explorer uygulamayı çağırır.
  * Aşağıdaki eklentiler kullanılabilir:
    * [Maya için Azure Batch](https://github.com/Azure/azure-batch-maya)
    * [3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
    * [Blender](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)

## <a name="getting-started-with-azure-batch-rendering"></a>Azure Batch işlemeye Başlarken

Azure Batch işlemesini denemek için aşağıdaki giriş öğreticilerine bakın:

* [Bir blender sahneyi işlemek için Batch Explorer kullanma](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
* [Autodesk 3ds Max sahnesini işlemek için Batch CLı 'yi kullanma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)

## <a name="next-steps"></a>Sonraki adımlar

[Bu makaledeki](https://docs.microsoft.com/azure/batch/batch-rendering-applications)Azure Marketi VM görüntülerinde bulunan işleme uygulamalarının ve sürümlerinin listesini belirleme.