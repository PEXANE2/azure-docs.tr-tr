---
title: Azure'da Windows VM dağıtımında sorun giderme | Microsoft Dokümanlar
description: Azure'da yeni bir Windows sanal makinesi oluşturduğunuzda Kaynak Yöneticisi dağıtım sorunlarını giderin
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: gwallace
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0bc363b87a9f5b2f013c0bae75a07d79a3a7a830
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981398"
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Azure'da yeni bir Windows VM oluştururken dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En önemli sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Diğer VM dağıtım sorunları ve soruları için, [Azure'da Windows sanal makine sorunlarını dağıtan Sorun Giderme](troubleshoot-deploy-vm-windows.md)konusuna bakın.

## <a name="collect-activity-logs"></a>Etkinlik günlüklerini toplama
Sorun giderme başlatmak için, sorunla ilişkili hatayı tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenme süreci hakkında ayrıntılı bilgi içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/templates/deployment-history.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** İşletim sistemi Windows genelleştirilmişse ve genelleştirilmiş ayarla yüklenir ve/veya yakalanırsa, herhangi bir hata olmaz. Benzer şekilde, işletim sistemi Windows özel ve / veya özel ayarı ile yakalanan ve yüklenir, o zaman herhangi bir hata olmayacaktır.

**Yükleme Hataları:**

**N<sup>1</sup>:** Os Windows genelleştirilmiş ise ve özel olarak yüklenir, VM OOBE ekranında sıkışmış bir sağlama zaman aşması hatası alırsınız.

**N<sup>2</sup>:** Os Windows özel ve genelleştirilmiş olarak yüklenir, yeni VM orijinal bilgisayar adı, kullanıcı adı ve parola ile çalışıyor, çünkü OOBE ekranında sıkışmış VM ile bir sağlama hatası hatası alırsınız.

**Çözünürlük**

Bu hataların her ikisini de gidermek için, işletim sistemi (genelleştirilmiş/özelleştirilmiş) ile aynı ayara sahip, şirket içinde bulunan orijinal VHD'yi yüklemek için [Add-AzVhd'yi](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd)kullanın. Genelleştirilmiş olarak yüklemek için önce sysprep çalıştırmayı unutmayın.

**Yakalama Hataları:**

**N<sup>3</sup>:** İşletim sistemi Windows genelleştirilmişse ve özel olarak yakalanırsa, özgün VM genelleştirilmiş olarak işaretlendirildiği için kullanılabilir olmadığından bir zaman acısı hatası alırsınız.

**N<sup>4</sup>:** İşletim sistemi Windows özelleştirilmişse ve genelleştirilmiş olarak yakalanırsa, yeni VM özgün bilgisayar adı, kullanıcı adı ve parolayla çalıştığı için bir sağlama hatası alırsınız. Ayrıca, özel olarak işaretlenmiştir, çünkü orijinal VM kullanılabilir değildir.

**Çözünürlük**

Bu hataların her ikisini de gidermek için, geçerli görüntüyü portaldan silin ve işletim sistemi (genelleştirilmiş/özelleştirilmiş) ile aynı ayarda [geçerli VHD'lerden yeniden yakalayın.](../windows/create-vm-specialized.md)

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Sorun: Özel/galeri/pazar yeri görüntüsü; ayırma hatası
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
Durmuş bir Windows VM'yi başlattığınızda veya Azure'da varolan bir Windows VM'yi yeniden boyutlandırDığınızda sorunlarla karşılaşırsanız, [Azure'da varolan bir Windows Sanal Makineyi yeniden başlatma veya yeniden boyutlandırma ile ilgili Sorun Gideri Kaynak Yöneticisi dağıtım sorunlarına](restart-resize-error-troubleshooting.md)bakın.


