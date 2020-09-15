---
title: Windows sorunlarını giderme sistemi yapılandırmayı tamamlayamadı
titlesuffix: Azure Virtual Machines
description: Bu makalede, Sysprep işleminin bir Azure VM 'nin önyüklenmesini önleyen sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5fc57a8f-5a0c-4b5f-beef-75eecb4d310d
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/09/2020
ms.author: v-miegge
ms.openlocfilehash: 9c170607d6300c4921285e85ac78db5a8a18ad9b
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078827"
---
# <a name="troubleshoot-windows-could-not-finish-configuring-the-system"></a>Windows sorunlarını giderme sistemi yapılandırmayı tamamlayamadı

Bu makalede, Sysprep işleminin bir Azure sanal makinesinin (VM) önyüklenmesini önleyen sorunları çözümlemek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, Windows kurulumu hizmetleri başlatırken ekran görüntüsünde bir Windows yükleme hatası görüntülediğini görürsünüz. Hata şu iletiyi görüntüler:

`Windows could not finish configuring the system. To attempt to resume configuration, restart the computer. Setup is starting services`

  ![Şekil 1 ' de şu iletiyle bir Windows yüklemesi hatası görüntülenir: "Windows sistemi yapılandırmayı tamamlayamadı. Yapılandırmayı sürdürmeyi denemek için bilgisayarı yeniden başlatın. Kurulum Hizmetleri başlatıyor "](./media/windows-could-not-configure-system/1-windows-error-configure.png)

## <a name="cause"></a>Nedeni

Bu hata, işletim sistemi (OS) [Sysprep işlemini](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-process-overview)tamamlayamadığında oluşur. Bu hata, genelleştirilmiş bir VM 'nin ilk önyüklemesine çalıştığınızda oluşur. Bu sorunla karşılaşırsanız, görüntü dağıtılabilir bir durumda olduğundan ve kurtarılamadığından Genelleştirilmiş görüntüyü yeniden oluşturun.

## <a name="solution"></a>Çözüm

Bu sorunu giderecek [bir görüntü hazırlama/yakalama](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed) ve yeni bir Genelleştirilmiş görüntü hazırlama hakkında Azure kılavuzunu izleyin.
