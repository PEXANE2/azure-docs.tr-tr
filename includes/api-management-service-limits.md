---
ms.openlocfilehash: ee44cd4ee5ec6fedfec46cbbb68b0c30118a4b9f
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75904437"
---
---

Başlık: içerme dosyası açıklaması: içerme dosya hizmetleri: API-Management Author: vladvino

MS. assetid: 1b813833-39c8-46be-8666-fd0960cfbf04 MS. Service: API-Management MS. Topic: MS. Date: 01/10/2020 MS. Author: vlvinlogr MS. Custom: Include File
---| Kaynak | Sınır |
| ---------------------------------------------------------------------- | -------------------------- |
| En fazla ölçek birimi sayısı | Bölge başına 10<sup>1</sup> |
| Önbellek boyutu | Birim<sup>2</sup> başına 5 gib |
| HTTP yetkilisi başına eşzamanlı arka uç bağlantıları<sup>3</sup> | <sup>4</sup> birim başına 2.048 |
| Önbelleğe alınan en büyük yanıt boyutu | 2 MIB |
| En fazla ilke belgesi boyutu | 256 KiB<sup>5</sup> |
| Hizmet örneği başına en fazla özel ağ geçidi etki alanı<sup>6</sup> | 20 |
| Hizmet örneği başına en fazla CA sertifikası sayısı | 10 |
| Abonelik başına en fazla hizmet örneği sayısı<sup>7</sup> | 20 |
| Hizmet örneği başına en fazla abonelik sayısı<sup>7</sup> | 500 |
| Hizmet örneği başına en fazla istemci sertifikası sayısı<sup>7</sup> | 50 |
| Hizmet örneği başına en fazla API sayısı<sup>7</sup> | 50 |
| Hizmet örneği başına en fazla API işlemi sayısı<sup>7</sup> | 1000 |
| En fazla toplam istek süresi<sup>7</sup> | 30 saniye |
| En fazla arabelleğe alınmış yük boyutu<sup>7</sup> | 2 MIB |
| En yüksek istek URL boyutu<sup>8</sup> | 4096 bayt |

<sup>1</sup> Ölçek sınırları, fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanlarını ve ölçekleme sınırlarını görmek için bkz. [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> Birim başına önbellek boyutu fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanlarını ve ölçekleme sınırlarını görmek için bkz. [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> Bağlantılar havuza alınır ve arka uç tarafından açıkça kapanmadığı takdirde yeniden kullanılır.<br/>
<sup>4</sup> Bu sınır, temel, standart ve Premium katmanların birimi başına olur. Geliştirici katmanı 1.024 ile sınırlıdır. Bu sınır, tüketim katmanına uygulanmaz.<br/>
<sup>5</sup> Bu sınır, temel, standart ve Premium katmanları için geçerlidir. Tüketim katmanında, ilke belgesi boyutu 4 KiB ile sınırlıdır.<br/>
<sup>6</sup> Bu kaynak yalnızca Premium katmanda kullanılabilir.<br/>
<sup>7</sup> Bu kaynak yalnızca tüketim katmanı için geçerlidir.<br/>
<sup>8</sup> Yalnızca tüketim katmanı için geçerlidir. En fazla 2048 bayt uzunluğunda sorgu dizesi içerir.<br/>
