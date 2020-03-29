---
title: DevTest Labs'da özel görüntüleri ve formülleri karşılaştırma | Microsoft Dokümanlar
description: Ortamınıza en uygun olanın hangisiolduğuna karar verebilmeniz için VM bazları olarak özel görüntüler ve formüller arasındaki farklar hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: ae7556eda817b9eb7be84f9d4a23ea91d3d5440d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64680292"
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>DevTest Labs'da özel görüntüleri ve formülleri karşılaştırma
Hem [özel görüntüler](devtest-lab-create-template.md) hem de [formüller](devtest-lab-manage-formulas.md) [oluşturulan yeni VM'ler](devtest-lab-add-vm.md)için temel olarak kullanılabilir. Ancak, özel görüntüler ve formüller arasındaki temel ayrım, özel bir görüntünün sadece VHD'ye dayalı bir görüntü olması, formül ise VM Boyutu, sanal ağ, alt ağ ve yapı yapıları gibi önceden yapılandırılmış *ayarlara ek olarak* VHD'ye dayalı bir görüntü olmasıdır. Önceden yapılandırılmış bu ayarlar, VM oluşturma sırasında geçersiz kılınabilen varsayılan değerlerle ayarlanır. Bu makalede, formülleri kullanarak karşı özel görüntüler kullanarak bazı avantajları (artıları) ve dezavantajları (eksileri) açıklar.

## <a name="custom-image-pros-and-cons"></a>Özel görüntü artıları ve eksileri
Özel görüntüler, istenilen ortamdan VM'ler oluşturmak için statik ve değişmez bir yol sağlar. 

**Artıları**

* Özel bir görüntüden VM sağlama, VM görüntüden yukarı döndürüldükten sonra hiçbir şey değişmeden hızlıdır. Başka bir deyişle, özel görüntü ayarları olmayan bir görüntü olduğu gibi uygulanacak hiçbir ayar yoktur. 
* Tek bir özel görüntüden oluşturulan VM'ler aynıdır.

**Simgeler**

* Özel görüntünün bazı yönünü güncelleştirmeniz gerekiyorsa, görüntünün yeniden oluşturulması gerekir.  

## <a name="formula-pros-and-cons"></a>Formül artıları ve eksileri
Formüller, istenen yapılandırma/ayarlardan VM oluşturmak için dinamik bir yol sağlar.

**Artıları**

* Çevredeki değişiklikler eserler aracılığıyla anında yakalanabilir. Örneğin, sürüm ardışık ardışık ardışık ardışık ardışık ardışık en son bitleri içeren bir VM'nin yüklenmesini veya deponuzdan en son kodu kaydolmasını istiyorsanız, en son bitleri dağıtan veya formüldeki en son kodu bir hedef tabanla birlikte listeleyen bir yapı belirtebilirsiniz Görüntü. Bu formül VM'ler oluşturmak için kullanıldığında, en son bitler/kod VM'ye dağıtılır/kaydedilir. 
* Formüller, Özel görüntülerin sağlayamadığı VM boyutları ve sanal ağ ayarları gibi varsayılan ayarları tanımlayabilir. 
* Formülde kaydedilen ayarlar varsayılan değerler olarak gösterilir, ancak VM oluşturulduğunda değiştirilebilir. 

**Simgeler**

* Formülden VM oluşturmak, özel bir görüntüden VM oluşturmaktan daha fazla zaman alabilir.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntüler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
- [Devtest Labs SSS](devtest-lab-faq.md)