---
title: Azure DevTest Labs'da görüntü fabrikası oluşturma | Microsoft Dokümanlar
description: Bu makalede, Git deposunda (Azure DevTest Labs) bulunan örnek komut dosyalarını kullanarak özel bir görüntü fabrikasının nasıl kurulabileceğinizgösterilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759457"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Azure DevTest Labs'da özel bir görüntü fabrikası oluşturun
Bu makalede, [Git deposunda](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)bulunan örnek komut dosyalarını kullanarak özel bir görüntü fabrikasının nasıl kurulabileceğinizi gösterilmektedir.

## <a name="whats-an-image-factory"></a>İmaj fabrikası nedir?
Görüntü fabrikası, istenen tüm yapılandırmalarla görüntüleri düzenli olarak otomatik olarak oluşturan ve dağıtan bir yapılandırma kodu olarak çözümdür. Görüntü fabrikasındaki görüntüler her zaman günceldir ve tüm işlem otomatikleştirici olduğunda devam eden bakım neredeyse sıfırdır. Ve gerekli tüm yapılandırmalar zaten görüntüde olduğundan, temel işletim sistemi yle bir VM oluşturulduktan sonra sistemi el ile yapılandırmaktan zaman kazandırır.

DevTest Labs'da geliştirici masaüstünü hazır duruma getirmek için önemli hızlandırıcı özel görüntüler kullanıyor. Özel görüntülerin dezavantajı, laboratuvarda muhafaza etmek için ekstra bir şey olmasıdır. Örneğin, ürünlerin deneme sürümlerinin süresi zaman içinde sona erer (veya) yeni yayımlanan güvenlik güncelleştirmeleri uygulanmaz ve bu da bizi özel görüntüyü düzenli aralıklarla yenilemeye zorlar. Bir görüntü fabrikasında, kaynak kod denetimi için iade edilen görüntünün bir tanımına sahipsiniz ve tanıma dayalı özel görüntüler oluşturmak için otomatik bir işlem elde elabilirsiniz.

Çözüm, özel görüntülerden sanal makineler oluşturma hızını sağlarken, devam eden ek bakım maliyetlerini de ortadan kaldırır. Bu çözümle, otomatik olarak özel görüntüler oluşturabilir, bunları diğer DevTest Labs'a dağıtabilir ve eski görüntüleri emekli yedirebilirsiniz. Aşağıdaki videoda, görüntü fabrikası ve DevTest Labs ile nasıl uygulandığı hakkında bilgi edinebilirsiniz.  Tüm Azure Powershell komut dosyaları serbestçe kullanılabilir [https://aka.ms/dtlimagefactory](https://aka.ms/dtlimagefactory)ve burada bulunur: .

<br/>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Custom-Image-Factory-with-Azure-DevTest-Labs/player]


## <a name="high-level-view-of-the-solution"></a>Çözümün üst düzey görünümü
Çözüm, özel görüntülerden sanal makineler oluşturma hızını sağlarken, devam eden ek bakım maliyetlerini de ortadan kaldırır. Bu çözümle, otomatik olarak özel görüntüler oluşturabilir ve bunları diğer DevTest Laboratuvarlarına dağıtabilirsiniz. DevTest Labs'daki tüm işlemleri otomatikleştirmek için Azure DevOps'leri (eski adıyla Visual Studio Team Services) orkestrasyon motoru olarak kullanırsınız.

![Çözümün üst düzey görünümü](./media/create-image-factory/high-level-view-of-solution.png)

DevTest Labs için bu adımları tek tek yürütmenize olanak tanıyan bir [VSTS Uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) vardır:

- Özel görüntü oluşturma
- VM oluşturma
- VM Silme
- Ortam oluşturma
- Ortamı silme
- Ortamı doldurma

DevTest Labs uzantısını kullanmak, DevTest Labs'da otomatik olarak özel görüntüler oluşturmaya başlamanın kolay bir yoludur.

Daha karmaşık bir senaryo için PowerShell komut dosyası kullanarak alternatif bir uygulama var. PowerShell'i kullanarak, Sürekli Tümleştirme ve Sürekli Teslimat (CI/CD) araç zincirinizde kullanılabilecek DevTest Labs'ı temel alan bir görüntü fabrikasını tamamen otomatikleştirebilirsiniz. Bu alternatif çözümde izlenen ilkeler şunlardır:

- Ortak güncelleştirmeler görüntü fabrikasında değişiklik gerektirmemelidir. (örneğin, yeni bir tür özel görüntü eklemek, eski görüntüleri otomatik olarak emekliye ayırın, özel görüntüler almak için yeni bir 'bitiş noktası' DevTest Labs ekleyerek vb.)
- Sık yapılan değişiklikler kaynak kodu denetimi (kod olarak altyapı) tarafından yedeklenir
- Özel görüntüler alan DevTest Labs aynı Azure Aboneliğinde olmayabilir (laboratuvarlar aboneliklere yayılabilir)
- PowerShell komut dosyaları yeniden kullanılabilir olmalıdır, böylece gerektiğinde ek fabrikalar kurabiliriz

## <a name="next-steps"></a>Sonraki adımlar
Bu bölümdeki bir sonraki makaleye geçin: [Azure DevOps'lerden bir resim fabrikası çalıştırın](image-factory-set-up-devops-lab.md)
