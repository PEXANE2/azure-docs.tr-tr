---
title: Sorun giderme Linux VM dağıtımı| Microsoft Dokümanlar
description: Azure'da yeni bir Linux sanal makinesi oluşturduğunuzda Kaynak Yöneticisi dağıtım sorunlarını giderin
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: gwallace
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 98c3a6b14230e30ccbb103be741595696a20c236
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981414"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure'da yeni bir Linux sanal makinesi oluşturmayla ilgili Kaynak Yöneticisi dağıtımı sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En önemli sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Diğer VM dağıtım sorunları ve soruları için bkz. [Azure'da Linux sanal makine dağıtma sorunlarını giderme](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Etkinlik günlüklerini toplama
Sorun giderme başlatmak için, sorunla ilişkili hatayı tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenme süreci hakkında ayrıntılı bilgi içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/templates/deployment-history.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Os Linux genelleştirilmiş ise ve yüklenir ve / veya genelleştirilmiş ayarı ile yakalanan, o zaman herhangi bir hata olmayacaktır. Benzer şekilde, işletim sistemi Linux konusunda uzmanlaşmışsa ve özel ayarla yüklenir ve/veya yakalanırsa, herhangi bir hata olmayacaktır.

**Yükleme Hataları:**

**N<sup>1</sup>:** İşletim sistemi Linux genelleştirilmişse ve özel olarak yüklenmişse, VM sağlama aşamasında sıkışıp kaldığı için bir zaman alameti hatası alırsınız.

**N<sup>2</sup>:** Os Linux özel ve genelleştirilmiş olarak yüklenir, yeni VM orijinal bilgisayar adı, kullanıcı adı ve şifre ile çalışıyor, çünkü bir sağlama hatası hatası alırsınız.

**Çözünürlük:**

Bu hataların her ikisini de gidermek için, işletim sistemi (genelleştirilmiş/özelleştirilmiş) için aynı ayara sahip, şirket içinde bulunan orijinal VHD'yi yükleyin. Genelleştirilmiş olarak yüklemek için önce -deprovision'ı çalıştırmayı unutmayın.

**Yakalama Hataları:**

**N<sup>3</sup>:** İşletim sistemi Linux genelleştirilmişse ve özel olarak yakalanırsa, genelleştirilmiş olarak işaretlendiği için orijinal VM kullanılabilir olmadığından bir zaman alabilme hatası alırsınız.

**N<sup>4</sup>:** Os Linux özel ve genelleştirilmiş olarak yakalanırsa, yeni VM orijinal bilgisayar adı, kullanıcı adı ve şifre ile çalışıyor, çünkü bir sağlama hatası hatası alırsınız. Ayrıca, özel olarak işaretlenmiştir, çünkü orijinal VM kullanılabilir değildir.

**Çözünürlük:**

Bu hataların her ikisini de gidermek için, geçerli görüntüyü portaldan silin ve işletim sistemi (genelleştirilmiş/özelleştirilmiş) ile aynı ayarda [geçerli VHD'lerden yeniden yakalayın.](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: Özel / galeri / pazar görüntüsü; ayırma hatası
Bu hata, yeni VM isteğinin istenen VM boyutunu destekleyemeyen veya isteği karşılamak için boş alana sahip olmayan bir kümeye sabitlendiğinde ortaya çıkar.

**Neden 1:** Küme istenen VM boyutunu destekleyemez.

**1. Çözüm:**

* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM'nin boyutu değiştirilemiyorsa:
  * Kullanılabilirlik kümesindeki tüm VM'leri durdurun.
    **Kaynak gruplarını** > tıklatın*Kaynak grubu* > **Kaynakları** > *kullanılabilirlik ayarsanal* > **makine** > *sanal makine* > **Durdur**.
  * Tüm VM'ler durduktan sonra, yeni VM'yi istenilen boyutta oluşturun.
  * Önce yeni VM'yi başlatın ve ardından durdurulan VM'lerin her birini seçin ve **Başlat'ı**tıklatın.

**Neden 2:** Kümenin boş kaynakları yok.

**2. Çözüm:**

* İsteğini daha sonra yeniden deneyin.
* Yeni VM farklı bir kullanılabilirlik kümesinin parçası olabilirse
  * Farklı bir kullanılabilirlik kümesinde (aynı bölgede) yeni bir VM oluşturun.
  * Yeni VM'yi aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Durmuş bir Linux VM'yi başlattığınızda veya Azure'da varolan bir Linux VM'yi yeniden boyutlandırdığınızda sorunlarla karşılaşırsanız, [Azure'da varolan bir Linux Sanal Makineyi yeniden başlatma veya yeniden boyutlandırma ile ilgili Sorun Gideri Kaynak Yöneticisi dağıtım sorunları](../linux/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)yla ilgili bkz.

