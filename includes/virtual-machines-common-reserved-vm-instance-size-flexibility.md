---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02-18-2020
ms.openlocfilehash: aebe8f05bd1a3607da6d1153c87490891fc6b299
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471473"
---
Ayrılmış bir VM Örneği satın aldığınızda, örneğin boyutu esnekliğini veya kapasite önceliğini en iyi duruma getirmeyi seçebilirsiniz. Ayrılmış VM örnekleri için en iyi duruma getirme ayarı ayarlama veya değiştirme hakkında daha fazla bilgi için [bkz.](../articles/cost-management-billing/reservations/manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances)

Örneğin boyut esnekliği için optimize edilmiş ayrılmış bir sanal makine örneği yle, satın aldığınız rezervasyon aynı örnek boyutu esneklik grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Örneğin, Standard_DS5_v2 gibi DSv2 Serisi'nde listelenen bir VM boyutu için rezervasyon satın alırsanız, rezervasyon indirimi aynı örnek boyutu esneklik grubunda listelenen diğer dört boyut için geçerli olabilir:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Ancak bu rezervasyon indirimi, DSv2 Serisi Yüksek Bellekteki SK'ler gibi farklı örnek boyutu esneklik gruplarında listelenen VM boyutları için geçerli değildir: Standard_DS11_v2, Standard_DS12_v2, ve benzeri.

Örnek boyutu esneklik grubunda, rezervasyon indiriminin geçerli olduğu VM sayısı, rezervasyon satın alırken seçtiğiniz VM boyutuna bağlıdır. Ayrıca, çalıştırdığınız VM'lerin boyutlarına da bağlıdır. Oran sütunu, o örnek boyutu esneklik grubundaki her VM boyutu için göreli ayak izini karşılaştırır. Rezervasyon indiriminin çalıştırdığınız VM'ler için nasıl uygulandığını hesaplamak için oran değerini kullanın.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde DSv2 serisi tablosundaki boyut lar ve oranlar kullanılır.

Bu serideki diğer boyutlara göre oranın veya göreli ayak izinin 8 olduğu Standard_DS4_v2 boyutuna sahip ayrılmış bir VM örneği satın alırsınız.

- Senaryo 1: Sekiz Standard_DS1_v2 boyutlu VM'leri 1'e göre çalıştırın. Rezervasyon indiriminiz bu sekiz VM için de geçerlidir.
- Senaryo 2: Her biri 2'lik bir orana sahip iki Standard_DS2_v2 boyutlu VM çalıştırın. Ayrıca 4 oranı ile Standard_DS3_v2 boyutlu vm çalıştırın. Toplam ayak izi 2+2+4=8'dir. Yani rezervasyon indirimi bu vm'lerin üçü için de geçerlidir.
- Senaryo 3: 16 oranı ile bir Standard_DS5_v2 çalıştırın. Rezervasyon indiriminiz VM'nin işlem maliyetinin yarısı için geçerlidir.

Aşağıdaki bölümler, örnek boyutu esnekliği için optimize edilmiş ayrılmış bir VM örneği satın aldığınızda aynı boyut serisi grubunda hangi boyutların olduğunu gösterir.

## <a name="instance-size-flexibility-ratio-for-vms"></a>VM'ler için örnek boyutu esneklik oranı 

CSV aşağıda örnek boyutu esneklik grupları, ArmSkuName ve oranları vardır.  

[Örnek boyut esnekliği oranları](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Bu dosyayı programlı olarak tüketebilmeniz için dosya URL'sini ve şema sabit tutacağız. Veriler yakında API aracılığıyla da sunulacaktır.
