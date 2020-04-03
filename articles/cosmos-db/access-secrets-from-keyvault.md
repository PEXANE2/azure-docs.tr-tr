---
title: Azure Cosmos DB tuşlarını depolamak ve erişmek için Key Vault'u kullanın
description: Azure Cosmos DB bağlantı dizesini, anahtarlarını, uç noktalarını depolamak ve erişmek için Azure Key Vault'u kullanın.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/23/2019
ms.reviewer: sngun
ms.openlocfilehash: 225221635f978e3d70cec4ce7e9d78d6b100b4fd
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618767"
---
# <a name="secure-azure-cosmos-keys-using-azure-key-vault"></a>Azure Key Vault kullanarak Azure Cosmos anahtarlarının güvenliğini sağlama 

>[!IMPORTANT]
> Azure Cosmos DB tuşlarına erişmek için önerilen [çözüm, sisteme atanmış yönetilen](managed-identity-based-authentication.md)bir kimlik kullanmaktır. Hizmetiniz yönetilen kimliklerden yararlanamıyorsa, [sertifika tabanlı çözümü](certificate-based-authentication.md)kullanın. Hem yönetilen kimlik çözümü hem de sertifika tabanlı çözüm ihtiyaçlarınızı karşılamazsa, lütfen aşağıdaki anahtar kasa çözümlerini kullanın.

Uygulamalarınız için Azure Cosmos DB kullanırken, uygulamanın yapılandırma dosyasındaki bitiş noktasını ve anahtarı kullanarak veritabanına, koleksiyonlara, belgelere erişebilirsiniz.  Ancak, tüm kullanıcılara açık metin biçiminde kullanılabilir olduğundan, anahtarları ve URL'yi doğrudan uygulama koduna koymak güvenli değildir. Uç noktanın ve anahtarların sağlanmasını ama güvenli bir mekanizma üzerinden sağlanmasını istersiniz. İşte bu noktada Azure Key Vault uygulama gizli dizilerini güvenle depolamanıza ve yönetmenize yardımcı olabilir.

Key Vault'tan Azure Cosmos DB erişim anahtarlarını depolamak ve okumak için aşağıdaki adımlar gereklidir:

* Anahtar kasası oluşturma  
* Key Vault'a Azure Cosmos DB erişim tuşlarını ekleyin  
* Azure web uygulaması oluşturma  
* Anahtar Kasası'nı okumak için başvuruyu kaydedin & izinler ver  


## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın.  
2. **Güvenlik > Anahtar Kasası > kaynak oluştur'u**seçin.  
3. **Anahtar kasası oluşturma** bölümünde aşağıdaki bilgileri sağlayın:  
   * **Adı:** Key Vault için benzersiz bir ad sağlayın.  
   * **Abonelik:** Kullanacağınız aboneliği seçin.  
   * **Kaynak Grubu** altında **Yeni oluştur**’u seçin ve bir kaynak grubu adı girin.  
   * Konum çekme menüsünde bir konum seçin.  
   * Diğer seçenekleri varsayılanlarına bırakın.  
4. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin.  

## <a name="add-azure-cosmos-db-access-keys-to-the-key-vault"></a>Key Vault'a Azure Cosmos DB erişim tuşlarını ekleyin.
1. Önceki adımda oluşturduğunuz Anahtar Kasası'na gidin, **Sırlar** sekmesini açın.  
2. **+Oluştur/İçe Aktar'** ı seçin , 

   * **Yükleme seçenekleri**için **El Kitabı'nı** seçin.
   * Sırrınız için bir **Ad** sağlayın
   * Cosmos DB hesabınızın bağlantı dizesini **Değer** alanına sağlayın. Ve sonra **Oluştur'u**seçin.

   ![Gizli anahtar oluşturma](./media/access-secrets-from-keyvault/create-a-secret.png)

4. Gizli oluşturulduktan sonra, açın ve aşağıdaki biçimde **Secret Identifier kopyalayın. Bu tanımlayıcıyı sonraki bölümde kullanacaksınız. 

   `https://<Key_Vault_Name>.vault.azure.net/secrets/<Secret _Name>/<ID>`

## <a name="create-an-azure-web-application"></a>Azure web uygulaması oluşturma

1. Bir Azure web uygulaması oluşturun veya uygulamayı [GitHub deposundan](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/Demo/keyvaultdemo)indirebilirsiniz. Bu basit bir MVC uygulamasıdır.  

2. İndirilen uygulamanın zip'ini açın ve **HomeController.cs** dosyasını açın. Gizli kimliği aşağıdaki satırda güncelleştirin:

   `var secret = await keyVaultClient.GetSecretAsync("<Your Key Vault’s secret identifier>")`

3. Dosyayı **kaydet,** çözümü **oluşturun.**  
4. Ardından uygulamayı Azure'a dağıtın. Projeye sağ tıklayın ve **yayımla'yı**seçin. Yeni bir uygulama hizmeti profili oluşturun (uygulamaya WebAppKeyVault1 adını verebilir) ve **Yayımla'yı**seçin.   

5. Uygulama dağıtıldıktan sonra. Azure portalından, dağıttığınız web uygulamasına gidin ve bu uygulamanın **Yönetilen hizmet kimliğini** açın.  

   ![Yönetilen hizmet kimliği](./media/access-secrets-from-keyvault/turn-on-managed-service-identity.png)

Uygulamayı şimdi çalıştıracaksanız, Key Vault'ta bu uygulamaya herhangi bir izin vermediniz gibi aşağıdaki hatayı görürsünüz.

![Uygulama erişim olmadan dağıtıldı](./media/access-secrets-from-keyvault/app-deployed-without-access.png)

## <a name="register-the-application--grant-permissions-to-read-the-key-vault"></a>Anahtar Kasası'nı okumak için başvuruyu kaydedin & izinler ver

Bu bölümde, uygulamayı Azure Etkin Dizini'ne kaydediyor ve uygulamanın Anahtar Kasası'nı okumasına izin verebilirsiniz. 

1. Azure portalına gidin, önceki bölümde oluşturduğunuz **Anahtar Kasası'nı** açın.  

2. **Access ilkelerini**aç, **+Yeni Ekle'yi** seçin, dağıttığınız web uygulamasını bulun, izinleri seçin ve **Tamam'ı**seçin.  

   ![Erişim ilkesi ekleme](./media/access-secrets-from-keyvault/add-access-policy.png)

Şimdi, eğer başvuruyu çalıştırArsanız, Key Vault'tan sırrı okuyabilirsiniz.

![Uygulama gizli olarak dağıtıldı](./media/access-secrets-from-keyvault/app-deployed-with-access.png)
 
Benzer şekilde, Vault tuşuna erişmek için bir kullanıcı ekleyebilirsiniz. **Erişim İlkeleri'ni** seçerek kendinizi Key Vault'a eklemeniz ve ardından uygulamayı Visual studio'dan çalıştırmak için gereken tüm izinleri vermeniz gerekir. Bu uygulama masaüstünüzden çalışırken kimliğinizi alır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için bir güvenlik duvarı yapılandırmak için [güvenlik duvarı destek makalesine](firewall-support.md) bakın.
* Sanal ağ hizmeti bitiş noktasını yapılandırmak için [VNet hizmeti bitiş noktası makalesini kullanarak güvenli erişime](vnet-service-endpoint.md) bakın.
