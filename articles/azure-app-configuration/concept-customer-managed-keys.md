---
title: Yapılandırma verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları kullanma
description: Müşteri tarafından yönetilen anahtarları kullanarak yapılandırma verilerinizi şifreleme
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/18/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 5749b2fc58c4e1c5c75142f85a5132946714e25b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472641"
---
# <a name="use-customer-managed-keys-to-encrypt-your-app-configuration-data"></a>Uygulama Yapılandırma verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları kullanma
Azure Uygulama [Yapılandırması, hassas bilgileri istirahatte şifreler.](../security/fundamentals/encryption-atrest.md) Müşteri tarafından yönetilen anahtarların kullanımı, şifreleme anahtarlarınızı yönetmenize izin vererek gelişmiş veri koruması sağlar.  Yönetilen anahtar şifrelemesi kullanıldığında, App Configuration'daki tüm hassas bilgiler kullanıcı tarafından sağlanan Azure Key Vault anahtarıyla şifrelenir.  Bu, şifreleme anahtarını isteğe bağlı olarak döndürme olanağı sağlar.  Ayrıca, Uygulama Yapılandırması örneğinin anahtara erişimini iptal ederek Azure Uygulama Yapılandırması'nın hassas bilgilere erişimini iptal etme olanağı da sağlar.

## <a name="overview"></a>Genel Bakış 
Azure Uygulama Yapılandırması, Microsoft tarafından sağlanan 256 bit AES şifreleme anahtarını kullanarak hassas bilgileri istirahatte şifreler. Her Uygulama Yapılandırma örneğinin, hizmet tarafından yönetilen ve hassas bilgileri şifrelemek için kullanılan kendi şifreleme anahtarı vardır. Hassas bilgiler, anahtar değer çiftlerinde bulunan değerleri içerir.  Müşteri tarafından yönetilen anahtar özelliği etkinleştirildiğinde, App Configuration Azure Etkin Dizini ile kimlik doğrulaması yapmak için Uygulama Yapılandırması örneğine atanmış yönetilen bir kimlik kullanır. Yönetilen kimlik daha sonra Azure Key Vault'u arar ve Uygulama Yapılandırma örneği şifreleme anahtarını sarar. Sarılmış şifreleme anahtarı daha sonra depolanır ve paketlenmemiş şifreleme anahtarı App Configuration içinde bir saat önbelleğe çıkar. Uygulama Yapılandırması, App Configuration örneğinin şifreleme anahtarının paketlenmemiş sürümünü saatlik olarak yeniler. Bu, normal çalışma koşullarında kullanılabilirliği sağlar. 

>[!IMPORTANT]
> Uygulama Yapılandırması örneğine atanan kimlik artık örneğin şifreleme anahtarını açma yetkisine sahip değilse veya yönetilen anahtar kalıcı olarak silinirse, Uygulamada depolanan hassas bilgilerin şifresini çözmek artık mümkün olmayacaktır Yapılandırma örneği. Azure Key Vault'un [yumuşak silme](../key-vault/key-vault-ovw-soft-delete.md) işlevini kullanmak, şifreleme anahtarınızı yanlışlıkla silme şansını azaltır.

Kullanıcılar Azure Uygulama Yapılandırma örneğinde müşterinin yönetilen anahtar özelliğini etkinleştirdiklerinde, hizmetin hassas bilgilerine erişme yeteneğini denetler. Yönetilen anahtar bir kök şifreleme anahtarı olarak hizmet vermektedir. Bir kullanıcı, anahtar kasa erişim ilkesini değiştirerek Uygulama Yapılandırma örneğinin yönetilen anahtarına erişimini iptal edebilir. Bu erişim iptal edildiğinde, App Configuration bir saat içinde kullanıcı verilerinin şifresini çözme yeteneğini kaybeder. Bu noktada, Uygulama Yapılandırması örneği tüm erişim denemelerini yasaklar. Bu durum, yönetilen anahtara bir kez daha hizmet erişimi vererek kurtarılabilir.  Bir saat içinde, App Configuration kullanıcı verilerinin şifresini çözebilecek ve normal koşullarda çalışabilecek.

