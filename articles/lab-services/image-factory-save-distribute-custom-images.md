---
title: Azure DevTest Labs'da görüntüleri kaydetme ve dağıtma | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs'da zaten oluşturulmuş sanal makinelerden (VM) özel görüntüler kaydetme adımları sunulur.
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
ms.openlocfilehash: e5bc8e5041bfe6d95e3ff1a93bb3338ccead5bb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759440"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Özel görüntüleri birden çok laboratuvara kaydetme ve dağıtma
Bu makalede, zaten oluşturulan sanal makinelerden (VM) özel görüntüleri kaydetmek için adımlar verir. Ayrıca, bu özel görüntülerin kuruluştaki diğer DevTest Laboratuvarlarına nasıl dağıtılabildiğini de kapsar.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki öğeler zaten yerinde olmalıdır:

- Azure DevTest Labs'daki Görüntü Fabrikası için bir laboratuvar.
- Görüntü fabrikasını otomatikleştirmek için kullanılan bir Azure DevOps Projesi.
- Komut dosyalarını ve yapılandırmayı içeren kaynak kodu konumu (örneğimizde, önceki adımda bahsedilen aynı DevOps Projesi'nde).
- Azure Powershell görevlerini düzenlemek için tanım oluşturun.

Gerekirse, bu öğeleri oluşturmak veya kurmak için [Azure DevOps'tan bir görüntü fabrikasını çalıştır'daki](image-factory-set-up-devops-lab.md) adımları izleyin. 

## <a name="save-vms-as-generalized-vhds"></a>Genelleştirilmiş VHD'ler olarak VM'leri kaydetme
Varolan VM'leri genelleştirilmiş VHD'ler olarak kaydedin.  Genelleştirilmiş VHD'ler olarak varolan VM'leri kaydetmek için örnek bir PowerShell komut dosyası vardır. Kullanmak için önce, aşağıdaki resimde gösterildiği gibi yapı tanımına başka bir **Azure Powershell** görevi ekleyin:

![Azure PowerShell adımı ekleme](./media/save-distribute-custom-images/powershell-step.png)

Listede yeni bir görev aldıktan sonra, aşağıdaki resimde gösterildiği gibi tüm ayrıntıları doldurabilmemiz için öğeyi seçin: 

![PowerShell ayarları](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Genelleştirilmiş vs özel özel görüntüler
Azure [portalında,](https://portal.azure.com)sanal bir makineden özel bir görüntü oluştururken, genelleştirilmiş veya özel bir özel görüntüye sahip olmayı seçebilirsiniz.

- **Özel özel görüntü:** Sysprep / Deprovision makineüzerinde çalıştırmak değildi. Bu görüntü varolan sanal makine (bir anlık görüntü) üzerinde IŞLETIM Sistemi Disk tam bir kopyası olduğu anlamına gelir.  Bu özel görüntüden yeni bir makine oluşturduğumuzda, aynı dosyalar, uygulamalar, kullanıcı hesapları, bilgisayar adı ve benzeri, hepsi mevcuttur.
- **Genelleştirilmiş Özel Görüntü:** Sysprep/Deprovision makinede çalıştırıldı.  Bu işlem çalıştığında, kullanıcı hesaplarını kaldırır, bilgisayar adını kaldırır, kullanıcı kayıt defteri kovanları, vb dışarı şeritler, böylece başka bir sanal makine oluştururken özelleştirilebilir görüntü genelleme amacı ile.  Sanal bir makineyi genellediğinizde (sysprep çalıştırarak), işlem geçerli sanal makineyi yok eder – artık işlevsel olmayacaktır.

Resim Fabrikası'nda özel görüntüler yakalama komut dosyası, önceki adımda oluşturulan (Azure'daki kaynağa dayalı olarak tanımlanan) sanal makineler için VHD'leri kaydeder.

## <a name="update-configuration-for-distributing-images"></a>Görüntüleri dağıtmak için yapılandırmayı güncelleştirme
Bu süreçteki bir sonraki adım, özel görüntüleri görüntü fabrikası laboratuvarından ihtiyaç duyan diğer laboratuvarlara itmektir. Bu işlemin temel parçası **labs.json** yapılandırma dosyasıdır. Bu dosyayı görüntü fabrikasında bulunan **Yapılandırma** klasöründe bulabilirsiniz.

Labs.json yapılandırma dosyasında listelenen iki önemli şey vardır:

- Abonelik kimliğini ve laboratuvar adını kullanarak belirli bir hedef laboratuarını benzersiz bir şekilde tanımlama.
- Yapılandırma köküne göreli yollar olarak laboratuvara itilmesi gereken belirli görüntü kümesi. Klasörün tamamını (bu klasördeki tüm görüntüleri almak için) de belirtebilirsiniz.

Burada listelenen iki laboratuvarları ile örnek labs.json dosyasıdır. Bu durumda, görüntüleri iki farklı laboratuvara dağıtıyorsunuz.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Derleme görevi oluşturma
Bu makalede daha önce gördüğünüz adımları kullanarak, yapı tanımına ek bir **Azure Powershell** oluşturma görevi ekleyin. Aşağıdaki resimde gösterildiği gibi ayrıntıları doldurun: 

![Görüntüleri dağıtmak için görev oluşturma](./media/save-distribute-custom-images/second-build-task-powershell.png)

Parametreler şunlardır:`-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Bu görev, görüntü fabrikasında bulunan özel görüntüleri alır ve Labs.json dosyasında tanımlanan herhangi bir laboratuvara iter.

## <a name="queue-the-build"></a>Yapıyı sıraya tonla
Dağıtım oluşturma görevi tamamlandıktan sonra, her şeyin çalıştığından emin olmak için yeni bir yapı oluşturun sıraya girin. Yapı başarıyla tamamlandıktan sonra, yeni özel görüntüler Labs.json yapılandırma dosyasına girilen hedef laboratuarında gösterilecek.

## <a name="next-steps"></a>Sonraki adımlar
Serinin bir sonraki makalesinde, resim fabrikasını bekletme ilkesi ve temizleme adımlarıyla güncelleştirirsiniz: [Bekletme ilkesini ayarlayın ve temizleme komut dosyalarını çalıştırın.](image-factory-set-retention-policy-cleanup.md)
