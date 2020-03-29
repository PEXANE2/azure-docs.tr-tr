---
title: Klasik dağıtım modelinde Azure VM ayırma hatalarını giderme| Microsoft Dokümanlar
description: Azure'da klasik bir VM oluştururken, yeniden başlattığınızda veya yeniden boyutlandırdığınızda giderme hataları
services: azure-service-management
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 20e64e5225987a8045e406a0e8fcae098c580c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77913387"
---
# <a name="troubleshooting-steps-specific-to-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Klasik dağıtım modelindeki ayırma hatası senaryolarına özgü sorun giderme adımları

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Aşağıda, ayırma isteğinin sabitlenemesine neden olan yaygın ayırma senaryoları verilmiştir. Bu makalede daha sonra her senaryoya dalacağız.

- VM'yi yeniden boyutlandırma veya varolan bir bulut hizmetine VM veya rol örnekleri ekleme
- Kısmen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma
- Tamamen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma
- Evreleme ve üretim dağıtımları (yalnızca hizmet olarak platform)
- Yakınlık grubu (VM veya hizmet yakınlığı)
- Affinity-grup tabanlı sanal ağ

Bir ayırma hatası aldığınızda, listelenen senaryolardan herhangi birinin hatanız için geçerli olup olmadığını denetleyin. İlgili senaryoyu tanımlamak için Azure platformu tarafından döndürülen ayırma hatasını kullanın. İsteğiniz sabitlenirse, isteğinizi daha fazla kümeye açmak için sabitleme kısıtlamalarından bazılarını kaldırın ve bu nedenle ayırma başarısı olasılığını artırın.
Genel olarak, hata "istenen VM boyutu desteklenmiyor" olarak belirtmezse, her zaman daha sonra yeniden deneyebilirsiniz. Bunun nedeni, isteğinizi karşılamak için kümede yeterli kaynağın serbest bırakılmış olmasıdır. Sorun istenen VM boyutunun desteklenmemesiyse, farklı bir VM boyutu deneyin. Aksi takdirde, tek seçenek sabitleme kısıtlamasını kaldırmaktır.

İki yaygın hata senaryosu yakınlık gruplarıyla ilişkilidir. Geçmişte, Bir yakınlık grubu VM'lere ve hizmet örneklerine yakın bir yakınlık sağlamak için veya sanal bir ağ oluşturulmasını etkinleştirmek için kullanılmıştır. Bölgesel sanal ağların devreye girmesiyle, yakınlık gruplarının sanal ağ oluşturması na gerek kalmaz. Azure altyapısında ağ gecikmesinin azaltılmasıyla, VM'ler veya hizmet yakınlığı için yakınlık grupları kullanma önerisi değişti.

Aşağıdaki Diyagram (sabitlenmiş) ayırma senaryolarının taksonomisini sunar. 

