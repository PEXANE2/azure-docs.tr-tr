---
title: Oluşturma özellikleri - Azure Toplu İş
description: İş yüklerini ve uygulamaları çalıştırmak için Standart Azure Toplu İş özellikleri kullanılır. Toplu iş yükünü oluşturmayı desteklemek için belirli özellikler içerir.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 697e2640b7215e0bbb9202c672f936535831eb99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75449719"
---
# <a name="azure-batch-rendering-capabilities"></a>Azure Toplu İşlem oluşturma özellikleri

İş yük lerini ve uygulamalarını çalıştırmak için Standart Azure Toplu İş özellikleri kullanılır. Toplu iş yükünü oluşturmayı desteklemek için belirli özellikler de içerir.

Havuzlar, işler ve görevler de dahil olmak üzere Toplu Iş kavramlarına genel bakış için [bu makaleye](https://docs.microsoft.com/azure/batch/batch-api-basics)bakın.

## <a name="batch-pools"></a>Toplu Havuzlar

### <a name="rendering-application-installation"></a>Uygulama yüklemesi oluşturma

Yalnızca önceden yüklenmiş uygulamaların kullanılması gerekiyorsa, havuz yapılandırmasında VM görüntüsü işleyen bir Azure Marketi belirtilebilir.

Windows 2016 görüntüsü ve CentOS görüntüsü vardır.  [Azure](https://azuremarketplace.microsoft.com)Marketi'nde, VM görüntüleri 'toplu işleme' arayarak bulunabilir.

Örneğin havuz yapılandırması için [Azure CLI oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)öğreticisi'ne bakın.  Azure portalı ve Toplu Gezgin, havuz oluştururken görüntü leyen bir VM görüntüsünü seçmek için GUI araçları sağlar.  Toplu İş API kullanıyorsanız, havuz oluştururken [ImageReference](https://docs.microsoft.com/rest/api/batchservice/pool/add#imagereference) için aşağıdaki özellik değerlerini belirtin:

| Yayımcı | Sunduğu | Sku | Sürüm |
|---------|---------|---------|--------|
| toplu iş | render-centos73 | Işleme | en son |
| toplu iş | render-windows2016 | Işleme | en son |

Havuz VM'lerinde ek uygulamalar gerekiyorsa diğer seçenekler kullanılabilir:

* Paylaşılan Resim Galerisi'nden özel bir resim:
  * Bu seçeneği kullanarak VM'nizi ihtiyacınız olan uygulamalar ve sürümlerle yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Paylaşılan Resim Galerisi ile havuz oluştur.](batch-sig-images.md) Autodesk ve Chaos Group, Bir Azure Toplu Lisans hizmetine karşı doğrulamak için sırasıyla Arnold ve V-Ray'i değiştirdi. Bu destekle bu uygulamaların sürümlerine sahip olduğunuzdan emin olun, aksi takdirde kullanım başına ödeme lisanslama çalışmaz. Maya veya 3ds Max'in geçerli sürümleri, başsız çalışırken (toplu/komut satırı modunda) bir lisans sunucusu gerektirmez. Bu seçeneği nasıl sürdüreceğinden emin değilseniz Azure desteğine başvurun.
* [Uygulama paketleri](https://docs.microsoft.com/azure/batch/batch-application-packages):
  * Bir veya daha fazla ZIP dosyasını kullanarak uygulama dosyalarını paketleyin, Azure portalı üzerinden yükleyin ve paketi havuz yapılandırmasında belirtin. Havuz VM'leri oluşturulduğunda, ZIP dosyaları indirilir ve dosyalar ayıklanır.
* Kaynak dosyaları:
  * Uygulama dosyaları Azure blob depolamaya yüklenir ve [havuz başlangıç görevinde](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)dosya başvurularını belirtirsiniz. Havuz VM'leri oluşturulduğunda, kaynak dosyaları her VM'ye karşı yüklenir.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Önceden yüklenmiş uygulamalar için kullanım için ödeme lisanslama

Kullanılacak ve lisans ücreti olan uygulamaların havuz yapılandırmasında belirtilmesi gerekir.

* Havuz `applicationLicenses` [oluştururken](https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body)özelliği belirtin.  "vray", "arnold", "3dsmax", "maya" dizelerinde aşağıdaki değerler belirtilebilir.
* Bir veya daha fazla uygulama belirttiğiniz zaman, bu uygulamaların maliyeti VM'lerin maliyetine eklenir.  Uygulama fiyatları Azure [Toplu Toplu fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering)listelenir.

> [!NOTE]
> Bunun yerine, işleme uygulamalarını kullanmak üzere bir lisans sunucusuna `applicationLicenses` bağlanırsanız, özelliği belirtmeyin.

Uygulamaları seçmek ve uygulama fiyatlarını göstermek için Azure portalını veya Toplu Gezgini'ni kullanabilirsiniz.

Bir uygulamayı kullanmaya çalışılırsa, ancak uygulama havuz yapılandırmasının `applicationLicenses` özelliğinde belirtilmemişse veya bir lisans sunucusuna ulaşamıyorsa, uygulama yürütmesi bir lisans hatası ve sıfır olmayan çıkış koduyla başarısız olur.

### <a name="environment-variables-for-pre-installed-applications"></a>Önceden yüklenmiş uygulamalar için ortam değişkenleri

Görevleri işlemek için komut satırı oluşturabilmek için, işleme uygulaması yürütülebilirlerinin yükleme konumu belirtilmelidir.  Azure Marketi VM görüntülerinde, gerçek yolları belirtmek yerine kullanılabilen sistem ortamı değişkenleri oluşturuldu.  Bu ortam değişkenleri, her görev için oluşturulan [standart Toplu iş ortamı değişkenlerine](https://docs.microsoft.com/azure/batch/batch-compute-node-environment-variables) ek olarak yapılır.

|Uygulama|Uygulama Çalıştırılabilir|Çevre Değişkeni|
|---------|---------|---------|
|Autodesk 3ds Max 2018|3dsmaxcmdio.exe|3DSMAX_2018_EXEC|
|Autodesk 3ds Max 2019|3dsmaxcmdio.exe|3DSMAX_2019_EXEC|
|Autodesk Maya 2017|render.exe|MAYA_2017_EXEC|
|Autodesk Maya 2018|render.exe|MAYA_2018_EXEC|
|Kaos Grubu V-Ray Bağımsız|vray.exe|VRAY_3.60.4_EXEC|
Arnold 2017 komuta hattı|kick.exe|ARNOLD_2017_EXEC|
|Arnold 2018 komuta satırı|kick.exe|ARNOLD_2018_EXEC|
|Blender|blender.exe|BLENDER_2018_EXEC|

### <a name="azure-vm-families"></a>Azure VM aileleri

Diğer iş yüklerinde olduğu gibi, uygulama sistemi gereksinimlerini işleme, iş ve projeler için performans gereksinimleri farklılık gösterir.  Gereksinimlerinize bağlı olarak Azure'da çok çeşitli VM aileleri mevcuttur – en düşük maliyet, en iyi fiyat/performans, en iyi performans, ve benzeri.
Arnold gibi bazı işleme uygulamaları CPU tabanlıdır; V-Ray ve Blender Döngüleri gibi diğerleri CPU'ları ve/veya GPU'ları kullanabilir.
Kullanılabilir VM ailelerinin ve VM boyutlarının açıklaması [için VM türleri ve boyutlarına bakın.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)

### <a name="low-priority-vms"></a>Düşük öncelikli sanal makineler

Diğer iş yüklerinde olduğu gibi, işleme için Toplu Iş havuzlarında düşük öncelikli VM'lerden kullanılabilir.  Düşük öncelikli VM'ler, normal özel VM'ler ile aynı performansı gösterir, ancak fazla Azure kapasitesini kullanır ve büyük bir indirim için kullanılabilir.  Düşük öncelikli VM'lerin kullanılmasının amacı, bu VM'lerin kullanılabilir kapasiteye bağlı olarak herhangi bir zamanda tahsis edilemeyebilir veya önceden engellenemeyebilir. Bu nedenle, düşük öncelikli VM'ler tüm işleme işleri için uygun olmayacaktır. Örneğin, görüntülerin işlenmesi saatler sürüyorsa, vm'lerin önceden belirtilmesi nedeniyle bu görüntülerin işlenmesinin kesintiye uğraması ve yeniden başlatılması kabul edilemez.

Düşük öncelikli VM'lerin özellikleri ve Toplu İş'i kullanarak yapılandırmanın çeşitli yolları hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/batch/batch-low-pri-vms)

## <a name="jobs-and-tasks"></a>İş ve görevler

İşler ve görevler için işlemeye özgü destek gerekmez.  Ana yapılandırma öğesi, gerekli uygulamaya başvurması gereken görev komut satırıdır.
Azure Marketi VM görüntüleri kullanıldığında, en iyi yöntem, yürütülebilir yolu ve uygulamayı belirtmek için ortam değişkenlerini kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme örnekleri için iki öğreticideneyin:

* [Azure CLI'yi kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Batch Explorer'ı kullanarak işleme](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
