---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: a98a941477fc83a104b55ed91f457c5a48f90d59
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029878"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Ayrılmış VM örnekleriyle sanal makine boyutu esnekliği

Örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir sanal makine örneğiyle, satın aldığınız rezervasyon aynı örnek boyutu esneklik grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Örneğin, DSv2 serisinde Standard_DS5_v2 gibi listelenen bir VM boyutu için bir ayırma satın alırsanız, rezervasyon iskontosu aynı örnek boyutu esneklik grubunda listelenen diğer dört boyuta de uygulanabilir:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Ancak bu rezervasyon indirimi, DSv2 serisi yüksek bellekteki SKU 'Lar gibi farklı örnek boyutu esneklik gruplarında listelenen VM boyutlarına uygulanmaz: Standard_DS11_v2, Standard_DS12_v2 vb.

Örnek boyutu esneklik grubu içinde, rezervasyon iskontosunun uygulandığı VM 'lerin sayısı, bir rezervasyon satın aldığınızda seçtiğiniz VM boyutuna bağlıdır. Ayrıca, çalıştırdığınız VM 'lerin boyutlarına bağlıdır. Oran sütunu, söz konusu örnek boyutu esneklik grubundaki her VM boyutu için göreli parmak izini karşılaştırır. Rezervasyon iskontosunun çalıştırdığınız VM 'lere nasıl uygulanacağını hesaplamak için oran değerini kullanın.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler DSv2-Series tablosundaki boyutları ve oranları kullanır.

Standard_DS4_v2 boyutu ile ayrılmış bir sanal makine örneği satın alabilirsiniz. Bu, söz konusu serideki diğer boyutlarla karşılaştırılan oran veya göreli bir parmak izi 8 ' dir.

- Senaryo 1: 1 oranına sahip sekiz Standard_DS1_v2 boyutlu VM çalıştırın. Rezervasyon indiriminiz bu VM 'lerin sekiz ' unun tamamına uygulanır.
- Senaryo 2: İki Standard_DS2_v2 boyutlu sanal makineyi 2 oranında bir oranıyla çalıştırın. Ayrıca 4 oranında bir Standard_DS3_v2 boyutlu VM çalıştırın. Toplam parmak izi 2 + 2 + 4 = 8 ' dir. Böylece rezervasyon indiriminiz bu sanal makinelerin üçü için geçerlidir.
- Senaryo 3: 16 oranında bir Standard_DS5_v2 çalıştırın. Rezervasyon indiriminiz, sanal makinenin işlem maliyetinin yarısı için geçerlidir.

Aşağıdaki bölümlerde, örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir VM örneğini satın aldığınızda, aynı boyut serisi grubundaki boyutlar gösterilmektedir.

## <a name="instance-size-flexibility-ratio-for-vms"></a>VM 'Ler için örnek boyutu esneklik oranı 

Aşağıdaki CSV 'nin örnek boyutu esneklik grupları, ArmSkuName ve oranlar vardır.  

[Örnek boyutu esneklik oranları](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Bu dosyayı programlama yoluyla kullanabilmeniz için dosya URL 'sini ve şemayı sabit tutacağız. Veriler yakında API aracılığıyla da kullanılabilir.
