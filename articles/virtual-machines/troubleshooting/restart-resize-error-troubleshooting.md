---
title: Azure 'da VM yeniden başlatma veya yeniden boyutlandırma | Microsoft Docs
description: Azure 'da var olan bir sanal makineyi yeniden başlatma veya yeniden boyutlandırma ile Kaynak Yöneticisi dağıtım sorunlarını giderme
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a6532558107463311c4225b9855bc4cd3f19eed9
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75965617"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Azure 'da mevcut bir Windows VM 'yi yeniden başlatma veya yeniden boyutlandırma ile ilgili dağıtım sorunlarını giderme
Durdurulmuş bir Azure sanal makinesini (VM) başlatmaya veya mevcut bir Azure VM 'yi yeniden boyutlandırmaya çalıştığınızda, karşılaştığınız ortak hata bir ayırma hatasıdır. Bu hata, küme veya bölgenin kullanılabilir kaynakları yoksa veya istenen VM boyutunu destekleyemezse oluşur.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Etkinlik günlüklerini topla
Sorun gidermeye başlamak için, sorunla ilişkili hatayı belirlemek üzere etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar işlemle ilgili ayrıntılı bilgiler içerir:

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/templates/deployment-history.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: durdurulmuş bir VM başlatılırken hata oluştu
Durdurulmuş bir VM başlatmaya ve bir ayırma hatası almaya çalışırsınız.

### <a name="cause"></a>Nedeni
Durdurulan VM 'yi başlatma isteği, bulut hizmetini barındıran orijinal kümede denenmelidir. Ancak, kümede isteği yerine getirmek için kullanılabilir boş alan yok.

### <a name="resolution"></a>Çözünürlük
* Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun ve ardından her VM 'yi yeniden başlatın.
  
  1. Kaynak **grupları** > *kaynak*grupları > 'natıklayın*kullanılabilirlik kümesi* **sanal makineleriniz**sanal makineniz >  >  > *Durdur.*  > 
  2. Tüm VM 'Ler durduktan sonra, durdurulan sanal makinelerin her birini seçin ve Başlat ' a tıklayın.
* Yeniden başlatma isteğini daha sonra yeniden deneyin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: var olan bir VM yeniden boyutlandırılırken hata
Mevcut bir VM 'yi yeniden boyutlandırmaya ve bir ayırma hatası almaya çalışırsınız.

### <a name="cause"></a>Nedeni
VM 'yi yeniden boyutlandırma isteği, bulut hizmetini barındıran orijinal kümede denenmelidir. Ancak, küme istenen VM boyutunu desteklemiyor.

### <a name="resolution"></a>Çözünürlük
* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM 'nin boyutu değiştirilenemez:
  
  1. Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun.
     
     * Kaynak **grupları** > *kaynak*grupları > 'natıklayın*kullanılabilirlik kümesi* **sanal makineleriniz**sanal makineniz >  >  > *Durdur.*  > 
  2. Tüm VM 'Ler durduktan sonra, istenen VM 'yi daha büyük bir boyuta göre yeniden boyutlandırın.
  3. Yeniden boyutlandırılmış VM 'yi seçin ve **Başlat**' a tıklayın ve ardından durdurulan her bir VM 'yi başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure 'da yeni bir Windows VM oluştururken sorunlarla karşılaşırsanız bkz. [Azure 'da yeni bir Windows sanal makinesi oluşturmayla ilgili dağıtım sorunlarını giderme](../windows/troubleshoot-deployment-new-vm.md).

