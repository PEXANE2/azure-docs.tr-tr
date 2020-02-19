---
title: Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma-Azure Key Vault | Microsoft Docs
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlayıp, oluşturmanıza ve aktarmaya yardımcı olması için bu makaleyi kullanın. BYOK olarak da bilinir veya kendi anahtarınızı getir.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 2f5269587d222be9a1628b72c1f3f0dc1b105f3c
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461752"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>HSM korumalı anahtarları Key Vault içeri aktar (Önizleme)

> [!NOTE]
> Bu özellik önizleme aşamasındadır ve yalnızca **Doğu ABD 2 euap** ve **Orta ABD euap** bölgelerinde kullanılabilir. 

Azure Key Vault kullanırken ek güvence için, HSM sınırını hiçbir zaman bırakmamış donanım güvenlik modüllerinde (HSM 'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. Bu senaryo, genellikle *kendi anahtarını getir*veya bYok olarak adlandırılır. Azure Key Vault anahtarlarınızı korumak için HSM 'lerin nCipher nShield ailesini (FIPS 140-2 düzey 2 doğrulanan) kullanır.

Bu konudaki bilgileri kullanarak, Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlayıp, oluşturmanıza ve daha sonra aktarmanızı sağlamanıza yardımcı olması için bu konudaki bilgileri kullanın.

> [!NOTE]
> Bu işlev, Azure Çin 21Vianet için kullanılamaz. 
> 
> Bu içeri aktarma yöntemi yalnızca [desteklenen HSM](#supported-hsms)'ler için kullanılabilir. 

Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](key-vault-overview.md)  HSM korumalı anahtarlar için Anahtar Kasası oluşturmayı içeren bir başlangıç öğreticisi için bkz. [Azure Key Vault nedir?](key-vault-overview.md).

## <a name="overview"></a>Genel Bakış

* Anahtar kasasında anahtar (anahtar değişim anahtarı veya KEK olarak adlandırılır) oluşturun. Bu, tek anahtar işlemi olarak ' Import ' içeren bir RSA-HSM anahtarı olmalıdır. Yalnızca Anahtar Kasası Premium SKU 'SU RSA-HSM anahtarlarını destekler.
* KEK ortak anahtarını bir. pek dosyası olarak indirin
* Şirket içi HSM 'ye bağlı çevrimdışı iş istasyonunuza KEK ortak anahtarını aktarın.
* Çevrimdışı iş istasyonunuzdan bir BYOK dosyası oluşturmak için HSM satıcınız tarafından sunulan BYOK aracını kullanın. 
* Hedef anahtar, Azure Key Vault HSM 'lerine aktarılana kadar şifrelenmiş olarak kalacak bir KEK ile şifrelenir. Yalnızca anahtarınızın şifrelenmiş sürümü şirket içi HSM 'yi bırakır.
* Azure Key Vault HSM 'ler içinde oluşturulan ve dışarı aktarılabilir olmayan KEK. HSM 'ler, KEK 'in Key Vault HSM dışından net bir sürümü olmaması için zorlar.
* KEK, hedef anahtarın içeri aktarılacağı aynı anahtar kasasında olmalıdır.
* BYOK dosyası Key Vault karşıya yüklendiğinde, Key Vault HSM 'ler, hedef anahtar malzemelerinin şifresini çözmek ve bir HSM anahtarı olarak içeri aktarmak için KEK özel anahtarını kullanır. Bu işlem tamamen Key Vault HSMs içinde gerçekleşir ve hedef anahtar her zaman HSM koruma sınırında kalır.

## <a name="prerequisites"></a>Önkoşullar

Azure Key Vault için kendi anahtarınızı getir (BYOK) önkoşulları listesi için aşağıdaki tabloya bakın.

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Azure aboneliği |Azure Key Vault oluşturmak için bir Azure aboneliğine ihtiyacınız vardır: [ücretsiz deneme Için kaydolun](https://azure.microsoft.com/pricing/free-trial/) |
| HSM korumalı anahtarları içeri aktarmaya yönelik bir Anahtar Kasası (Premium SKU) |Azure Key Vault yönelik hizmet katmanları ve özellikleri hakkında daha fazla bilgi için [Azure Key Vault fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) Web sitesine bakın. |
| HSM satıcınız tarafından sunulan BYOK aracı ve yönergeleriyle birlikte desteklenen HSM 'ler listesinden bir HSM | Bir donanım güvenlik modülüne erişiminizin olması ve HSM 'nizin temel operasyonel bilgisine sahip olmanız gerekir. Bkz. [desteklenen HSM](#supported-hsms)'ler. |
| Azure CLı sürüm 2.1.0 veya üzeri | Daha fazla bilgi için lütfen bkz. [Azure CLI 'Yı yüklemeyin](/cli/azure/install-azure-cli?view=azure-cli-latest) .|

## <a name="supported-hsms"></a>Desteklenen HSM 'ler

|HSM satıcı adı|Desteklenen HSM modelleri|Ek ayrıntılar|
|---|---|---|
|Thales|Üretici yazılımı 7,3 veya daha yeni bir sürümü ile SafeNet Luna HSM 7 ailesi| [SafeNet Luna BYOK aracı ve belgeleri](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|


> [!NOTE]
> HSM korumalı anahtarları MSMS 'nin nCipher nShield ailesinden içeri aktarmak için [eskı BYOK yordamını kullanın](hsm-protected-keys-legacy.md)


## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Anahtarınızı Azure Key Vault HSM 'ye oluşturun ve aktarın

Anahtarınızı oluşturmak ve bir Azure Key Vault HSM 'ye aktarmak için aşağıdaki adımları kullanacaksınız:

* [1. Adım: KEK oluşturma](#step-1-generate-a-kek)
* [2. Adım: KEK ortak anahtarını Indirme](#step-2-download-kek-public-key)
* [3. Adım: anahtarınızı aktarım için oluşturma ve hazırlama](#step-3-generate-and-prepare-your-key-for-transfer)
* [4. Adım: anahtarınızı Azure Key Vault aktarma](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>1\. Adım: KEK oluşturma

KEK (anahtar değişim anahtarı) Key Vault HSM 'de oluşturulan bir RSA anahtarıdır. Bu anahtar, içeri aktarılacak anahtarı şifrelemek için kullanılır (hedef anahtar).

KEK şu olmalıdır:
1. bir **RSA-HSM** anahtarı (2048-bit veya 3072-bit veya 4096-bit)
2. hedef anahtarı içeri aktarmayı planladığınız aynı anahtar kasasında oluşturuldu
3. **içeri aktarmaya** ayarlanmış izin verilen anahtar işlemleri ile oluşturuldu

Anahtar işlemleri içeri aktarmaya ayarlanmış KEK oluşturmak için [az keykasa Key Create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) komutunu kullanın. Aşağıdaki komuttan döndürülen ' KID ' anahtar tanımlayıcısını aklınızda edin. [Adım 3](#step-3-generate-and-prepare-your-key-for-transfer)' te buna ihtiyacınız olacak.


```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-kek-public-key"></a>2\. Adım: KEK ortak anahtarını Indirme

KEK ortak anahtarını bir. ped dosyasına indirmek için [az keykasakey Download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) komutunu kullanın. İçeri aktardığınız hedef anahtar, KEK ortak anahtarı kullanılarak şifrelenir.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

KEKforBYOK. PublicKey. ped dosyasını çevrimdışı iş istasyonunuza aktarın. Sonraki adımda bu dosyaya ihtiyacınız olacak.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>3\. Adım: anahtarınızı aktarım için oluşturma ve hazırlama

BYOK aracını indirmek ve yüklemek için lütfen HSM satıcınızın belgelerine bakın. Bir hedef anahtar oluşturmak ve ardından bir anahtar aktarım paketi (BYOK dosyası) oluşturmak için HSM satıcınızdan yönergeyi izleyin. BYOK Aracı, bir BYOK dosyasında şifreli hedef anahtar oluşturmak için [Adım 2](#step-2-download-kek-public-key) ' de indirdiğiniz [Adım 1](#step-1-generate-a-kek) ve KEKforBYOK. PublicKey. ped dosyasındaki anahtar tanımlayıcısını kullanır.

BYOK dosyasını bağlı iş istasyonunuza aktarın.

> [!NOTE] 
> Hedef anahtar, 2048-bit veya 3072-bit ya da 4096-bit boyutundaki bir RSA anahtarı olmalıdır. Eliptik Eğri anahtarlarının içeri aktarılması Şu anda desteklenmiyor.
> <br/><strong>Bilinen sorun:</strong> SafeNet Luna HSM 'lerden RSA 4K hedef anahtarını içeri aktarma işlemi başarısız oluyor. Sorun çözüldüğünde bu belge güncelleştirilir.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>4\. Adım: anahtarınızı Azure Key Vault aktarma

Bu son adımda, anahtar alma iş istasyonunuzdan anahtar aktarım paketini (bir BYOK dosyası) Internet 'e bağlı iş istasyonuna aktarın ve ardından, anahtar içeri aktarma işleminin tamamlanabilmesi için Azure Key Vault HSM 'yi karşıya yüklemek üzere [az keykasa anahtarı al](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) komutunu kullanın.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Karşıya yükleme başarılı olursa, yeni içeri aktardığınız anahtarın özelliklerinin görüntülendiğini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Artık Anahtar Kasanızda HSM korumalı bu anahtarı kullanabilirsiniz. Daha fazla bilgi için bu fiyat ve özellik [karşılaştırması](https://azure.microsoft.com/pricing/details/key-vault/)bölümüne bakın.
