---
title: Sanal makine boyutu esnekliği-Azure ayrılmış VM örnekleri
description: Ayrılmış bir sanal makine örneği tarafından sizin tarafınızdan bir rezervasyon iskontosunun için geçerli olan boyut serisini öğrenin.
author: yashesvi
ms.service: virtual-machines
ms.subservice: reserved-instances
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 03/01/2021
ms.author: yashar
ms.openlocfilehash: 9270b7fb135eddd78a7aeb30cf88af4d704e176e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102030717"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği

Ayrılmış bir sanal makine örneği satın aldığınızda, örnek boyutu esnekliği veya kapasite önceliği için iyileştirmeden seçim yapabilirsiniz. Ayrılmış VM örnekleri için en iyileştirme ayarını ayarlama veya değiştirme hakkında daha fazla bilgi için bkz. [ayrılmış VM örnekleri için optimizasyon ayarını değiştirme](../cost-management-billing/reservations/manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).

Örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir sanal makine örneğiyle, satın aldığınız rezervasyon aynı örnek boyutu esneklik grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Örneğin, Standard_DS3_v2 gibi DSv2 serisinde listelenen bir VM boyutu için bir ayırma satın alırsanız, rezervasyon iskontosu aynı örnek boyutu esneklik grubunda listelenen diğer boyutlara uygulanabilir:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Ancak bu rezervasyon indirimi, DSv2 serisi yüksek bellekteki SKU 'Lar gibi farklı örnek boyutu esneklik gruplarında listelenen VM boyutlarına uygulanmaz: Standard_DS11_v2, Standard_DS12_v2 vb.

Örnek boyutu esneklik grubu içinde, rezervasyon iskontosunun uygulandığı VM 'lerin sayısı, bir rezervasyon satın aldığınızda seçtiğiniz VM boyutuna bağlıdır. Ayrıca, çalıştırdığınız VM 'lerin boyutlarına bağlıdır. Oran sütunu, söz konusu örnek boyutu esneklik grubundaki her VM boyutu için göreli parmak izini karşılaştırır. Rezervasyon iskontosunun çalıştırdığınız VM 'lere nasıl uygulanacağını hesaplamak için oran değerini kullanın.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler DSv2-Series tablosundaki boyutları ve oranları kullanır.

Bir ayrılmış sanal makine örneğini satın alarak, bu dizideki diğer boyutlarla karşılaştırılan oran veya göreli parmak izini içeren boyut Standard_DS4_v2 satın alabilirsiniz.

- Senaryo 1:1 oranına sahip sekiz Standard_DS1_v2 boyutlu VM çalıştırma. Rezervasyon indiriminiz bu VM 'lerin sekiz ' unun tamamına uygulanır.
- 2. Senaryo: iki Standard_DS2_v2 boyutlu sanal makineyi 2 ' nin oranıyla çalıştırın. Ayrıca 4 oranında bir Standard_DS3_v2 boyutlu bir VM çalıştırın. Toplam parmak izi 2 + 2 + 4 = 8 ' dir. Böylece rezervasyon indiriminiz bu sanal makinelerin üçü için geçerlidir.
- Senaryo 3: bir Standard_DS5_v2 16 oranıyla çalıştırın. Rezervasyon indiriminiz, sanal makinenin işlem maliyetinin yarısı için geçerlidir.

Aşağıdaki bölümlerde, örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir VM örneğini satın aldığınızda, aynı boyut serisi grubundaki boyutlar gösterilmektedir.

## <a name="instance-size-flexibility-ratio-for-vms"></a>VM 'Ler için örnek boyutu esneklik oranı 

Aşağıdaki CSV 'nin örnek boyutu esneklik grupları, ArmSkuName ve oranlar vardır.  

[Örnek boyutu esneklik oranları](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Dosyayı programlı olarak kullanabilmeniz için Azure bağlantıyı ve şemayı güncelledi.

## <a name="view-vm-size-recommendations"></a>VM boyut önerilerini görüntüle

Azure, satın alma deneyiminde VM boyut önerilerini gösterir. En küçük boyut önerilerini görüntülemek için, **en küçük boyuta göre Gruplandır**' ı seçin.

:::image type="content" source="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" alt-text="Önerilen miktarları gösteren ekran görüntüsü." lightbox="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" :::

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure ayırmaları](../cost-management-billing/reservations/save-compute-costs-reservations.md)nelerdir.