>[!NOTE]
>Tüm Azure Uygulama Yapılandırma verileri yalıtılmış bir yedeklemede 24 saate kadar saklanır. Bu, paketlenmemiş şifreleme anahtarını içerir. Bu veriler servis veya servis ekibi için hemen kullanılamaz. Acil bir geri yükleme durumunda, Azure Uygulama Yapılandırması yönetilen anahtar verilerden kendisini yeniden iptal eder.

## <a name="requirements"></a>Gereksinimler
Azure Uygulama Yapılandırması için müşteri tarafından yönetilen anahtar özelliğini başarıyla etkinleştirmek için aşağıdaki bileşenler gereklidir:
- Standart katman Azure Uygulama Yapılandırma örneği
- Yumuşak silme ve temizleme koruma özellikleri etkinleştirilmiş Azure Anahtar Kasası
- Anahtar Kasası içinde bir RSA veya RSA-HSM anahtarı
    - Anahtarın süresi dolmamalı, etkinleştirilmiş olmalı ve hem kaydırma hem de açma özellikleri etkin olmalıdır

Bu kaynaklar yapılandırıldıktan sonra, Azure Uygulama Yapılandırması'nın Anahtar Kasa sı anahtarını kullanmasına izin vermek için iki adım kalır:
1. Azure Uygulama Yapılandırması örneğine yönetilen bir kimlik atama
2. Hedef Key `GET` `WRAP`Vault'un erişim ilkesinde kimliği ve `UNWRAP` izinleri ver.

## <a name="enable-customer-managed-key-encryption-for-your-azure-app-configuration-instance"></a>Azure Uygulama Yapılandırma örneğiniz için müşteri tarafından yönetilen anahtar şifrelemesini etkinleştirme
Başlamak için, düzgün yapılandırılmış bir Azure Uygulama Yapılandırma örneğine ihtiyacınız olacaktır. Henüz kullanılabilir bir Uygulama Yapılandırma örneği yoksa, aşağıdaki hızlı başlangıçlardan birini ayarlamayı izleyin:
- [Azure Uygulama Yapılandırması ile ASP.NET Core uygulaması oluşturun](quickstart-aspnet-core-app.md)
- [Azure Uygulama Yapılandırması ile bir .NET Core uygulaması oluşturun](quickstart-dotnet-core-app.md)
- [Azure Uygulama Yapılandırması ile bir .NET Framework uygulaması oluşturun](quickstart-dotnet-app.md)
- [Azure Uygulama Yapılandırması ile Java Spring uygulaması oluşturma](quickstart-java-spring-app.md)

>[!TIP]
> Azure Bulut Kabuğu, bu makaledeki komut satırı yönergelerini çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  .NET Core SDK de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com) shell.azure.com başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Bulut BulutU Hakkında Daha Fazla Bilgi Edinebilirsiniz

### <a name="create-and-configure-an-azure-key-vault"></a>Azure Anahtar Kasası oluşturma ve yapılandırma
1. Azure CLI'yi kullanarak bir Azure Anahtar Kasası oluşturun.  Her ikisinin de `vault-name` kullanıcı tarafından sağlandığını ve `resource-group-name` benzersiz olması gerektiğini unutmayın.  Biz `contoso-vault` ve `contoso-resource-group` bu örneklerde kullanın.

    ```azurecli
    az keyvault create --name contoso-vault --resource-group contoso-resource-group
    ```
    
1. Anahtar Kasası için yumuşak silme ve temizleme korumasını etkinleştirin. 1. adımda oluşturulan`contoso-vault`Anahtar Kasa (`contoso-resource-group`) ve Kaynak Grubu () adlarını değiştirin.

    ```azurecli
    az keyvault update --name contoso-vault --resource-group contoso-resource-group --enable-purge-protection --enable-soft-delete
    ```
    
