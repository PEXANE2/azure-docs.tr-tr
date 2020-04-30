---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: 8247b1cedc2c5ebc8577af6be485aed0fcd5d6af
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81768763"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Anahtar işlemleri (bölge başına en fazla 10 saniye içinde izin verilen işlem sayısı<sup>1</sup>):

|Anahtar türü|HSM anahtarı<br>Anahtar oluştur|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluştur|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2.048 bit|5|1000|10|2.000|
|RSA 3.072 bit|5|250|10|500|
|RSA 4.096 bit|5|125|10|250|
|ECC P-256|5|1000|10|2.000|
|ECC P-384|5|1000|10|2.000|
|ECC P-521|5|1000|10|2.000|
|ECC SECP256K1|5|1000|10|2.000|

> [!NOTE]
> Önceki tabloda, RSA 2.048-bit yazılım anahtarları için, 2.000/10 saniyeye kadar işlem yapılmasına izin verileceğini görüyoruz. RSA 2.048 bit HSM anahtarları için, her 10 saniye başına 1.000 al işlem yapılmasına izin verilir.
>
> Daraltma eşikleri ağırlıklı olur ve zorlamasının toplam üzerinde olması gerekir. Örneğin, önceki tabloda gösterildiği gibi, RSA HSM anahtarları üzerinde al işlemleri gerçekleştirdiğinizde, 2.048-bit anahtarlara kıyasla 4.096 bitlik anahtarların kullanılması sekiz kat daha pahalıdır. Bunun nedeni 1000/125 = 8 ' dir.
>
> Belirli bir 10 saniyelik aralıkta, bir Azure Key Vault istemci, `429` azaltma http durum kodu ile karşılaşmadan önce aşağıdaki işlemlerden *yalnızca birini* gerçekleştirebilir:
> - 2.000 RSA 2.048-bit yazılım-anahtar al işlemler
> - 1.000 RSA 2.048-bit HSM-anahtar al işlemleri
> - 125 RSA 4.096-bit HSM-anahtar al işlemleri
> - 124 RSA 4.096-bit HSM-anahtar al işlem ve 8 RSA 2.048-bit HSM-anahtar al işlem

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Gizli dizileri, yönetilen depolama hesabı anahtarları ve kasa işlemleri:

| İşlem türü | Bölge başına en fazla 10 saniye içinde izin verilen işlem sayısı<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2.000 |

Bu sınırlar aşıldığında azaltmayı işleme hakkında daha fazla bilgi için bkz. [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> tüm işlem türleri için abonelik genelinde sınır, Anahtar Kasası sınırı başına beş kat olur. Örneğin, HSM-abonelik başına diğer işlemler, abonelik başına 10 saniye içinde 5.000 işlem ile sınırlıdır.

### <a name="azure-private-link-integration"></a>Azure özel bağlantı tümleştirmesi

| Kaynak | Sınır |
| -------- | ----- |
| Anahtar Kasası başına özel uç noktalar | 64 |
| Abonelik başına özel uç noktalara sahip anahtar kasaları | 64 |