![Sabitlenmiş Tahsistakisi](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>VM'yi yeniden boyutlandırma veya varolan bir bulut hizmetine VM veya rol örnekleri ekleme
**Hata**

Upgrade_VMSizeNotSupported veya GenelHata

**Küme sabitleme nedeni**

Varolan bulut hizmetine bir VM'i yeniden boyutlandırma veya VM veya rol örneği ekleme isteği, varolan bulut hizmetini barındıran özgün kümede denenmiş olmalıdır. Yeni bir bulut hizmeti oluşturmak, Azure platformunun ücretsiz kaynaklara sahip veya istediğiniz VM boyutunu destekleyen başka bir küme bulmasına olanak tanır.

**Geçi -ci çözüm**

Hata Upgrade_VMSizeNotSupported* ise, farklı bir VM boyutu deneyin. Farklı bir VM boyutu kullanmak bir seçenek değilse, ancak farklı bir sanal IP adresi (VIP) kullanmak kabul edilebilirse, yeni VM'yi barındırmak ve yeni bulut hizmetini mevcut VM'lerin çalıştığı bölgesel sanal ağa eklemek için yeni bir bulut hizmeti oluşturun. Varolan bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, yeni bulut hizmeti için yeni bir sanal ağ oluşturabilir ve [mevcut sanal ağınızı yeni sanal ağa](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/)bağlayabilirsiniz. [Bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)hakkında daha fazla bilgi edinin.

Hata Genel Hata*ise, kaynak türünün (belirli bir VM boyutu gibi) küme tarafından desteklenmesi olasıdır, ancak kümenin şu anda boş kaynakları yoktur. Yukarıdaki senaryoya benzer şekilde, yeni bir bulut hizmeti oluşturarak istediğiniz bilgi işlem kaynağını ekleyin (yeni bulut hizmetinin farklı bir VIP kullanması gerektiğini unutmayın) ve bulut hizmetlerinizi bağlamak için bölgesel bir sanal ağ kullanın.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma
**Hata**

GenelHata*

**Küme sabitleme nedeni**

Kısmi ayırma, bir bulut hizmetindeki VM'leri bir veya daha fazla, ancak tümü yle değil, durdurmuş (ayrılmış) anlamına gelir. Bir VM'yi durdurduğunuzda (anlaşma yaptığınızda) ilişkili kaynaklar serbest bırakılır. Bu nedenle, durdurulan (ayrılan) VM'yi yeniden başlatmak yeni bir ayırma isteğidir. Kısmen ayrılmış bir bulut hizmetinde VM'leri yeniden başlatmak, varolan bir bulut hizmetine VM eklemeye eşdeğerdir. Ayırma isteği, varolan bulut hizmetini barındıran özgün kümede denenmiş olmalıdır. Farklı bir bulut hizmeti oluşturmak, Azure platformunun ücretsiz kaynağa sahip veya istediğiniz VM boyutunu destekleyen başka bir küme bulmasına olanak tanır.

**Geçi -ci çözüm**

Farklı bir VIP kullanmak kabul edilebilirse, durdurulan (ayrılan) VM'leri silin (ancak ilişkili diskleri koruyun) ve farklı bir bulut hizmeti aracılığıyla VM'leri geri ekleyin. Bulut hizmetlerinizi bağlamak için bölgesel bir sanal ağ kullanın:

* Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanıyorsa, yeni bulut hizmetini aynı sanal ağa eklemeniz yeterlidir.
* Varolan bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, yeni bulut hizmeti için yeni bir sanal ağ oluşturun ve [mevcut sanal ağınızı yeni sanal ağa bağlayın.](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) [Bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)hakkında daha fazla bilgi edinin.

## <a name="restart-fully-stopped-deallocated-vms"></a>Tamamen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma
**Hata**

GenelHata*

**Küme sabitleme nedeni**

Tam ayırma, tüm VM'leri bir bulut hizmetinden durdurmuş (devre sizde) olduğunuz anlamına gelir. Bu VM'leri yeniden başlatmak için ayırma istekleri, bulut hizmetini barındıran özgün kümede denenmiş olmalıdır. Yeni bir bulut hizmeti oluşturmak, Azure platformunun ücretsiz kaynaklara sahip veya istediğiniz VM boyutunu destekleyen başka bir küme bulmasına olanak tanır.

**Geçi -ci çözüm**

Farklı bir VIP kullanmak kabul edilebilirse, orijinal durdurulan (ayrılan) VM'leri silin (ancak ilişkili diskleri saklayın) ve ilgili bulut hizmetini silin (VM'leri durdurduğunuzda (ayrılan) ilişkili bilgi işlem kaynakları zaten serbest bırakıldı). VM'leri geri eklemek için yeni bir bulut hizmeti oluşturun.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Evreleme/üretim dağıtımları (yalnızca hizmet olarak platform)
**Hata**

New_General* veya New_VMSizeNotSupported*

**Küme sabitleme nedeni**

Hazırlama dağıtımı ve bir bulut hizmetinin üretim dağıtımı aynı kümede barındırılır. İkinci dağıtım eklediğinizde, ilgili ayırma isteği ilk dağıtımı barındıran aynı kümede denenir.

**Geçi -ci çözüm**

İlk dağıtımı ve özgün bulut hizmetini silin ve bulut hizmetini yeniden dağıtın. Bu eylem, ilk dağıtımı hem dağıtımlara sığdıracak kadar özgür kaynağa sahip bir kümeye hem de istediğiniz VM boyutlarını destekleyen bir kümeye indirebilir.

## <a name="affinity-group-vmservice-proximity"></a>Yakınlık grubu (VM/servis yakınlığı)
**Hata**

New_General* veya New_VMSizeNotSupported*

**Küme sabitleme nedeni**

Bir yakınlık grubuna atanan herhangi bir bilgi işlem kaynağı bir kümeye bağlıdır. Bu yakınlık grubundaki yeni bilgi işlem kaynak istekleri, varolan kaynakların barındırıldığı aynı kümede denenir. Bu, yeni kaynakların yeni bir bulut hizmeti veya varolan bir bulut hizmeti aracılığıyla oluşturulup oluşturulmadığı doğrudur.

**Geçi -ci çözüm**

Bir yakınlık grubu gerekli değilse, bir yakınlık grubu kullanmayın veya bilgi işlem kaynaklarınızı birden çok yakınlık grubunda gruplandırmayın.

## <a name="affinity-group-based-virtual-network"></a>Affinity-grup tabanlı sanal ağ
**Hata**

New_General* veya New_VMSizeNotSupported*

**Küme sabitleme nedeni**

Bölgesel sanal ağlar kullanılmadan önce, bir sanal ağı bir yakınlık grubuyla ilişkilendirmeniz gerekiyordu. Sonuç olarak, bir yakınlık grubuna yerleştirilen bilgi işlem kaynakları, yukarıdaki "Ayırma senaryosu: Affinity group (VM/service proximity)" bölümünde açıklandığı gibi aynı kısıtlamalara bağlıdır. İşlem kaynakları bir kümeye bağlıdır.

**Geçi -ci çözüm**

Bir yakınlık grubuna ihtiyacınız yoksa, eklediğiniz yeni kaynaklar için yeni bir bölgesel sanal ağ oluşturun ve [ardından varolan sanal ağınızı yeni sanal ağa bağlayın.](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) [Bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)hakkında daha fazla bilgi edinin.

Alternatif olarak, [yakınlık grubu tabanlı sanal ağınızı bölgesel bir sanal ağa geçirebilir](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)ve ardından istenen kaynakları yeniden ekleyebilirsiniz.


