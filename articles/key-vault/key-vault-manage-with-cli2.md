---
title: CLI kullanarak Azure Anahtar Kasası'nı yönetin - Azure Anahtar Kasası | Microsoft Dokümanlar
description: Azure CLI'yi kullanarak Key Vault'taki yaygın görevleri otomatikleştirmek için bu makaleyi kullanın
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: f69b65ece09a63c7a1d9e23d5cd488d9659724ad
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79457432"
---
# <a name="manage-key-vault-using-the-azure-cli"></a>Azure CLI'yi kullanarak Anahtar Kasasını Yönetme 

Bu makalede, Azure CLI'yi kullanarak Azure Anahtar Kasası ile çalışmaya nasıl başlanıncayasınız.  Şu bilgileri görebilirsiniz:

- Azure'da sertleştirilmiş bir kapsayıcı (kasa) nasıl oluşturulur?
- Anahtar kasasına anahtar, gizli veya sertifika ekleme
- Azure Active Directory ile uygulama kaydetme
- Bir uygulamayı anahtar veya gizli kullanma yetkisi verme
- Anahtar kasası gelişmiş erişim ilkelerini ayarlama
- Donanım güvenlik modülleri (HSM) ile çalışma
- Anahtar kasasını ve ilgili anahtarları ve sırları silme
- Çeşitli Azure Çapraz Platform Komut Satırı Arayüz Komutları


Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

> [!NOTE]
> Bu makalede, bir uygulamanın anahtar kasasında anahtar veya gizli kullanması için nasıl yetki verilebildiğini gösteren adımlardan birinin içerdiği Azure uygulamasının nasıl yazılalalagerektiğine ilişkin yönergeler yer almaz.
>

