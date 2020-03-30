---
title: 'D: VM sürücüsübir veri diski olun '
description: "D: sürücü'yi veri sürücüsü olarak kullanabilmeniz için Windows VM için sürücü harflerini nasıl değiştireceğiniz açıklanır."
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 491e7b7be084017cc370fe431c3175ac5b2673f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033646"
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Windows VM'de D: sürücü
Uygulamanızın verileri depolamak için D sürücüsünün kullanılması gerekiyorsa, geçici disk için farklı bir sürücü harfi kullanmak için aşağıdaki yönergeleri izleyin. Saklamanız gereken verileri depolamak için geçici diski asla kullanmayın.

Sanal bir makineyi yeniden boyutlandırveya **durdurursanız (Deallocate)** bu, sanal makinenin yeni bir hipervizöre yerleştirilmesini tetikleyebilir. Planlı veya planlanmamış bir bakım olayı da bu yerleşimi tetikleyebilir. Bu senaryoda, geçici disk kullanılabilir ilk sürücü harfine yeniden atanır. Özellikle D: sürücü gerektiren bir uygulamanız varsa, pagefile.sys'yi geçici olarak taşımak, yeni bir veri diski eklemek ve D harfini atamak ve ardından pagefile.sys'yi geçici sürücüye geri taşımak için aşağıdaki adımları izlemeniz gerekir. Tamamlandığında, Azure D'yi geri almaz: VM farklı bir hipervizöre geçerse.

Azure'un geçici diski nasıl kullandığı hakkında daha fazla bilgi [için](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) bkz.

## <a name="attach-the-data-disk"></a>Veri diskini ekleme
İlk olarak, veri diskini sanal makineye iliştirmeniz gerekir. Bunu yapmak için portalı kullanarak, [Azure portalına yönetilen bir veri diskinin nasıl ekildiğini](attach-managed-disk-portal.md)öğrenin.

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Pagefile.sys'yi geçici olarak C sürücüsüne taşıyın
1. Sanal makineye bağlanın. 
2. **Başlat** menüsüne sağ tıklayın ve **Sistem'i**seçin.
3. Sol menüde **Gelişmiş sistem ayarlarını**seçin.
4. **Performans** bölümünde **Ayarlar'ı**seçin.
5. **Gelişmiş** sekmesini seçin.
6. Sanal **bellek** bölümünde **Değiştir'i**seçin.
7. **C** sürücüsünü seçin ve ardından **Sistem yönetilen boyutu** na tıklayın ve sonra **Ayarla'yı**tıklatın.
8. **D** sürücüsünü seçin ve ardından **Çağrı Yok dosyasını** tıklatın ve sonra **Ayarla'yı**tıklatın.
9. Uygula'yı tıklatın. Değişikliklerin etkisini etkilemesi için bilgisayarın yeniden başlatılması gerektiğine dair bir uyarı alırsınız.
10. Sanal makineyi yeniden başlatın.

## <a name="change-the-drive-letters"></a>Sürücü harflerini değiştirme
1. VM yeniden başlatıldıktan sonra, VM'ye yeniden oturum açın.
2. **Başlat** menüsüne tıklayın ve **diskmgmt.msc** yazın ve Enter tuşuna basın. Disk Yönetimi başlayacaktır.
3. Geçici Depolama sürücüsü **D'ye**sağ tıklayın ve **Sürücü Mektubunu ve Yolları Değiştir'i**seçin.
4. Sürücü harfinin altında, **T** gibi yeni bir sürücü seçin ve ardından **Tamam'ı**tıklatın. 
5. Veri diskine sağ tıklayın ve **Sürücü Mektubu ve Yolları Değiştir'i**seçin.
6. Sürücü harfinin altında, **D** sürücüsünü seçin ve ardından **Tamam'ı**tıklatın. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>pagefile.sys'yi geçici depolama sürücüsüne geri taşıma
1. **Başlat** menüsüne sağ tıklayın ve **Sistem'i** seçin
2. Sol menüde **Gelişmiş sistem ayarlarını**seçin.
3. **Performans** bölümünde **Ayarlar'ı**seçin.
4. **Gelişmiş** sekmesini seçin.
5. Sanal **bellek** bölümünde **Değiştir'i**seçin.
6. İşletim sistemi sürücüsü **C'yi** seçin ve **Çağrı Yok dosyasını** tıklatın ve sonra **Ayarla'yı**tıklatın.
7. Geçici depolama sürücüsü **T'yi** seçin ve ardından **Sistem yönetilen boyutu** tıklatın ve sonra **Ayarla'yı**tıklatın.
8. **Uygula**’ya tıklayın. Değişikliklerin etkisini etkilemesi için bilgisayarın yeniden başlatılması gerektiğine dair bir uyarı alırsınız.
9. Sanal makineyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
* [Ek bir veri diski ekleyerek](attach-managed-disk-portal.md)sanal makinenizin kullanabileceği depolama alanını artırabilirsiniz.

