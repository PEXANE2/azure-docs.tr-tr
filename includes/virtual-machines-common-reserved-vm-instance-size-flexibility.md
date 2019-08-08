---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 790fb39c3d8964c053ffe1d7ee04418fcbc0913c
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857515"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Ayrılmış VM örnekleriyle sanal makine boyutu esnekliği

Örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir sanal makine örneğiyle, satın aldığınız rezervasyon aynı boyut serisi grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Örneğin, DSv2-Series tablosunda listelenen bir VM boyutu için Standard_DS5_v2 gibi bir ayırma satın aldıysanız, rezervasyon iskontosu aynı tabloda listelenen diğer dört boyuta de uygulanabilir:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Ancak bu rezervasyon indirimi, DSv2 serisi yüksek bellek tablosunda yer alan farklı tablolarda listelenen VM boyutlarına uygulanmaz: Standard_DS11_v2, Standard_DS12_v2 vb.

Boyut Serisi grubu içinde, rezervasyon iskontosunun uygulandığı sanal makinelerin sayısı, bir rezervasyon satın alırken seçtiğiniz VM boyutuna bağlıdır. Ayrıca, çalıştırdığınız VM 'lerin boyutlarına bağlıdır. Aşağıdaki tablolarda listelenen oran sütunu, ilgili gruptaki her VM boyutu için göreli parmak izini karşılaştırır. Rezervasyon iskontosunun çalıştırdığınız VM 'lere nasıl uygulanacağını hesaplamak için oran değerini kullanın.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler DSv2-Series tablosundaki boyutları ve oranları kullanır.

 Standard_DS4_v2 boyutu ile ayrılmış bir sanal makine örneği satın alabilirsiniz. Bu, söz konusu serideki diğer boyutlarla karşılaştırılan oran veya göreli bir parmak izi 8 ' dir.

- Senaryo 1: 1 oranına sahip sekiz Standard_DS1_v2 boyutlu VM çalıştırın. Rezervasyon indiriminiz bu VM 'lerin sekiz ' unun tamamına uygulanır.
- Senaryo 2: İki Standard_DS2_v2 boyutlu sanal makineyi 2 oranında bir oranıyla çalıştırın. Ayrıca 4 oranında bir Standard_DS3_v2 boyutlu VM çalıştırın. Toplam parmak izi 2 + 2 + 4 = 8 ' dir. Böylece rezervasyon indiriminiz bu sanal makinelerin üçü için geçerlidir.
- Senaryo 3: 16 oranında bir Standard_DS5_v2 çalıştırın. Rezervasyon indiriminiz, sanal makinenin işlem maliyetinin yarısı için geçerlidir.

Aşağıdaki bölümlerde, örnek boyutu esnekliği için en iyi duruma getirilmiş ayrılmış bir VM örneğini satın aldığınızda, aynı boyut serisi grubundaki boyutlar gösterilmektedir.

## <a name="b-series"></a>B serisi

| Size | Sabit|
|---|---|
| Standard_B1s | 1 |
|Standard_B2s|4|

