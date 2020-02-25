---
title: Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma-Azure Key Vault | Microsoft Docs
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlayıp, oluşturmanıza ve aktarmaya yardımcı olması için bu makaleyi kullanın. Kendi anahtarını getir (BYOK) olarak da bilinir.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 9b8f1065660ea8331853f8804e709134fe682ba7
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566123"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>HSM korumalı anahtarları Key Vault’a içeri aktarma (önizleme)

> [!NOTE]
> Bu özellik önizleme aşamasındadır ve yalnızca *Doğu ABD 2 euap* ve *Orta ABD euap*Azure bölgelerinde kullanılabilir. 

Azure Key Vault kullandığınızda ek güvence için bir donanım güvenlik modülünde (HSM) bir anahtar alabilir veya oluşturabilirsiniz; anahtar hiçbir şekilde HSM sınırını terk etmeyecektir. Bu senaryo genellikle *kendi anahtarını getir* (bYok) olarak adlandırılır. Key Vault anahtarlarınızı korumak için HSM 'lerin nCipher nShield ailesini (FIPS 140-2 düzey 2 doğrulanan) kullanır.

Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlayıp, oluşturmanıza ve aktarmaya yardımcı olması için bu makaledeki bilgileri kullanın.

> [!NOTE]
> Bu işlev, Azure Çin 21Vianet için kullanılamaz. 
> 
> Bu içeri aktarma yöntemi yalnızca [desteklenen HSM](#supported-hsms)'ler için kullanılabilir. 

Daha fazla bilgi ve Key Vault kullanmaya başlama öğreticisi için (HSM korumalı anahtarlar için bir Anahtar Kasası oluşturma dahil), bkz. [nedir? Azure Key Vault nedir?](key-vault-overview.md).

## <a name="overview"></a>Genel Bakış

İşleme genel bakış. Belirli adımları makalenin ilerleyen kısımlarında açıklanmıştır.

* Key Vault ' de bir anahtar oluşturun ( *anahtar değişim anahtarı* (kek) olarak adlandırılır). KEK yalnızca `import` anahtar işlemi olan bir RSA-HSM anahtarı olmalıdır. Yalnızca Premium SKU 'SU Key Vault RSA-HSM anahtarlarını destekler.
* KEK ortak anahtarını bir. pek dosyası olarak indirin.
* KEK ortak anahtarını, şirket içi HSM 'ye bağlı bir çevrimdışı bilgisayara aktarın.
* Çevrimdışı bilgisayarda, bir BYOK dosyası oluşturmak için HSM satıcınız tarafından sunulan BYOK aracını kullanın. 
* Hedef anahtar, Key Vault HSM 'ye aktarılana kadar şifreli olarak kalacak bir KEK ile şifrelenir. Yalnızca anahtarınızın şifrelenmiş sürümü şirket içi HSM 'yi bırakır.
* Key Vault HSM içinde oluşturulan bir KEK dışarı aktarılabilir değildir. HSM 'ler, Key Vault HSM dışında KEK 'in hiçbir net sürümünün mevcut olmadığını, kuralı uygular.
* KEK, hedef anahtarın içeri aktarılacağı aynı anahtar kasasında olmalıdır.
* BYOK dosyası Key Vault karşıya yüklendiğinde, Key Vault HSM, hedef anahtar malzemelerinin şifresini çözmek ve bir HSM anahtarı olarak içeri aktarmak için KEK özel anahtarını kullanır. Bu işlem tamamen bir Key Vault HSM içinde gerçekleşir. Hedef anahtar her zaman HSM koruma sınırında kalır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki tabloda Azure Key Vault BYOK kullanımına yönelik önkoşullar listelenmektedir:

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Bir Azure aboneliği |Azure Key Vault bir Anahtar Kasası oluşturmak için bir Azure aboneliğine ihtiyacınız vardır. [Ücretsiz deneme Için kaydolun](https://azure.microsoft.com/pricing/free-trial/). |
| HSM korumalı anahtarları içeri aktarmak için Key Vault Premium SKU 'SU |Azure Key Vault hizmet katmanları ve özellikleri hakkında daha fazla bilgi için bkz. [Key Vault fiyatlandırması](https://azure.microsoft.com/pricing/details/key-vault/). |
| Desteklenen HSMs listesinden bir HSM ve HSM satıcınız tarafından sunulan bir BYOK aracı ve yönergeleri | HSM 'nizin nasıl kullanılacağına ilişkin bir HSM ve temel bilgi için izinleriniz olmalıdır. Bkz. [desteklenen HSM](#supported-hsms)'ler. |
| Azure CLı sürüm 2.1.0 veya üzeri | Bkz. [Azure CLI 'Yi yüklemeyi](/cli/azure/install-azure-cli?view=azure-cli-latest).|

## <a name="supported-hsms"></a>Desteklenen HSM 'ler

|HSM satıcı adı|Desteklenen HSM modelleri|Daha fazla bilgi|
|---|---|---|
|Thales|Yazılım üretici sürümü 7,3 veya üzeri ile SafeNet Luna HSM 7 ailesi| [SafeNet Luna BYOK aracı ve belgeleri](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|

> [!NOTE]
> HSM korumalı anahtarları MSMS 'nin nCipher nShield ailesinden içeri aktarmak için, [eskı BYOK yordamını](hsm-protected-keys-legacy.md)kullanın.

## <a name="supported-key-types"></a>Desteklenen anahtar türleri

|Anahtar adı|Anahtar türü|Anahtar boyutu|Kaynak|Açıklama|
|---|---|---|---|---|
|Anahtar değişim anahtarı (KEK)|RSA| 2\.048 bit<br />3\.072 bit<br />4\.096 bit|Azure Key Vault HSM|Azure Key Vault içinde oluşturulan HSM ile desteklenen bir RSA anahtar çifti|
|Hedef anahtar|RSA|2\.048 bit<br />3\.072 bit<br />4\.096 bit|Satıcı HSM|Azure Key Vault HSM 'ye aktarılacak anahtar|

## <a name="generate-and-transfer-your-key-to-the-key-vault-hsm"></a>Anahtarınızı oluşturun ve Key Vault HSM 'ye aktarın

Anahtarınızı oluşturmak ve Key Vault HSM 'ye aktarmak için:

* [1. Adım: KEK oluşturma](#step-1-generate-a-kek)
* [2. Adım: KEK ortak anahtarını Indirme](#step-2-download-the-kek-public-key)
* [3. Adım: anahtarınızı aktarım için oluşturma ve hazırlama](#step-3-generate-and-prepare-your-key-for-transfer)
* [4. Adım: anahtarınızı Azure Key Vault aktarma](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>1\. Adım: KEK oluşturma

Bir KEK, Key Vault HSM 'de oluşturulan bir RSA anahtarıdır. KEK, içe aktarmak istediğiniz anahtarı ( *hedef* anahtar) şifrelemek için kullanılır.

KEK şu olmalıdır:
- Bir RSA-HSM anahtarı (2.048-bit; 3.072-bit; veya 4.096-bit)
- hedef anahtarı içeri aktarmayı planladığınız aynı anahtar kasasında oluşturuldu
- `import` olarak ayarlanan izin verilen anahtar işlemleri ile oluşturuldu

`import`olarak ayarlanmış anahtar işlemleri olan bir KEK oluşturmak için [az keykasa Key Create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) komutunu kullanın. Aşağıdaki komuttan döndürülen anahtar tanımlayıcısını (`kid`) kaydedin. ( [Adım 3](#step-3-generate-and-prepare-your-key-for-transfer)' te `kid` değerini kullanacaksınız.)

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-the-kek-public-key"></a>2\. Adım: KEK ortak anahtarını Indirme

KEK ortak anahtarını bir. ped dosyasına indirmek için [az keykasakey Download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) komutunu kullanın. İçeri aktardığınız hedef anahtar, KEK ortak anahtarı kullanılarak şifrelenir.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

KEKforBYOK. PublicKey. ped dosyasını çevrimdışı bilgisayarınıza aktarın. Sonraki adımda bu dosyaya ihtiyacınız olacaktır.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>3\. Adım: anahtarınızı aktarım için oluşturma ve hazırlama

BYOK aracını indirip yüklemek için HSM satıcınızın belgelerine bakın. Bir hedef anahtar oluşturmak için HSM satıcınızdan gelen yönergeleri izleyin ve ardından bir anahtar aktarım paketi (BYOK dosyası) oluşturun. BYOK Aracı, bir BYOK dosyasında şifreli hedef anahtar oluşturmak için adım [1](#step-1-generate-a-kek) ' deki `kid` ve [Adım 2](#step-2-download-the-kek-public-key) ' de Indirdiğiniz KEKforBYOK. PublicKey. ped dosyasını kullanır.

BYOK dosyasını bağlı bilgisayarınıza aktarın.

> [!NOTE] 
> RSA 1.024 bit anahtarlarının içe aktarılması desteklenmez. Şu anda, eliptik eğri (EC) anahtarının içe aktarılması desteklenmez.
> 
> **Bilinen sorun**: bir RSA 4k hedef anahtarını SafeNet Luna hsms 'den içeri aktarma işlemi başarısız oluyor. Sorun çözüldüğünde, bu makale güncelleştirilir.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>4\. Adım: anahtarınızı Azure Key Vault aktarma

Anahtar içeri aktarmayı gerçekleştirmek için, anahtar aktarım paketini (bir BYOK dosyası) bağlantısı kesilen bilgisayarınızdan İnternet 'e bağlı bilgisayara aktarın. BYOK dosyasını Key Vault HSM 'ye yüklemek için [az keykasa Key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) komutunu kullanın.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Karşıya yükleme başarılı olursa, Azure CLı içeri aktarılan anahtarın özelliklerini görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Artık Anahtar Kasanızda HSM korumalı bu anahtarı kullanabilirsiniz. Daha fazla bilgi için [Bu fiyat ve Özellik Karşılaştırması](https://azure.microsoft.com/pricing/details/key-vault/)bölümüne bakın.



