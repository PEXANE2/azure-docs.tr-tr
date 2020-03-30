---
title: Sertifikalı Güvenli B2B iletileri
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta B2B iletilerinin güvenliğini sağlamaya yardımcı olmak için sertifika ekleme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/17/2018
ms.openlocfilehash: c1b48ae8191e2e5313d9037c791eca73c8a55691
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191394"
---
# <a name="improve-security-for-b2b-messages-by-using-certificates"></a>Sertifikalar kullanarak B2B iletilerinin güvenliğini artırın

B2B iletişimini gizli tutmanız gerektiğinde, b2B iletişiminin güvenliğini, özellikle de mantıksal uygulamalarınızla, entegrasyon hesabınıza sertifika ekleyerek artırabilirsiniz. Sertifikalar, elektronik iletişimde katılımcıların kimliklerini kontrol eden ve iletişimi şu şekilde sağlamanıza yardımcı olan dijital belgelerdir:

* İleti içeriğini şifreleyin.
* İletileri dijital olarak imzalayın.

Bu sertifikaları kurumsal tümleştirme uygulamalarınızda kullanabilirsiniz:

* Genel internet [sertifikası yetkilisinden (CA)](https://en.wikipedia.org/wiki/Certificate_authority) satın almanız gereken ancak herhangi bir anahtar gerektirmeyen [herkese açık sertifikalar.](https://en.wikipedia.org/wiki/Public_key_certificate) 

* Oluşturduğunuz ve kendiniz oluşturduğunuz ancak özel anahtarlar da gerektiren özel sertifikalar veya [*kendi imzalı sertifikalar.*](https://en.wikipedia.org/wiki/Self-signed_certificate) 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upload-a-public-certificate"></a>Herkese açık sertifika yükleme

B2B özelliklerine sahip mantık uygulamalarında *herkese açık* bir sertifika kullanmak için, önce sertifikayı entegrasyon hesabınıza yüklemeniz gerekir. Oluşturduğunuz [anlaşmalardaki](logic-apps-enterprise-integration-agreements.md) özellikleri tanımladıktan sonra, sertifika B2B iletilerinizi güvenli hale almanıza yardımcı olmak için kullanılabilir.

1. [Azure portalında](https://portal.azure.com)oturum açın. Ana Azure menüsünde **Tüm kaynakları**seçin. Arama kutusuna, entegrasyon hesabı adınızı girin ve ardından istediğiniz tümleştirme hesabını seçin.

   ![Entegrasyon hesabınızı bulun ve seçin](media/logic-apps-enterprise-integration-certificates/select-integration-account.png)  

2. **Bileşenler**altında, **Sertifikalar** döşemesini seçin.

   !["Sertifikalar"ı seçin](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

3. **Sertifikalar**altında **Ekle'yi**seçin. **Sertifika Ekle'nin**altında, sertifikanız için bu ayrıntıları sağlayın. İşiniz bittiğinde **Tamam**’ı seçin.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Adı** | <*sertifika adı*> | Bu örnekte "publicCert" olan sertifikanızın adı | 
   | **Sertifika Türü** | Genel | Sertifikanızın türü |
   | **Sertifika** | <*sertifika-dosya adı*> | Yüklemek istediğiniz sertifika dosyasını bulmak ve seçmek için **Sertifika** kutusunun yanındaki klasör simgesini seçin. |
   ||||

   !["Ekle"yi seçin, sertifika ayrıntılarını sağlayın](media/logic-apps-enterprise-integration-certificates/public-certificate-details.png)

   Azure seçiminizi doğruladıktan sonra, Azure sertifikanızı yükler.

   ![Azure yeni sertifikagörüntüler](media/logic-apps-enterprise-integration-certificates/new-public-certificate.png) 

## <a name="upload-a-private-certificate"></a>Özel sertifika yükleme

B2B özelliklerine sahip mantık uygulamalarında özel bir *sertifika* kullanmak için, önce sertifikayı entegrasyon hesabınıza yüklemeniz gerekir. Ayrıca, azure [key vault'a](../key-vault/key-vault-get-started.md)ilk eklediğiniz özel bir anahtarınız olması gerekir. 

Oluşturduğunuz [anlaşmalardaki](logic-apps-enterprise-integration-agreements.md) özellikleri tanımladıktan sonra, sertifika B2B iletilerinizi güvenli hale almanıza yardımcı olmak için kullanılabilir.

> [!NOTE]
> Özel sertifikalar için, iletileri imzalamak ve şifrelemek için [AS2 anlaşmasının](logic-apps-enterprise-integration-as2.md) **Gönder ve Al** ayarlarında görünen karşılık gelen bir herkese açık sertifika eklediğinizden emin olun.

1. [Azure Key Vault'a özel anahtarınızı ekleyin](../key-vault/certificate-scenarios.md#import-a-certificate) ve bir **Anahtar Adı**sağlayın.
   
2. Azure Key Vault'ta işlem gerçekleştirmesi için Azure Logic Apps'a yetki ver. Logic Apps hizmet ilkesine erişim sağlamak için, PowerShell komutunu kullanın, [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy), örneğin:

   `Set-AzKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. [Azure portalında](https://portal.azure.com)oturum açın. Ana Azure menüsünde **Tüm kaynakları**seçin. Arama kutusuna, entegrasyon hesabı adınızı girin ve ardından istediğiniz tümleştirme hesabını seçin.

   ![Entegrasyon hesabınızı bulun](media/logic-apps-enterprise-integration-certificates/select-integration-account.png) 

4. **Bileşenler**altında, **Sertifikalar** döşemesini seçin.  

   ![Sertifikalar döşemesini seçin](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

5. **Sertifikalar**altında **Ekle'yi**seçin. **Sertifika Ekle'nin**altında, sertifikanız için bu ayrıntıları sağlayın. İşiniz bittiğinde **Tamam**’ı seçin.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Adı** | <*sertifika adı*> | Bu örnekte "privateCert" olan sertifikanızın adı | 
   | **Sertifika Türü** | Özel | Sertifikanızın türü |
   | **Sertifika** | <*sertifika-dosya adı*> | Yüklemek istediğiniz sertifika dosyasını bulmak ve seçmek için **Sertifika** kutusunun yanındaki klasör simgesini seçin. Özel anahtar için anahtar kasası kullanırken, yüklenen dosya genel sertifika olacaktır. | 
   | **Kaynak Grubu** | <*tümleştirme-hesap-kaynak-grup*> | Bu örnekte "MyResourceGroup" olan tümleştirme hesabınızın kaynak grubu | 
   | **Anahtar Kasası** | <*anahtar-tonoz-adı*> | Azure anahtar kasanızın adı |
   | **Anahtar adı** | <*anahtar adı*> | Anahtarınızın adı |
   ||||

   !["Ekle"yi seçin, sertifika ayrıntılarını sağlayın](media/logic-apps-enterprise-integration-certificates/private-certificate-details.png)

   Azure seçiminizi doğruladıktan sonra, Azure sertifikanızı yükler.

   ![Azure yeni sertifikagörüntüler](media/logic-apps-enterprise-integration-certificates/new-private-certificate.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [B2B anlaşması oluşturma](logic-apps-enterprise-integration-agreements.md)
