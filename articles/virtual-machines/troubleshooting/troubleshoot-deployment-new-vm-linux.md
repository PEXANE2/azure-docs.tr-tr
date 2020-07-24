---
title: Linux VM dağıtımı sorunlarını giderme | Microsoft Docs
description: Azure 'da yeni bir Linux sanal makinesi oluştururken Kaynak Yöneticisi dağıtım sorunlarını giderme
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: DavidCBerry13
manager: gwallace
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 09/09/2016
ms.author: daberry
ms.openlocfilehash: f48963a4d18e80cb67bfbbdc532d34f89b8b5d8a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87028456"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure 'da yeni bir Linux sanal makinesi oluşturma konusunda Kaynak Yöneticisi dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En önemli sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Diğer VM dağıtım sorunları ve soruları için bkz. [Azure'da Linux sanal makine dağıtma sorunlarını giderme](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Etkinlik günlüklerini topla
Sorun gidermeye başlamak için, sorunla ilişkili hatayı belirlemek üzere etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenecek işlemle ilgili ayrıntılı bilgiler içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/templates/deployment-history.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../azure-resource-manager/management/view-activity-logs.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** İşletim sistemi Linux genelleştirilir ve yüklenmiş ve/veya Genelleştirilmiş ayarıyla yakalandıysa, herhangi bir hata olmayacaktır. Benzer şekilde, işletim sistemi Linux özelleşmiş ise ve yüklenmiş ve/veya özel ayarıyla yakalandıysa, herhangi bir hata olmayacaktır.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** İşletim sistemi Linux genelleştirilir ve özelleştirilmiş olarak karşıya yüklenirse, VM sağlama aşamasında takılı olduğundan bir sağlama zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** İşletim sistemi Linux özelleşmiş ise ve genelleştirilmiş olarak yüklenirse, yeni VM özgün bilgisayar adı, Kullanıcı adı ve parola ile çalıştığı için bir sağlama hatası hatası alırsınız.

**Çözünürlüğüne**

Bu hataları gidermek için, şirket içinde bulunan özgün VHD 'yi, işletim sistemi (Genelleştirilmiş/özelleştirilmiş) ile aynı ayarla birlikte karşıya yükleyin. Genelleştirilmiş olarak karşıya yüklemek için önce çalıştırmayı unutmayın.

**Yakalama hataları:**

**N<sup>3</sup>:** İşletim sistemi Linux genelleştirilir ve özel olarak yakalandıysa, orijinal VM Genelleştirilmiş olarak işaretlendiğinden kullanılabilir olmadığı için bir sağlama zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** İşletim sistemi Linux özelleşmiş ise ve genelleştirilmiş olarak yakalandıysa, yeni VM özgün bilgisayar adı, Kullanıcı adı ve parolasıyla çalıştığı için bir sağlama hatası hatası alırsınız. Ayrıca, özgün VM özelleştirilmiş olarak işaretlendiğinden kullanılabilir değildir.

**Çözünürlüğüne**

Bu hataları gidermek için, portaldan geçerli görüntüyü silin ve [geçerli VHD 'lerden](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , işletim sistemi (Genelleştirilmiş/özelleştirilmiş) ile aynı ayarı kullanarak yeniden yakalayın.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: özel/galeri/Market görüntüsü; ayırma hatası
Bu hata, yeni VM isteği istenen VM boyutunu destekleyebilen bir kümeye sabitlendiğinde veya isteğe uygun boş alana sahip olmadığında ortaya çıkar.

**Neden 1:** Küme, istenen VM boyutunu desteklemez.

**Çözüm 1:**

* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM 'nin boyutu değiştirilenemez:
  * Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun.
    Kaynak **grupları**' na tıklayın  >  *your resource group*  >  **Resources**  >  *kullanılabilirlik kümesi*sanal  >  **makinelerinizi**  >  *sanal makinenizin*  >  **durulacağı**kaynak grubu
  * Tüm VM 'Ler durduktan sonra, istenen boyutta yeni VM 'yi oluşturun.
  * Önce yeni VM 'yi başlatın ve sonra durdurulan sanal makinelerin her birini seçip **Başlat**' a tıklayın.

**Neden 2:** Kümede boş kaynak yok.

**Çözüm 2:**

* İsteği daha sonra yeniden deneyin.
* Yeni VM farklı bir kullanılabilirlik kümesinin parçası olabilir
  * Farklı bir kullanılabilirlik kümesinde (aynı bölgede) yeni bir VM oluşturun.
  * Yeni VM 'yi aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Durdurulmuş bir Linux VM başlattığınızda veya Azure 'da var olan bir Linux sanal makinesini yeniden boyutlandırdığınızda sorunlarla karşılaşırsanız bkz. [Azure 'da var olan bir Linux sanal makinesini yeniden başlatma veya yeniden boyutlandırma ile ilgili dağıtım sorunlarını giderme Kaynak Yöneticisi](./troubleshoot-deploy-vm-linux.md?toc=/azure/virtual-machines/linux/toc.json).
