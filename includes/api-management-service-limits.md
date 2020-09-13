---
title: dosya dahil etme
description: dosya dahil etme
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 04/14/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 9ed918c841989cb50efa362251d09c0b655b7fc4
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89570145"
---
| Kaynak | Sınır |
| ---------------------------------------------------------------------- | -------------------------- |
| En fazla ölçek birimi sayısı | Bölge başına 10<sup>1</sup> |
| Önbellek boyutu | Birim<sup>2</sup> başına 5 gib |
| HTTP yetkilisi başına eşzamanlı arka uç bağlantıları<sup>3</sup> | <sup>4</sup> birim başına 2.048 |
| Önbelleğe alınan en büyük yanıt boyutu | 2 MIB |
| En fazla ilke belgesi boyutu | 256 KiB<sup>5</sup> |
| Hizmet örneği başına en fazla özel ağ geçidi etki alanı<sup>6</sup> | 20 |
| Hizmet örneği başına en fazla CA sertifikası sayısı<sup>7</sup> | 10 |
| Abonelik başına en fazla hizmet örneği sayısı<sup>8</sup> | 20 |
| Hizmet örneği başına en fazla abonelik sayısı<sup>8</sup> | 500 |
| Hizmet örneği başına en fazla istemci sertifikası sayısı<sup>8</sup> | 50 |
| Hizmet örneği başına en fazla API sayısı<sup>8</sup> | 50 |
| Hizmet örneği başına en fazla API işlemi sayısı<sup>8</sup> | 1.000 |
| En fazla toplam istek süresi<sup>8</sup> | 30 saniye |
| En fazla arabelleğe alınmış yük boyutu<sup>8</sup> | 2 MIB |
| En yüksek istek URL 'SI boyutu<sup>9</sup> | 4096 bayt |
| Şirket içinde barındırılan ağ geçitlerinin maksimum sayısı<sup>10</sup> | 25 |

<sup>1</sup> Ölçek sınırları, fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanları ve ölçek limitleriyle ilgili ayrıntılar için bkz. [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> Birim başına önbellek boyutu fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanlarını ve ölçekleme sınırlarını görmek için bkz. [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> Bağlantılar havuza alınır ve arka uç tarafından açıkça kapanmadığı takdirde yeniden kullanılır.<br/>
<sup>4</sup> Bu sınır, temel, standart ve Premium katmanların birimi başına olur. Geliştirici katmanı 1.024 ile sınırlıdır. Bu sınır, tüketim katmanına uygulanmaz.<br/>
<sup>5</sup> Bu sınır, temel, standart ve Premium katmanları için geçerlidir. Tüketim katmanında, ilke belgesi boyutu 4 KiB ile sınırlıdır.<br/>
<sup>6</sup> Yalnızca geliştirici ve Premium katmanlarında birden çok özel etki alanı desteklenir.<br/>
<sup>7</sup> CA sertifikaları, tüketim katmanında desteklenmez.<br/>
<sup>8</sup> Bu sınır yalnızca tüketim katmanı için geçerlidir. Bu kategorilerde diğer katmanlara yönelik bir sınır yoktur.<br/>
<sup>9</sup> Yalnızca tüketim katmanı için geçerlidir. En fazla 2048 bayt uzunluğunda sorgu dizesi içerir.<br/>
<sup>10</sup> Şirket içinde barındırılan ağ geçitleri yalnızca geliştirici ve Premium katmanlarda desteklenir. Sınır, [kendinden konak ağ geçidi kaynaklarının](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/gateway)sayısı için geçerlidir. Bu sınırı artırmak için lütfen [desteğe](https://azure.microsoft.com/support/options/)başvurun. Şirket içinde barındırılan bir ağ geçidi kaynağıyla ilişkili düğümlerin (veya çoğaltmaların) sayısının Premium katmanda sınırsız olduğunu ve geliştirici katmanındaki tek bir düğümde bulunduğunu unutmayın.
