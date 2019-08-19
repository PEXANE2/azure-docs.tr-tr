---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6f6d373d6dd21f3fd04ca434678ec798146a0ffa
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512686"
---
**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerine giden veriler) bant genişliği kullanımı için faturalandırılır.

**İşlemler**: Standart yönetilen bir diskte gerçekleştirdiğiniz işlem sayısı için faturalandırılırsınız. Standart SSD 'Ler için, 256 kıb aktarım hızına eşit veya daha küçük olan her g/ç işlemi tek bir g/ç işlemi olarak kabul edilir. 256 Kigb 'den büyük g/ç işlemleri, 256 KiB boyutundaki birden fazla g/ç işlemi olarak kabul edilir. Standart HDD 'Ler için, her GÇ işlemi, g/ç boyutundan bağımsız olarak tek bir işlem olarak değerlendirilir.

İşlem maliyetleri dahil olmak üzere yönetilen disklere yönelik fiyatlandırma hakkında ayrıntılı bilgi için bkz. [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disk-vm-reservation-fee"></a>Ultra disk VM ayırma ücreti

Azure VM 'Leri, Ultra disklerle uyumlu olup olmadığını gösterebilir. Bir ultra disk uyumlu VM, performansı iyileştirmek ve gecikme süresini azaltmak için işlem VM örneği ve blok depolama ölçek birimi arasında ayrılmış bant genişliği kapasitesini ayırır. Bu özelliği sanal makineye eklemek, yalnızca bir ultra disk iliştirmeden VM 'de Ultra disk özelliğini etkinleştirdiyseniz uygulanan bir rezervasyon ücretine neden olur. Ultra disk uyumlu VM 'ye bir ultra disk eklendiğinde, bu ücret uygulanmaz. Bu ücret, VM 'de sağlanan vCPU başına.

Ultra disk fiyatlandırma ayrıntıları için [Azure diskleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/managed-disks/) bakın.