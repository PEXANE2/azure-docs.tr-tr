---
title: include dosyası
description: include dosyası
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 01/10/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 5608683578a796ae0fffff7544a749043e8698fd
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656197"
---
| Kaynak | Sınır |
| ---------------------------------------------------------------------- | -------------------------- |
| Maksimum ölçek birimi sayısı | Bölge başına 10<sup>1</sup> |
| Önbellek boyutu | Birim başına 5 GİB<sup>2</sup> |
| Http yetkilisi başına eşzamanlı arka uç bağlantıları<sup>3</sup> | Birim<sup>4</sup> başına 2.048 |
| Maksimum önbelleğe alınmış yanıt boyutu | 2 MiB |
| Maksimum ilke belge boyutu | 256 KiB<sup>5</sup> |
| Hizmet başına maksimum özel ağ geçidi etki alanı örneği<sup>6</sup> | 20 |
| Hizmet örneği başına maksimum CA sertifikası sayısı | 10 |
| Abonelik başına maksimum hizmet örneği sayısı<sup>7</sup> | 20 |
| Hizmet başına maksimum abonelik sayısı örneği<sup>7</sup> | 500 |
| Hizmet başına maksimum istemci sertifikası sayısı örneği<sup>7</sup> | 50 |
| Hizmet başına maksimum API sayısı örneği<sup>7</sup> | 50 |
| Hizmet başına maksimum API işlemi sayısı örneği<sup>7</sup> | 1000 |
| Maksimum toplam istek süresi<sup>7</sup> | 30 saniye |
| Maksimum arabelleğe alınan yük boyutu<sup>7</sup> | 2 MiB |
| Maksimum istek URL boyutu<sup>8</sup> | 4096 bayt |

<sup>1.1.2</sup> Ölçeklendirme sınırları fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanlarını ve ölçeklendirme sınırlarını görmek için [API Yönetimi fiyatlandırmasına](https://azure.microsoft.com/pricing/details/api-management/)bakın.<br/>
<sup>2.000</sup> Birim önbellek boyutu başına fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanlarını ve ölçeklendirme sınırlarını görmek için [API Yönetimi fiyatlandırmasına](https://azure.microsoft.com/pricing/details/api-management/)bakın.<br/>
<sup>3.2.2</sup> Bağlantılar, arka uç tarafından açıkça kapatılmadığı sürece bir araya getirilir ve yeniden kullanılır.<br/>
<sup>4.2.2</sup> Bu sınır, Temel, Standart ve Premium katmanların birimi başına dır. Geliştirici katmanı 1.024 ile sınırlıdır. Bu sınır Tüketim katmanı için geçerli değildir.<br/>
<sup>5.000</sup> Bu sınır Temel, Standart ve Premium katmanlar için geçerlidir. Tüketim katmanında, ilke belge boyutu 16 KiB ile sınırlıdır.<br/>
<sup>6.000</sup> Bu kaynak yalnızca Premium katmanında kullanılabilir.<br/>
<sup>7.000</sup> Bu kaynak yalnızca Tüketim katmanı için geçerlidir.<br/>
<sup>8.000</sup> Yalnızca Tüketim katmanı için geçerlidir. 2048'e kadar bayt uzun sorgu dizesi içerir.<br/>