1. Bir Anahtar Kasası anahtarı oluşturun. Bu anahtar `key-name` için benzersiz bir anahtar sağlayın ve`contoso-vault`adım 1'de oluşturulan Anahtar Kasası () adlarını değiştirin. Tercih edip `RSA` etmediğiniz veya `RSA-HSM` şifreleme yi belirtin.

    ```azurecli
    az keyvault key create --name key-name --kty {RSA or RSA-HSM} --vault-name contoso-vault
    ```
    
    Bu komutun çıktısı, oluşturulan anahtar için anahtar kimliğini ("çocuk") gösterir.  Bu alıştırmada daha sonra kullanmak üzere anahtar kimliğini not edin.  Anahtar kimliği nde form `https://{my key vault}.vault.azure.net/keys/{key-name}/{Key version}`vardır: .  Anahtar kimliğinin üç önemli bileşeni vardır:
    1. Anahtar Vault URI: 'https://{benim anahtar vault}.vault.azure.net
    1. Anahtar Vault anahtar adı: {Anahtar Adı}
    1. Key Vault anahtar sürümü: {Key version}

1. Önceki adımlarda kullanılan Uygulama Yapılandırma örneğinizin ve kaynak grubunuzun adını nida eden Azure CLI'yi kullanarak yönetilen kimlik atanmış bir sistem oluşturun. Yönetilen kimlik, yönetilen anahtara erişmek için kullanılır. Bir `contoso-app-config` Uygulama Yapılandırmaörneğinin adını göstermek için kullanırız:
    
    ```azurecli
    az appconfig identity assign --na1. me contoso-app-config --group contoso-resource-group --identities [system]
    ```
    
    Bu komutun çıktısı, atanan sistemin temel kimliğini ("principalId") ve kiracı kimliğini ("tenandId") içerir.  Bu, yönetilen anahtara kimlik erişimi sağlamak için kullanılır.

    ```json
    {
    "principalId": {Principal Id},
    "tenantId": {Tenant Id},
    "type": "SystemAssigned",
    "userAssignedIdentities": null
    }
    ```

1. Azure Uygulama Yapılandırma örneğinin yönetilen kimliği, anahtar doğrulama, şifreleme ve şifre çözme gerçekleştirmek için anahtara erişilmesi gerekir. Erişmesi gereken belirli eylem kümesi şunları `GET` `WRAP`içerir: `UNWRAP` , , ve anahtarlar için.  Erişim inanılması, Uygulama Yapılandırması örneğinin yönetilen kimliğinin temel kimliğini gerektirir. Bu değer önceki adımda elde edildi. Aşağıda " olarak `contoso-principalId`gösterilmiştir. Komut satırını kullanarak yönetilen anahtara izin verme:

    ```azurecli
    az keyvault set-policy -n contoso-vault --object-id contoso-principalId --key-permissions get wrapKey unwrapKey
    ```

1. Azure Uygulama Yapılandırmaörneği yönetilen anahtara erişebildiği zaman, Azure CLI'yi kullanarak hizmette müşteri tarafından yönetilen anahtar özelliğini etkinleştirebiliriz. Anahtar oluşturma adımları sırasında kaydedilen aşağıdaki `key name` `key vault URI`özellikleri geri çağırın: .

    ```azurecli
    az appconfig update -g contoso-resource-group -n contoso-app-config --encryption-key-name key-name --encryption-key-version key-version --encryption-key-vault key-vault-Uri
    ```

Azure Uygulama Yapılandırma örneğiniz artık Azure Anahtar Kasası'nda depolanan müşteri tarafından yönetilen bir anahtarı kullanacak şekilde yapılandırılmıştır.

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, Azure Uygulama Yapılandırma örneğini şifreleme için müşteri tarafından yönetilen bir anahtar kullanacak şekilde yapılandırıldınız.  [Hizmetinizi Azure Yönetilen Kimlikler ile nasıl entegre edacağınızı](howto-integrate-azure-managed-service-identity.md)öğrenin.