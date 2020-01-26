---
title: Azure DevTest Labs bir görüntü fabrikası oluşturun | Microsoft Docs
description: Bu makalede, git deposunda (Azure DevTest Labs) bulunan örnek betikler kullanılarak özel bir görüntü fabrikası ayarlama gösterilmektedir.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 2c5a44a9505d4a312be521cdc3219c5e4ce95a42
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759457"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Azure DevTest Labs özel görüntü fabrikası oluşturma
Bu makalede, [Git deposunda](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)bulunan örnek betikleri kullanarak özel bir görüntü fabrikası ayarlama gösterilmektedir.

## <a name="whats-an-image-factory"></a>Görüntü fabrikası nedir?
Görüntü fabrikası, istenen tüm yapılandırmalara göre düzenli olarak görüntü oluşturup dağıtan bir yapılandırma olarak yapılandırma çözümüdür. Görüntü fabrikasındaki görüntüler her zaman güncel değildir ve işlemin tamamı otomatikleştirildiğinde devam eden bakım neredeyse sıfırdır. Ayrıca, gerekli tüm yapılandırmaların görüntüde zaten bulunduğu için, temel işletim sistemi ile bir VM oluşturulduktan sonra sistemi el ile yapılandırarak saati kaydeder.

Geliştirici masaüstlerini DevTest Labs 'de hazırlama durumuna getirmek için önemli Hızlandırıcı, özel görüntüler kullanmaktır. Özel görüntülerin downi, laboratuvarda devam etmek için ek bir şeydir. Örneğin, ürünlerin deneme sürümleri zaman içinde sona erer (veya) yeni yayınlanan güvenlik güncelleştirmeleri uygulanmaz ve bu, özel görüntüyü düzenli aralıklarla yenilemeye zorlar. Bir görüntü fabrikası ile, kaynak kodu denetimine iade edilen görüntünün bir tanımına sahip olursunuz ve tanımı temel alan özel görüntüler oluşturmak için otomatik bir işlemdir.

Çözüm, diğer devam eden bakım maliyetlerini ortadan kaldırarak özel görüntülerden sanal makine oluşturma hızını sunar. Bu çözümle, otomatik olarak özel görüntüler oluşturabilir, bunları diğer DevTest Labs 'e dağıtabilir ve eski görüntüleri devre dışı bırakabilirsiniz. Aşağıdaki videoda, görüntü fabrikası ve DevTest Labs ile nasıl uygulandığı hakkında bilgi edineceksiniz.  Tüm Azure PowerShell betikleri serbestçe kullanılabilir ve burada yer alır: [https://aka.ms/dtlimagefactory](https://aka.ms/dtlimagefactory).

<br/>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Custom-Image-Factory-with-Azure-DevTest-Labs/player]


## <a name="high-level-view-of-the-solution"></a>Çözümün üst düzey görünümü
Çözüm, diğer devam eden bakım maliyetlerini ortadan kaldırarak özel görüntülerden sanal makine oluşturma hızını sunar. Bu çözümle, otomatik olarak özel görüntüler oluşturabilir ve bunları diğer DevTest Labs 'e dağıtabilirsiniz. DevTest Labs içindeki tüm işlemleri otomatik hale getirmek için düzenleme motoru olarak Azure DevOps (eski adıyla Visual Studio Team Services) kullanın.

![Çözümün üst düzey görünümü](./media/create-image-factory/high-level-view-of-solution.png)

[DevTest Labs için](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) bu bireysel adımları yürütmenizi sağlayan bir VSTS uzantısı vardır:

- Özel görüntü oluşturma
- VM oluşturma
- VM 'yi silme
- Ortam Oluştur
- Ortamı sil
- Ortamı doldur

DevTest Labs uzantısının kullanılması, DevTest Labs 'de otomatik olarak özel görüntüler oluşturmaya başlamak için kolay bir yoldur.

Daha karmaşık bir senaryo için PowerShell betiği kullanan alternatif bir uygulama vardır. PowerShell kullanarak, sürekli tümleştirme ve sürekli teslim (CI/CD) araç zincirinde kullanılabilen DevTest Labs tabanlı bir görüntü fabrikasını tamamen otomatikleştirebileceğinizi unutmayın. Bu alternatif çözümün ardından aşağıdaki ilkeler şunlardır:

- Ortak güncelleştirmelerin görüntü fabrikası üzerinde hiçbir değişiklik gerektirmemelidir. (örneğin, yeni bir özel görüntü türü eklemek, eski görüntüleri devre dışı bırakmak, özel görüntüler almak için yeni bir ' Endpoint ' DevTest Labs eklemek vb.)
- Ortak değişiklikler kaynak kodu denetimiyle desteklenir (kod olarak altyapı)
- Özel görüntüler alan DevTest Labs aynı Azure aboneliğinde (Labs 'in yayma abonelikleri) bulunmayabilir
- Gerektiğinde ek fabrikaları çalıştırabilmemiz için PowerShell betikleri yeniden kullanılabilir olmalıdır

## <a name="next-steps"></a>Sonraki adımlar
Bu bölümdeki bir sonraki makaleye geçin: [Azure DevOps 'dan bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md)
