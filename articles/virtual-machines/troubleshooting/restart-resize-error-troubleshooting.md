---
title: Azure'da VM yeniden başlatma veya yeniden boyutlandırma sorunları | Microsoft Dokümanlar
description: Azure'da varolan bir Sanal Makineyi yeniden başlatma veya yeniden boyutlandırma ile ilgili Kaynak Yöneticisi dağıtımı sorunlarını giderme
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75965617"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Azure'daki bir Windows VM'yi yeniden başlatma veya yeniden boyutlandırmayla ilgili sorunları giderme
Durmuş bir Azure Sanal Makine 'ni (VM) başlatmaya veya varolan bir Azure VM'sini yeniden boyutlandırmaya çalıştığınızda, karşılaştığınız yaygın hata bir ayırma hatasıdır. Küme veya bölge kullanılabilir kaynaklara sahip olmadığında veya istenen VM boyutunu destekleyemediğinde bu hata sonucu olur.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Etkinlik günlüklerini toplama
Sorun giderme başlatmak için, sorunla ilişkili hatayı tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar işlem hakkında ayrıntılı bilgi içerir:

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/templates/deployment-history.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: Durdurulmuş VM'yi başlatırken karşılaşılan hata
Durdurulan bir VM başlatmaya çalışırsınız, ancak bir ayırma hatası alırsınız.

### <a name="cause"></a>Nedeni
Durdurulan VM'yi başlatma isteği, bulut hizmetini barındıran özgün kümede denenmiş olmalıdır. Ancak, küme isteği yerine getirmek için boş alan yok.

### <a name="resolution"></a>Çözüm
* Kullanılabilirlik kümesindeki tüm VM'leri durdurun ve ardından her VM'yi yeniden başlatın.
  
  1. **Kaynak gruplarını** > tıklatın*Kaynak grubu* > **Kaynakları** > *kullanılabilirlik ayarsanal* > **makine** > *sanal makine* > **Durdur**.
  2. Tüm VM'ler durduktan sonra, durdurulan VM'lerin her birini seçin ve Başlat'ı tıklatın.
* Yeniden başlatma isteğini daha sonra yeniden deneyin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: Var olan bir VM'yi yeniden boyutlandırma sırasında hata
Varolan bir VM'yi yeniden boyutlandırmaya çalışırsınız, ancak bir ayırma hatası alırsınız.

### <a name="cause"></a>Nedeni
VM'yi yeniden boyutlandırma isteği, bulut hizmetini barındıran özgün kümede denenmiş olmalıdır. Ancak, küme istenen VM boyutunu desteklemez.

### <a name="resolution"></a>Çözüm
* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM'nin boyutu değiştirilemiyorsa:
  
  1. Kullanılabilirlik kümesindeki tüm VM'leri durdurun.
     
     * **Kaynak gruplarını** > tıklatın*Kaynak grubu* > **Kaynakları** > *kullanılabilirlik ayarsanal* > **makine** > *sanal makine* > **Durdur**.
  2. Tüm VM'ler durduktan sonra, istenen VM'yi daha büyük bir boyuta yeniden boyutlandırın.
  3. Yeniden boyutlandırılmış VM'yi seçin ve **Başlat'ı**tıklatın ve ardından durdurulan VM'lerin her birini başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni bir Windows VM oluştururken sorunlarla karşılaşırsanız, [Azure'da yeni bir Windows sanal makinesi oluşturmayla ilgili Sorun Giderme güncelleştirmesi sorunlarına](../windows/troubleshoot-deployment-new-vm.md)bakın.

