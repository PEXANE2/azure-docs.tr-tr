---
title: Azure DevTest Labs'da bekletme ilkesini ayarlama | Microsoft Dokümanlar
description: Bekletme politikasını nasıl yapılandıracak, fabrikayı nasıl temizleyecek ve DevTest Labs'daki eski görüntüleri nasıl emekli yedireceklerini öğrenin.
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
ms.openlocfilehash: a472c500ee6b968b1459e65e49a352b81e5ea6ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759423"
---
# <a name="set-up-retention-policy-in-azure-devtest-labs"></a>Azure DevTest Labs'da bekletme ilkesini ayarlama
Bu makalede, bir saklama ilkesi nin ayarlanması, fabrikanın temizlenmesi ve kuruluştaki diğer tüm DevTest Labs'ın eski görüntülerinin emekliye ayrılacağı yer alıyor. 

## <a name="prerequisites"></a>Ön koşullar
Daha fazla devam etmeden önce bu makaleleri takip ettiğinizden emin olun:

- [Görüntü fabrikası oluşturma](image-factory-create.md)
- [Azure DevOps’tan bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md)
- [Özel görüntüleri birden çok laboratuvara kaydetme ve dağıtma](image-factory-save-distribute-custom-images.md)

Aşağıdaki öğeler zaten yerinde olmalıdır:

- Azure DevTest Labs'daki görüntü fabrikası için bir laboratuvar
- Fabrikanın altın görüntüleri dağıtacağı bir veya daha fazla hedef Azure DevTest Labs
- Görüntü fabrikasını otomatikleştirmek için kullanılan bir Azure DevOps Projesi.
- Komut dosyalarını ve yapılandırmayı içeren kaynak kodu konumu (örneğimizde, yukarıda kullanılan aynı DevOps Projesi'nde)
- Azure Powershell görevlerini düzenlemek için yapı tanımı
 
## <a name="setting-the-retention-policy"></a>Bekletme ilkesini ayarlama
Temizleme adımlarını yapılandırmadan önce, DevTest Labs'da kaç tane tarihi görüntü saklamak istediğinizi tanımlayın. [Azure DevOps makalesinden bir görüntü fabrikasıçalıştır'ı](image-factory-set-up-devops-lab.md) takip ettiğinizde, çeşitli yapı Değişkenleri yapılandırıldınız. Bunlardan biri **ImageRetention**oldu. Bu değişkeni `1`, Yani DevTest Labs özel görüntülerin geçmişini sürdürmeyecektir. Yalnızca en son dağıtılan görüntüler kullanılabilir. Bu değişkeni `2`, en son dağıtılan görüntü artı öncekiler olarak değiştirirseniz korunur. Bu değeri, DevTest Labs'da korumak istediğiniz tarihi görüntü sayısını tanımlamak için ayarlayabilirsiniz.

## <a name="cleaning-up-the-factory"></a>Fabrikayı Temizleme
Fabrikayı temizlemenin ilk adımı, altın görüntü VM'lerini görüntü fabrikasından çıkarmaktır. Önceki komut dosyalarımız gibi bu görevi yapmak için bir komut dosyası vardır. İlk adım, aşağıdaki resimde gösterildiği gibi yapı tanımına başka bir **Azure Powershell** görevi eklemektir:

![PowerShell adımı](./media/set-retention-policy-cleanup/powershell-step.png)

Listede yeni bir görev olduğunda, öğeyi seçin ve aşağıdaki resimde gösterildiği gibi tüm ayrıntıları doldurun:

![Eski görüntüleri temizleme PowerShell görevi](./media/set-retention-policy-cleanup/configure-powershell-task.png)

Komut dosyası parametreleri şunlardır: `-DevTestLabName $(devTestLabName)`.

## <a name="retire-old-images"></a>Eski görüntüleri emekli edin 
Bu görev, yalnızca **ImageRe** yapı değişkeni yle eşleşen bir geçmişi tutarak eski görüntüleri kaldırır. Yapı tanımımıza ek bir **Azure Powershell** oluşturma görevi ekleyin. Eklendikten sonra görevi seçin ve aşağıdaki resimde gösterildiği gibi ayrıntıları doldurun: 

