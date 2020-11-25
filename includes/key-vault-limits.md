---
author: rothja
ms.service: key-vault
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: e4abbeadb0d30911d99fff57c0e99a3e427a6d8d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027356"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Anahtar işlemleri (bölge başına en fazla 10 saniye içinde izin verilen işlem sayısı<sup>1</sup>):

|Anahtar türü|HSM anahtarı<br>Anahtar oluştur|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluştur|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2.048 bit|5|1.000|10|2.000|
|RSA 3.072 bit|5|250|10|500|
|RSA 4.096 bit|5|125|10|250|
|ECC P-256|5|1.000|10|2.000|
|ECC P-384|5|1.000|10|2.000|
|ECC P-521|5|1.000|10|2.000|
|ECC SECP256K1|5|1.000|10|2.000|

> [!NOTE]
> Önceki tabloda, RSA 2.048-bit yazılım anahtarları için, 2.000/10 saniyeye kadar işlem yapılmasına izin verileceğini görüyoruz. RSA 2.048 bit HSM anahtarları için, her 10 saniye başına 1.000 al işlem yapılmasına izin verilir.
>
> Daraltma eşikleri ağırlıklı olur ve zorlamasının toplam üzerinde olması gerekir. Örneğin, önceki tabloda gösterildiği gibi, RSA HSM anahtarları üzerinde al işlemleri gerçekleştirdiğinizde, 2.048-bit anahtarlara kıyasla 4.096 bitlik anahtarların kullanılması sekiz kat daha pahalıdır. Bunun nedeni 1000/125 = 8 ' dir.
>
> Belirli bir 10 saniyelik aralıkta, bir Azure Key Vault istemci, azaltma HTTP durum kodu ile karşılaşmadan önce aşağıdaki işlemlerden *yalnızca birini* gerçekleştirebilir `429` :
> - 2.000 RSA 2.048-bit yazılım-anahtar al işlemler
> - 1.000 RSA 2.048-bit HSM-anahtar al işlemleri
> - 125 RSA 4.096-bit HSM-anahtar al işlemleri
> - 124 RSA 4.096-bit HSM-anahtar al işlem ve 8 RSA 2.048-bit HSM-anahtar al işlem

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Gizli dizileri, yönetilen depolama hesabı anahtarları ve kasa işlemleri:

| İşlem türü | Bölge başına en fazla 10 saniye içinde izin verilen işlem sayısı<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2.000 |

Bu sınırlar aşıldığında azaltmayı işleme hakkında daha fazla bilgi için bkz. [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/general/overview-throttling.md).

<sup>1</sup> tüm işlem türleri için abonelik genelinde sınır, Anahtar Kasası sınırı başına beş kat olur. Örneğin, HSM-abonelik başına diğer işlemler, abonelik başına 10 saniye içinde 5.000 işlem ile sınırlıdır.

### <a name="azure-private-link-integration"></a>Azure özel bağlantı tümleştirmesi

> [!NOTE]
> Abonelik başına etkin olan özel uç noktaları olan anahtar kasalarının sayısı, ayarlanabilir bir kısıtlamadır. Aşağıda gösterilen sınır varsayılan sınırdır. Hizmetiniz için sınır artışı istemek istiyorsanız lütfen adresine bir e-posta gönderin akv-privatelink@microsoft.com . Bu istekleri durum temelinde onaylayacağız.

| Kaynak | Sınır |
| -------- | ----- |
| Anahtar Kasası başına özel uç noktalar | 64 |
| Abonelik başına özel uç noktalara sahip anahtar kasaları | 400 |