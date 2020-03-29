---
title: Azure Key Vault - Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma | Microsoft Dokümanlar
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve aktarmanıza yardımcı olması için bu makaleyi kullanın. Kendi anahtarınızı (BYOK) getir olarak da bilinir.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 08a4330f4a786deca8ddb2f1c6803b29152e7f50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79080139"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>HSM korumalı anahtarları Key Vault’a içeri aktarma (önizleme)

> [!NOTE]
> Bu özellik önizlemededir ve yalnızca *Doğu ABD 2 EUAP* ve *Orta ABD EUAP'si*olan Azure bölgelerinde kullanılabilir. 

Azure Key Vault'u kullandığınızda daha fazla güvence için, donanım güvenlik modülünde (HSM) bir anahtar içe aktarabilir veya oluşturabilirsiniz; anahtar HSM sınırını asla terk edemez. Bu senaryo genellikle *kendi anahtarınızı getir* (BYOK) olarak adlandırılır. Key Vault, anahtarlarınızı korumak için nCipher nShield HSM ailesini (FIPS 140-2 Level 2 doğrulanmış) kullanır.

Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve aktarmanıza yardımcı olması için bu makaledeki bilgileri kullanın.

> [!NOTE]
> Bu işlev Azure China 21Vianet için kullanılamaz. 
> 
> Bu alma yöntemi yalnızca [desteklenen HSM'ler](#supported-hsms)için kullanılabilir. 

Daha fazla bilgi ve Key Vault'u kullanmaya başlamak için bir öğretici için (HSM korumalı anahtarlar için anahtar kasası nasıl oluşturulacak) için Azure [Anahtar Kasası nedir?](key-vault-overview.md)

## <a name="overview"></a>Genel Bakış

İşte sürecin genel görünümü. Tamamlanması gereken belirli adımlar daha sonra makalede açıklanmıştır.

* Key Vault'ta bir anahtar *(Anahtar Değişim Anahtarı* (KEK) olarak anılacaktır) oluşturun. KEK, yalnızca anahtar işlemine `import` sahip bir RSA-HSM anahtarı olmalıdır. RsA-HSM tuşlarını yalnızca Key Vault Premium SKU destekler.
* KEK ortak anahtarını .pem dosyası olarak indirin.
* KEK ortak anahtarını şirket içi HSM'ye bağlı çevrimdışı bir bilgisayara aktarın.
* Çevrimdışı bilgisayarda, bir BYOK dosyası oluşturmak için HSM satıcınız tarafından sağlanan BYOK aracını kullanın. 
* Hedef anahtar, Key Vault HSM'ye aktarılana kadar şifreli kalan bir KEK ile şifrelenir. Anahtarınızın yalnızca şifreli sürümü şirket içi HSM'den ayrılır.
* Key Vault HSM içinde oluşturulan bir KEK dışa aktarılamaz. HSM'ler, Key Vault HSM dışında bir KEK'in net bir sürümünün bulunmadığı kuralını uygular.
* KEK, hedef anahtarın alınacağı aynı anahtar kasasında olmalıdır.
* BYOK dosyası Key Vault'a yüklendiğinde, Key Vault HSM hedef anahtar malzemenin şifresini çözmek ve HSM anahtarı olarak almak için KEK özel anahtarını kullanır. Bu işlem tamamen Key Vault HSM içinde gerçekleşir. Hedef anahtar her zaman HSM koruma sınırında kalır.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki tablo, Azure Anahtar Kasası'nda BYOK'u kullanmak için ön koşulları listeler:

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Bir Azure aboneliği |Azure Key Vault'ta önemli bir kasa oluşturmak için bir Azure aboneliğine ihtiyacınız vardır. [Ücretsiz deneme sürümü için kaydolun.](https://azure.microsoft.com/pricing/free-trial/) |
| HSM korumalı anahtarları almak için Bir Key Vault Premium SKU |Azure Key Vault'taki hizmet katmanları ve yetenekleri hakkında daha fazla bilgi için [Key Vault Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/key-vault/)bakın. |
| Desteklenen HSM listesinden bir HSM ve HSM satıcınız tarafından sağlanan bir BYOK aracı ve yönergeleri | Bir HSM için izinleriniz ve HSM'nizi nasıl kullanacağınız hakkında temel bilgiye sahip olmalısınız. Bkz. [Desteklenen HSM'ler](#supported-hsms). |
| Azure CLI sürüm 2.1.0 veya sonrası | Bkz. [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli?view=azure-cli-latest)|

## <a name="supported-hsms"></a>Desteklenen HSM'ler

|Satıcı adı|Satıcı Türü|Desteklenen HSM modelleri|Daha fazla bilgi|
|---|---|---|---|
|Thales|Üretici|Firmware sürüm 7.3 veya daha sonra ile SafeNet Luna HSM 7 ailesi| [SafeNet Luna BYOK aracı ve dokümantasyonu](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|
|Fortanix|Hizmet Olarak HSM|Kendini Savunma Anahtar Yönetim Hizmeti (SDKMS)|[BYOK için Bulut Sağlayıcılarına SDKMS anahtarlarını dışa aktarma - Azure Key Vault](https://support.fortanix.com/hc/en-us/articles/360040071192-Exporting-SDKMS-keys-to-Cloud-Providers-for-BYOK-Azure-Key-Vault)|


> [!NOTE]
> HSM'lerin nCipher nShield ailesinden HSM korumalı anahtarları almak için [eski BYOK yordamını](hsm-protected-keys-legacy.md)kullanın.

## <a name="supported-key-types"></a>Desteklenen anahtar türleri

|Anahtar adı|Anahtar türü|Anahtar boyutu|Kaynak|Açıklama|
|---|---|---|---|---|
|Anahtar Değişim Anahtarı (KEK)|RSA| 2,048 bit<br />3,072-bit<br />4,096-bit|Azure Anahtar Kasası HSM|Azure Anahtar Kasası'nda oluşturulan HSM destekli RSA anahtar çifti|
|Hedef tuşu|RSA|2,048 bit<br />3,072-bit<br />4,096-bit|Satıcı HSM|Azure Key Vault HSM'ye aktarılacak anahtar|

## <a name="generate-and-transfer-your-key-to-the-key-vault-hsm"></a>Anahtarınızı oluşturun ve Key Vault HSM'ye aktarın

Anahtarınızı oluşturmak ve Key Vault HSM'ye aktarmak için:

* [Adım 1: KEK oluşturma](#step-1-generate-a-kek)
* [Adım 2: KEK ortak anahtarını indirin](#step-2-download-the-kek-public-key)
* [Adım 3: Transfer için anahtarınızı oluşturun ve hazırlayın](#step-3-generate-and-prepare-your-key-for-transfer)
* [Adım 4: Anahtarınızı Azure Anahtar Kasasına Aktarın](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>Adım 1: KEK oluşturma

KEK, Key Vault HSM'de oluşturulan bir RSA anahtarıdır. KEK, almak istediğiniz anahtarı *(hedef* anahtar) şifrelemek için kullanılır.

KEK olmalıdır:
- RsA-HSM anahtarı (2,048-bit; 3,072-bit; veya 4,096-bit)
- Hedef anahtarı almak istediğiniz aynı anahtar kasasında oluşturulmuştur
- İzin verilen anahtar işlemleri ayarlı olarak oluşturuldu`import`

> [!NOTE]
> KEK izin verilen tek anahtar işlemi olarak 'içe aktarma' olmalıdır. 'alma' diğer tüm önemli işlemlerle birbirini dışlar.

Az [keyvault tuşunu](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) kullanarak anahtar işlemleri ayarlanmış bir `import`KEK oluşturmak için komutu kullanın. Aşağıdaki komuttan döndürülen anahtar`kid`tanımlayıcısını kaydedin. (Adım `kid` [3'teki](#step-3-generate-and-prepare-your-key-for-transfer)değeri kullanacaksınız.)

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-the-kek-public-key"></a>Adım 2: KEK ortak anahtarını indirin

.pem dosyasının KEK ortak anahtarını indirmek için [az keyvault tuşunu indirin.](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) Aldığınız hedef anahtar KEK ortak anahtarı kullanılarak şifrelenir.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

KEKforBYOK.publickey.pem dosyasını çevrimdışı bilgisayarınıza aktarın. Bir sonraki adımda bu dosyaya ihtiyacınız olacaktır.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Adım 3: Transfer için anahtarınızı oluşturun ve hazırlayın

BYOK aracını indirmek ve yüklemek için HSM satıcınızın belgelerine bakın. Hedef anahtar oluşturmak için HSM satıcınızın yönergelerini izleyin ve ardından bir anahtar aktarım paketi (BYOK dosyası) oluşturun. BYOK [aracı,](#step-1-generate-a-kek) Adım `kid` [2'de](#step-2-download-the-kek-public-key) indirdiğiniz Adım 1 ve KEKforBYOK.publickey.pem dosyasını kullanarak Bir BYOK dosyasında şifreli bir hedef anahtarı oluşturur.

BYOK dosyasını bağlı bilgisayarınıza aktarın.

> [!NOTE] 
> RSA 1.024 bit anahtarları alma desteklenmez. Şu anda, eliptik eğri (EC) anahtarı alma desteklenmez.
> 
> **Bilinen sorun**: SafeNet Luna HSM'lerden RSA 4K hedef anahtarı almak yalnızca firmware 7.4.0 veya daha yeni ile desteklenir.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>Adım 4: Anahtarınızı Azure Anahtar Kasasına Aktarın

Anahtar alma işlemi tamamlamak için, anahtar aktarım paketini (BYOK dosyası) bağlantısı kesilen bilgisayarınızdan internete bağlı bilgisayara aktarın. BYOK dosyasını Key Vault HSM'ye yüklemek için [az keyvault anahtar alma](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) komutunu kullanın.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Yükleme başarılı olursa, Azure CLI içe aktarılan anahtarın özelliklerini görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Artık bu HSM korumalı anahtarı anahtar kasanızda kullanabilirsiniz. Daha fazla bilgi için [bu fiyat ve özellik karşılaştırması](https://azure.microsoft.com/pricing/details/key-vault/)bakın.