Daha fazla bilgi için bkz. [B serisi Burstable sanal makine boyutları](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="b-series-high-memory"></a>B serisi yüksek bellek

| Size | Sabit|
|---|---|
| Standard_B1ms |1\.|
|Standard_B2ms|4|
|Standard_B4ms|8|
|Standard_B8ms|16|

Daha fazla bilgi için bkz. [B serisi Burstable sanal makine boyutları](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="d-series"></a>D Serisi

| Size | Sabit|
|---|---|
| Standard_D1|1\.|
|Standard_D2|2|
|Standard_D3|4|
|Standard_D4|8|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="d-series-high-memory"></a>D serisi yüksek bellek

| Size | Sabit|
|---|---|
| Standard_D11|1\.|
|Standard_D12|2|
|Standard_D13|4|
|Standard_D14|8|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series"></a>DS serisi

| Size | Sabit|
|---|---|
|Standard_DS1|1\.|
|Standard_DS2|2|
|Standard_DS3|4|
|Standard_DS4|8|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series-high-memory"></a>DS serisi yüksek bellek

| Size | Sabit|
|---|---|
|Standard_DS11|1|
|Standard_DS12|2|
|Standard_DS13|4|
|Standard_DS14|8|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series"></a>DSv2 serisi

| Size | Sabit|
|---|---|
|Standard_DS1_v2|1\.|
|Standard_DS2_v2|2|
|Standard_DS3_v2|4|
|Standard_DS4_v2|8|
|Standard_DS5_v2|16|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series-high-memory"></a>DSv2 serisi yüksek bellek

| Size | Sabit|
|---|---|
|Standard_DS11_v2|1\.|
|Standard_DS11-1_v2|1\.|
|Standard_DS12_v2|2|
|Standard_DS12-1_v2|2|
|Standard_DS12-2_v2|2|
|Standard_DS13_v2|4|
|Standard_DS13-2_v2|4|
|Standard_DS13-4_v2|4|
|Standard_DS14_v2|8|
|Standard_DS14-4_v2|8|
|Standard_DS14-8_v2|8|
|Standard_DS15_v2|10|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-high-memory-isolated-series"></a>DSv2 yüksek bellek yalıtılmış-serisi

| Size | Sabit|
|---|---|
|Standard_DS15i_v2|1\.|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv3-series"></a>DSv3 serisi

| Size | Sabit|
|---|---|
|Standard_D2s_v3|1\.|
|Standard_D4s_v3|2|
|Standard_D8s_v3|4|
|Standard_D16s_v3|8|
|Standard_D32s_v3|16|
|Standard_D64s_v3|32|

Daha fazla bilgi için bkz. [genel amaçlı sanal makine boyutları](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-1).

## <a name="dv2-series"></a>Dv2 Serisi

| Size | Sabit|
|---|---|
|Standard_D1_v2|1|
|Standard_D2_v2|2|
|Standard_D3_v2|4|
|Standard_D4_v2|8|
|Standard_D5_v2|16|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2-series-high-memory"></a>Dv2 serisi yüksek bellek

| Size | Sabit|
|---|---|
|Standard_D11_v2|1\.|
|Standard_D12_v2|2|
|Standard_D13_v2|4|
|Standard_D14_v2|8|
|Standard_D15_v2|10|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2--high-memory-isolated-series"></a>Dv2 yüksek bellek yalıtılmış-serisi

| Size | Sabit|
|---|---|
|Standard_D15i_v2|1|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).


## <a name="dv3-series"></a>Dv3 serisi

| Size | Sabit|
|---|---|
| Standard_D2_v3|1\.|
|Standard_D4_v3|2|
|Standard_D8_v3|4|
|Standard_D16_v3|8|
|Standard_D32_v3|16|
|Standard_D64_v3|32|

Daha fazla bilgi için bkz. [genel amaçlı sanal makine boyutları](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1).

## <a name="esv3-series"></a>ESv3 serisi

| Size | Sabit|
|---|---|
|Standard_E2s_v3|1\.|
|Standard_E4s_v3|2|
|Standard_E4-2s_v3|2|
|Standard_E8s_v3|4|
|Standard_E8-2s_v3|4|
|Standard_E8-4s_v3|4|
|Standard_E16s_v3|8|
|Standard_E16-4s_v3|8|
|Standard_E16-8s_v3|8|
|Standard_E20s_v3|10|
|Standard_E32s_v3|16|
|Standard_E32-8s_v3|16|
|Standard_E32-16s_v3|16|
|Standard_E64s_v3|28,8|
|Standard_E64-16s_v3|28,8|
|Standard_E64-32s_v3|28,8|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#esv3-series).

## <a name="esv3-series-isolated-series"></a>ESv3 serisi yalıtılmış-seriler

| Size | Sabit|
|---|---|
|Standard_E64is_v3|1\.|

## <a name="ev3-series"></a>Ev3 serisi

| Size | Sabit|
|---|---|
|Standard_E2_v3|1\.|
|Standard_E4_v3|2|
|Standard_E8_v3|4|
|Standard_E16_v3|8|
|Standard_E20_v3|10|
|Standard_E32_v3|16|
|Standard_E64_v3|32|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#ev3-series).

## <a name="ev3-series-isolated-series"></a>Ev3 serisi yalıtılmış-seriler

| Size | Sabit|
|---|---|
|Standard_E64i_v3|1\.|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#ev3-series).

## <a name="f-series"></a>F Serisi

| Size | Sabit|
|---|---|
| Standard_F1|1\.|
|Standard_F2|2|
|Standard_F4|4|
|Standard_F8|8|
Standard_F16|16|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fs-series"></a>FS Serisi

| Size | Sabit|
|---|---|
| Standard_F1s|1\.|
|Standard_F2s|2|
|Standard_F4s|4|
|Standard_F8s|8|
|Standard_F16s|16|

Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fsv2-series"></a>Fsv2-serisi

| Size | Sabit|
|---|---|
|Standard_F2s_v2|1\.|
|Standard_F4s_v2|2|
|Standard_F8s_v2|4|
|Standard_F16s_v2|8|
|Standard_F32s_v2|16|
|Standard_F64s_v2|32|
|Standard_F72s_v2|36|

Daha fazla bilgi için bkz. [işlem için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-compute.md#fsv2-series-1).

## <a name="h-series"></a>H Serisi

| Size | Sabit|
|---|---|
| Standard_H8|1\.|
|Standard_H16|2|

Daha fazla bilgi için bkz. [yüksek performanslı Işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="h-series-high-memory"></a>H serisi yüksek bellek

| Size | Sabit|
|---|---|
| Standard_H8m|1|
|Standard_H16m|2|

Daha fazla bilgi için bkz. [yüksek performanslı Işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).


## <a name="hcs-series"></a>HCS serisi

| Size | Sabit|
|---|---|
|Standard_HC44rs|1\.|

Daha fazla bilgi için bkz. [yüksek performanslı Işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="hbs-series"></a>HBS Serisi

| Size | Sabit|
|---|---|
|Standard_HB60rs|1\.|

Daha fazla bilgi için bkz. [yüksek performanslı Işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="hbs-series"></a>HBS Serisi

| Size | Sabit|
|---|---|
|Standard_HB60rs|1\.|

Daha fazla bilgi için bkz. [yüksek performanslı Işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="h-series-low-latency-series"></a>H serisi düşük gecikme-serisi

| Size | Sabit|
|---|---|
|Standard_H16r|1\.|

Daha fazla bilgi için bkz. [depolama için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="h-series-high-memory-low-latency-series"></a>H serisi yüksek bellek düşük gecikme-serisi

| Size | Sabit|
|---|---|
|Standard_H16mr|1\.|

Daha fazla bilgi için bkz. [depolama için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="h-series"></a>H Serisi

| Size | Sabit|
|---|---|
|Standard_H8|1\.|
|Standard_H16|2|

Daha fazla bilgi için bkz. [depolama için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="lsv2-series-series"></a>LSv2 serisi serisi

| Size | Sabit|
|---|---|
|Standard_L8s_v2|1\.|
|Standard_L16s_v2|2|
|Standard_L32s_v2|4|
|Standard_L48s_v2|6|
|Standard_L64s_v2|8|
|Standard_L80s_v2|10|
|Standard_L96s_v2|12|

Daha fazla bilgi için bkz. [depolama için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-storage.md#lsv2-series).

## <a name="m-series"></a>M serisi

| Size | Sabit|
|---|---|
| Standard_M64s|1\.|
|Standard_M128s|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional"></a>A serisi kesir

| Size | Sabit|
|---|---|
| Standard_M16s|1\.|
|Standard_M32s|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-high-memory"></a>A serisi kesirli yüksek bellek

| Size | Sabit|
|---|---|
|Standard_M8ms|1\.|
|Standard_M8-2ms|1\.|
|Standard_M8-4ms|1\.|
|Standard_M16ms|2|
|Standard_M16-4ms|2|
|Standard_M16-8ms|2|
|Standard_M32ms|4|
|Standard_M32-8ms|4|
|Standard_M32-16ms|4|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-large"></a>A serisi kesirli büyük

| Size | Sabit|
|---|---|
|Standard_M32ls|1\.|
|Standard_M64ls|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-high-memory"></a>D serisi yüksek bellek

| Size | Sabit|
|---|---|
|Standard_M64ms|1\.|
|Standard_M64-16ms|1\.|
|Standard_M64-32ms|1\.|
|Standard_M128ms|2|
|Standard_M128-32ms|2|
|Standard_M128-64ms|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="ms-series-fractional-tiny"></a>MS Serisi Kesirli Küçük

| Size | Sabit|
|---|---|
|Standard_M32ls|1\.|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="msv2-series-high-memory-series"></a>MSv2 serisi yüksek bellek serisi

| Size | Sabit|
|---|---|
|Standard_M208ms_v2|1\.|
|Standard_M416ms_v2|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="msv2-series"></a>MSv2 serisi

| Size | Sabit|
|---|---|
|Standard_M208s_v2|1\.|
|Standard_M416s_v2|2|

Daha fazla bilgi için bkz. [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="nc-series"></a>NC serisi

| Size | Sabit|
|---|---|
|Standard_NC6|1\.|
|Standard_NC12|2|
|Standard_NC24|4|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-gpu.md).

## <a name="ncv2-series"></a>NCv2 serisi

| Size | Sabit|
|---|---|
| Standard_NC6s_v2|1\.|
|Standard_NC12s_v2|2|
|Standard_NC24s_v2|4|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).

## <a name="ncv3-series"></a>NCv3 serisi

| Size | Sabit|
|---|---|
| Standard_NC6s_v3|1\.|
|Standard_NC12s_v3|2|
|Standard_NC24s_v3|4|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series).

## <a name="nd-series"></a>ND serisi

| Size | Sabit|
|---|---|
| Standard_ND6s|1\.|
|Standard_ND12s|2|
|Standard_ND24s|4|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#nd-series).

## <a name="nv-series"></a>NV serisi

| Size | Sabit|
|---|---|
| Standard_NV6|1\.|
|Standard_NV12|2|
|Standard_NV24|4|

## <a name="nvsv3-series"></a>NVSv3 serisi

| Size | Sabit|
|---|---|
|Standard_NV12s_v3|1\.|
|Standard_NV24s_v3|2|
|Standard_NV48s_v3|4|

## <a name="nds-series-low-latency-series"></a>NDS serisi düşük gecikme-serisi

| Size | Sabit|
|---|---|
|Standard_ND24rs|1\.|

## <a name="ncsv3-series-low-latency-series"></a>NCSv3 serisi düşük gecikme süresi-serisi

| Size | Sabit|
|---|---|
|Standard_NC24rs_v3|1\.|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series).

## <a name="ncsv2-series-low-latency-series"></a>NCSv2 serisi düşük gecikme süresi-serisi

| Size | Sabit|
|---|---|
|Standard_NC24rs_v2|1\.|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).

## <a name="nc-series-low-latency-series"></a>NC serisi düşük gecikme-serisi

| Size | Sabit|
|---|---|
|Standard_NC24r|1|

Daha fazla bilgi için bkz. [GPU iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).