![Eski görüntüleri PowerShell görev emekli](./media/set-retention-policy-cleanup/retire-old-image-task.png)

Komut dosyası parametreleri şunlardır:`-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(devTestLabName) -ImagesToSave $(ImageRetention)`

## <a name="queue-the-build"></a>Yapıyı sıraya tonla
Yapı tanımını tamamladığınızda, her şeyin çalıştığından emin olmak için yeni bir yapı oluşturun sıraya girin. Yapı başarıyla tamamlandıktan sonra yeni özel görüntüler hedef laboratuarda açılır ve görüntü fabrikası laboratuarını kontrol ederseniz, sağlanan VM'ler görmezsiniz. Ayrıca, daha fazla yapı yığmak için sıraya girederseniz, geliştirme değişkenlerinde ayarlanan bekletme değerine uygun olarak DevTest Labs'dan eski özel görüntüleri emekli yediren temizleme görevlerini görürsünüz.

> [!NOTE]
> Serinin son makalesinin sonunda yapı ardışık hattını çalıştırdıysanız, yeni bir yapıyı sıraya girmeden önce görüntü fabrikası laboratuvarında oluşturulan sanal makineleri el ile silin.  Manuel temizleme adımı sadece biz her şeyi kurmak ve çalıştığını doğrulamak için gereklidir.



## <a name="summary"></a>Özet
Artık, isteğe bağlı olarak laboratuvarlarınıza özel görüntüler oluşturabilen ve dağıtabilen çalışan bir görüntü fabrikanız var. Bu noktada, sadece görüntüleri düzgün kurmak ve hedef laboratuvarları tespit meselesi. Önceki makalede belirtildiği gibi, **Configuration** klasörünüzde bulunan **Labs.json** dosyası, hedef laboratuvarların her birinde hangi görüntülerin kullanılabilir hale getirilmesi gerektiğini belirtir. Kuruluşunuza diğer DevTest Labs eklerken, sadece yeni laboratuvar için Labs.json bir giriş eklemeniz gerekir.

Fabrikanıza yeni bir görüntü eklemek de kolaydır. Fabrikanıza yeni bir resim eklemek istediğinizde [Azure portalını](https://portal.azure.com)açarsınız, fabrikanızdaki DevTest Labs'a gidin, VM eklemek için düğmeyi seçin ve istediğiniz pazar yeri görüntüsünü ve yapıyı seçin. Yeni VM yapmak için **Oluştur** düğmesini seçmek yerine, **Azure Kaynak Yöneticisi şablonunu görüntüle"yi**seçin ve şablonu deponuzdaki **GoldenImages** klasöründe bir yere .json dosyası olarak kaydedin. Görüntü fabrikanızı bir sonraki çalıştırdığınızda, özel resminizi oluşturur.


## <a name="next-steps"></a>Sonraki adımlar
1. Görüntü fabrikasını düzenli olarak çalıştırmak için [yapı/sürümünüzü zamanlayın.](/azure/devops/pipelines/build/triggers?view=azure-devops&tabs=designer) Fabrikada oluşturulan görüntüleri düzenli olarak yeniler.
2. Fabrikanız için daha fazla altın görüntü yapın. Ayrıca, VM kurulum görevlerinizin ek parçalarını komut dosyası oluşturmak ve yapıyı fabrika resimlerinize eklemek için [eserler oluşturmayı](devtest-lab-artifact-author.md) da düşünebilirsiniz.
4. **DistributeImages** komut dosyasını ayrı olarak çalıştırmak için ayrı bir [yapı/sürüm](/azure/devops/pipelines/overview?view=azure-devops-2019) oluşturun. Labs.json'da değişiklik yaptığınızda bu komut dosyasını çalıştırabilir ve tüm görüntüleri yeniden oluşturmak zorunda kalmadan görüntüleri hedef laboratuarlara kopyalayabilirsiniz.

