---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: 8247b1cedc2c5ebc8577af6be485aed0fcd5d6af
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768763"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Anahtar hareketler (10 saniyede izin verilen maksimum işlemler, bölge başına kasa başına<sup>1):</sup>

|Anahtar türü|HSM tuşu<br>CREATE anahtarı|HSM tuşu<br>Diğer tüm işlemler|Yazılım anahtarı<br>CREATE anahtarı|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2,048-bit|5|1000|10|2.000|
|RSA 3,072-bit|5|250|10|500|
|RSA 4,096-bit|5|125|10|250|
|ECC P-256|5|1000|10|2.000|
|ECC P-384|5|1000|10|2.000|
|ECC P-521|5|1000|10|2.000|
|ECC SECP256K1|5|1000|10|2.000|

> [!NOTE]
> Önceki tabloda, RSA 2.048 bit yazılım anahtarları için, 10 saniyede 2.000 GET işlemine izin verildiğini görüyoruz. RSA 2.048 bit HSM tuşları için 10 saniyede 1.000 GET işlemine izin verilir.
>
> Azaltma eşikleri ağırlıklıdır ve uygulama bunların toplamındadır. Örneğin, önceki tabloda gösterildiği gibi, RSA HSM tuşlarında GET işlemleri gerçekleştirdiğinizde, 2.048 bit anahtarlara kıyasla 4.096 bit tuşları kullanmak sekiz kat daha pahalıdır. Çünkü 1.000/125 = 8.
>
> Belirli bir 10 saniyelik aralıkta, bir *only one* Azure Anahtar Kasası istemcisi `429` azaltma http durum koduyla karşılaşmadan önce aşağıdaki işlemlerden yalnızca birini yapabilir:
> - 2.000 RSA 2.048 bit yazılım anahtarlı GET işlemleri
> - 1.000 RSA 2.048 bit HSM-anahtar GET işlemleri
> - 125 RSA 4.096 bit HSM-anahtar GET işlemleri
> - 124 RSA 4.096 bit HSM-key GET işlemleri ve 8 RSA 2.048 bit HSM-key GET işlemleri

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Sırlar, yönetilen depolama hesabı anahtarları ve kasa işlemleri:

| İşlemtürü | 10 saniyede izin verilen maksimum işlemler, bölge başına kasa başına<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2.000 |

Bu sınırlar aşıldığında azaltmanın nasıl işleyeceğiniz hakkında bilgi için [Azure Anahtar Kasası azaltma kılavuzuna](../articles/key-vault/key-vault-ovw-throttling.md)bakın.

<sup>1</sup> Tüm işlem türleri için abonelik genelindeki sınır, anahtar kasa sınırı başına beş katıdır. Örneğin, abonelik başına HSM-diğer işlemler abonelik başına 10 saniye içinde 5.000 işlemle sınırlıdır.

### <a name="azure-private-link-integration"></a>Azure Özel Bağlantı tümleştirmesi

| Kaynak | Sınır |
| -------- | ----- |
| Anahtar kasası başına özel uç noktaları | 64 |
| Abonelik başına özel uç noktaları olan anahtar kasaları | 64 |