Azure Anahtar Kasası'na genel bakış için [bkz.](key-vault-overview.md)
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede Azure CLI komutlarını kullanmak için aşağıdaki öğelere sahip olmalısınız:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Azure Komut Satırı Arabirimi sürüm 2.0 veya sonrası. En son sürümü yüklemek için Azure [CLI'yi yükleyin' e](/cli/azure/install-azure-cli)bakın.
* Bu makalede oluşturduğunuz anahtarı veya parolayı kullanacak şekilde yapılandırılacak bir uygulama. [Microsoft Yükleme Merkezi](https://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Talimatlar için, verilen Readme dosyasına bakın.

### <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure Platform Ötesi Komut Satırı Arabirimi ile ilgili yardım alma

Bu makalede, komut satırı arabirimini (Bash, Terminal, Komut istemi) bildiğiniz varsayar.

--yardım veya -h parametresi, belirli komutlar için yardım görüntülemek için kullanılabilir. Alternatif olarak, Azure yardım [komut] [seçenekleri] biçimi de kullanılabilir. Bir komutun gerektirdiği parametrelerden şüphe duyduğunuzda, yardıma başvurun. Örneğin, aşağıdaki komutların tümü aynı bilgileri döndürer:

```azurecli
az account set --help
az account set -h
```

Azure PlatformLar Arası Komut Satırı Arabirimi'nde Azure Kaynak Yöneticisi'ni tanımak için aşağıdaki makaleleri de okuyabilirsiniz:

* [Azure CLI'yi yükle](/cli/azure/install-azure-cli)
* [Azure CLI'yi kullanmaya başlama](/cli/azure/get-started-with-azure-cli)

## <a name="how-to-create-a-hardened-container-a-vault-in-azure"></a>Azure'da sertleştirilmiş bir kapsayıcı (kasa) nasıl oluşturulur?

Kasalar donanım güvenlik modülleri tarafından desteklenen güvenli kaplar. Kasalar, uygulama gizli dizilerinin depolanmasını merkezi hale getirerek güvenlik bilgilerini kazayla kaybetme olasılığını azaltmaya yardımcı olur. Anahtar Kasaları ayrıca içlerinde depolanmış her şeye erişimi denetler ve günlüğe kaydeder. Azure Key Vault, sağlam bir sertifika yaşam döngüsü yönetim çözümü için gereken özellikleri sağlayarak Aktarım Katmanı Güvenliği (TLS) sertifikalarını isteme ve yenileme işlemlerini gerçekleştirebilir. Sonraki adımlarda, bir kasa oluşturacaksınız.

### <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma

Etkileşimli olarak oturum açabilmek için aşağıdaki komutu kullanın:

```azurecli
az login
```
Kuruluş hesabı kullanarak oturum açabilmek için kullanıcı adınızı ve şifrenizi geçebilirsiniz.

```azurecli
az login -u username@domain.com -p password
```

Birden fazla aboneliğiniz varsa ve hangisini kullanacağınızı belirtmeniz gerekiyorsa, hesabınızın aboneliklerini görmek için aşağıdakileri yazın:

```azurecli
az account list
```

Abonelik parametresi içeren bir abonelik belirtin.

```azurecli
az account set --subscription <subscription name or ID>
```

Azure Çapraz Platform Komut Satırı Arabirimini yapılandırma hakkında daha fazla bilgi için Azure [CLI yükle'ye](/cli/azure/install-azure-cli)bakın.

### <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma

Azure Kaynak Yöneticisi kullanırken, ilgili tüm kaynaklar bir kaynak grubu içinde oluşturulur. Varolan bir kaynak grubunda anahtar kasası oluşturabilirsiniz. Yeni bir kaynak grubu kullanmak istiyorsanız, yeni bir kaynak grubu oluşturabilirsiniz.

```azurecli
az group create -n "ContosoResourceGroup" -l "East Asia"
```

İlk parametre kaynak grubu adı, ikinci parametre ise konumdur. Tüm olası konumların listesini almak için:

```azurecli
az account list-locations
``` 

### <a name="register-the-key-vault-resource-provider"></a>Key Vault kaynak sağlayıcısını kaydedin

 Yeni bir anahtar kasası oluşturmaya çalıştığınızda "Abonelik ad alanı 'Microsoft.KeyVault' kullanmak için kayıtlı değildir" hatasını görebilirsiniz. Bu ileti görünüyorsa, Key Vault kaynak sağlayıcısının aboneliğinizde kayıtlı olduğundan emin olun. Bu, her bir abonelik için tek seferlik bir işlemdir.

```azurecli
az provider register -n Microsoft.KeyVault
```

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Anahtar `az keyvault create` kasası oluşturmak için komutu kullanın. Bu komut dosyasının üç zorunlu parametresi vardır: kaynak grubu adı, önemli bir kasa adı ve coğrafi konum.

**ContosoKeyVault**adlı yeni bir kasa oluşturmak için, kaynak grubu **ContosoResourceGroup**, **Doğu Asya** konumda ikamet, yazın: 

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "East Asia"
```

Bu komutun çıktısı oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **ad**: Örnekte, adı ContosoKeyVault olduğunu. Bu adı diğer Key Vault komutları için kullanırsınız.
* **vaultUri**: Örnekte, URI https://contosokeyvault.vault.azure.netolduğunu . REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Şu andan itibaren, başka kimsenin yetkim yok.

## <a name="adding-a-key-secret-or-certificate-to-the-key-vault"></a>Anahtar kasasına anahtar, gizli veya sertifika ekleme

Azure Key Vault'un sizin için yazılım korumalı bir `az key create` anahtar oluşturmasını istiyorsanız, komutu kullanın.

```azurecli
az keyvault key create --vault-name "ContosoKeyVault" --name "ContosoFirstKey" --protection software
```

.pem dosyasında varolan bir anahtarınız varsa, bu anahtarı Azure Key Vault'a yükleyebilirsiniz. Anahtarı yazılım veya HSM ile korumayı seçebilirsiniz. Bu örnekte ,.pem dosyasından anahtarı içeri aktarın ve "hVFkk965BuUv" parolasını kullanarak yazılımla korur:

```azurecli
az keyvault key import --vault-name "ContosoKeyVault" --name "ContosoFirstKey" --pem-file "./softkey.pem" --pem-password "hVFkk965BuUv" --protection software
```

Artık oluşturduğunuz veya yüklediğiniz anahtarı URI'sini kullanarak Azure Key Vault'a başvurulayabilirsiniz. Her **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** zaman geçerli sürümü almak için kullanın. Bu özel sürümü almak için https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] kullanın. Örneğin, **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

SQLPassword adlı bir parola olan ve Azure Anahtar Kasalarına "hVFkkk965BuUv" değeri ne sahip olan kasaya bir sır ekleyin. 

```azurecli
az keyvault secret set --vault-name "ContosoKeyVault" --name "SQLPassword" --value "hVFkk965BuUv "
```

Uri'sini kullanarak bu parolaya başvurun. Bu **https://ContosoVault.vault.azure.net/secrets/SQLPassword** özel sürümü almak için her zaman geçerli sürümü ve https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id] sürümünü almak için kullanın. Örneğin, **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Bir .pem veya .pfx kullanarak kasaya bir sertifika aktarın.

```azurecli
az keyvault certificate import --vault-name "ContosoKeyVault" --file "c:\cert\cert.pfx" --name "ContosoCert" --password "hVFkk965BuUv"
```

Oluşturduğunuz anahtarı, sırrı veya sertifikayı görüntüleyelim:

* Anahtarlarınızı görüntülemek için şunları yazın: 

```azurecli
az keyvault key list --vault-name "ContosoKeyVault"
```

* Sırlarınızı görüntülemek için şunları yazın: 

```azurecli
az keyvault secret list --vault-name "ContosoKeyVault"
```

* Sertifikaları görüntülemek için şunları yazın: 

```azurecli
az keyvault certificate list --vault-name "ContosoKeyVault"
```

## <a name="registering-an-application-with-azure-active-directory"></a>Azure Active Directory ile uygulama kaydetme

Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure Key Vault'a özgü değildir, ancak farkındalık için buraya dahildir. Uygulama kaydını, hesabınızı, kasayı ve uygulamanın aynı Azure dizininde olması gerekir.

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir.  Uygulamanın sahibinin önce Azure Etkin Dizini'ne kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- **Uygulama Kimliği** (AAD İstemci Kimliği veya appID olarak da bilinir)
- **Kimlik doğrulaması anahtarı** (paylaşılan gizli dizi olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Bir uygulamanın belirteç almak için nasıl yapılandırıldığı uygulamaya bağlıdır. Key [Vault örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343)için, uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Azure Active Directory ile bir uygulamayı kaydetmeyle ilgili ayrıntılı adımlar için, [uygulamalara Azure Active Directory ile Tümleştirme](../active-directory/develop/active-directory-integrating-applications.md), [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturmak için portalı kullanma ve](../active-directory/develop/howto-create-service-principal-portal.md)Azure [CLI ile bir Azure hizmet ilkesi oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli)başlıklı makaleleri gözden geçirmelisiniz.

Azure Etkin Dizini'nde bir uygulama kaydetmek için:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password "hVFkk965BuUv" --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorizing-an-application-to-use-a-key-or-secret"></a>Bir uygulamayı anahtar veya gizli kullanma yetkisi verme

Kasadaki anahtara veya gizliye erişme yetkisi vermek `az keyvault set-policy` için komutu kullanın.

Örneğin, kasa adınız ContosoKeyVault ise, uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed bir appID vardır ve şifresini çözmek ve kasanızdaki tuşları ile imzalamak için uygulama yetki istiyorum, aşağıdaki komutu kullanın:

```azurecli
az keyvault set-policy --name "ContosoKeyVault" --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Kasanızdaki sırları okumak için aynı uygulamayı yetkilendirmek için aşağıdaki komutu yazın:

```azurecli
az keyvault set-policy --name "ContosoKeyVault" --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="setting-key-vault-advanced-access-policies"></a><a name="bkmk_KVperCLI"></a>Anahtar kasası gelişmiş erişim ilkelerini ayarlama

Anahtar kasası için gelişmiş ilkeleri etkinleştirmek için [az keyvault güncelleştirmesini](/cli/azure/keyvault#az-keyvault-update) kullanın.

 Dağıtım için Anahtar Kasasını Etkinleştir: Sanal makinelerin kasadan sır olarak depolanan sertifikaları almasına olanak tanır.

 ```azurecli
 az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"
 ```

Disk şifreleme için Anahtar Kasasını etkinleştirin: Azure Disk şifrelemesi için kasayı kullanırken gereklidir.

 ```azurecli
 az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-disk-encryption "true"
 ```  

Şablon dağıtımı için Anahtar Kasasını etkinleştirin: Kaynak Yöneticisi'nin kasadan sırları almasına izin verir.

```azurecli
az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-template-deployment "true"
```

## <a name="working-with-hardware-security-modules-hsms"></a>Donanım güvenlik modülleri (HSM) ile çalışma

Ek güvence için, HSM sınırını hiç terk çıkmayan donanım güvenlik modüllerinden (HSM) anahtar alabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğine sahip olmanız gerekir.

Keyvault'u oluşturduğunuzda, 'sku' parametresini ekleyin:

```azurecli
az keyvault create --name "ContosoKeyVaultHSM" --resource-group "ContosoResourceGroup" --location "East Asia" --sku "Premium"
```

Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için Hedef parametresini 'HSM' olarak ayarlayın:

```azurecli
az keyvault key create --vault-name "ContosoKeyVaultHSM" --name "ContosoFirstHSMKey" --protection "hsm"
```

Bilgisayarınızdaki bir .pem dosyasından bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```azurecli
az keyvault key import --vault-name "ContosoKeyVaultHSM" --name "ContosoFirstHSMKey" --pem-file "/.softkey.pem" --protection "hsm" --pem-password "PaSSWORD"
```

Bir sonraki komut bir "kendi anahtarını getir" (BYOK) paketini içeri zedeler. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```azurecli
az keyvault key import --vault-name "ContosoKeyVaultHSM" --name "ContosoFirstHSMKey" --byok-file "./ITByok.byok" --protection "hsm"
```

Bu BYOK paketinin nasıl oluşturacağı yla ilgili daha ayrıntılı talimatlar için Azure [Anahtar Kasası ile HSM Korumalı Anahtarların nasıl kullanılacağına](key-vault-hsm-protected-keys.md)bakın.

## <a name="deleting-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilgili anahtarları ve sırları silme

Artık anahtar kasasına ve anahtarlarına veya sırlarına ihtiyacınız yoksa, `az keyvault delete` komutu kullanarak anahtar kasasını silebilirsiniz:

```azurecli
az keyvault delete --name "ContosoKeyVault"
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```azurecli
az group delete --name "ContosoResourceGroup"
```

## <a name="miscellaneous-azure-cross-platform-command-line-interface-commands"></a>Çeşitli Azure Çapraz Platform Komut Satırı Arayüz Komutları

Azure Anahtar Kasası'nı yönetmek için yararlı olabileceğiniz diğer komutlar.

Bu komut, tüm anahtarların ve seçili özelliklerin bir tabular görüntülenmesini listeler:

```azurecli
az keyvault key list --vault-name "ContosoKeyVault"
```

Bu komut, belirtilen anahtar için özelliklerin tam listesini görüntüler:

```azurecli
az keyvault key show --vault-name "ContosoKeyVault" --name "ContosoFirstKey"
```

Bu komut, tüm gizli adların ve seçili özelliklerin bir tabular görüntülenmesini listeler:

```azurecli
az keyvault secret list --vault-name "ContosoKeyVault"
```

Aşağıda, belirli bir anahtarın nasıl kaldırılabildiğinianlatan bir örnek verilmiştir:

```azurecli
az keyvault key delete --vault-name "ContosoKeyVault" --name "ContosoFirstKey"
```

Aşağıda, belirli bir sırrın nasıl kaldırılabildiğinianlatan bir örnek verilmiştir:

```azurecli
az keyvault secret delete --vault-name "ContosoKeyVault" --name "SQLPassword"
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasa komutları için tam Azure CLI başvurusu için [Key Vault CLI başvurusuna](/cli/azure/keyvault)bakın.

- Programlama başvuruları için [Azure Anahtar Kasası geliştirici kılavuzuna](key-vault-developers-guide.md) bakın

- Azure Anahtar Kasası ve HSM hakkında bilgi için Azure [Anahtar Kasası ile HSM Korumalı Anahtarların nasıl kullanılacağına](key-vault-hsm-protected-keys.md)bakın.
