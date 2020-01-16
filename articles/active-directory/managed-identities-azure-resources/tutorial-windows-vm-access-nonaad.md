---
title: Eğitim`:` Azure Key Vault erişmek için yönetilen kimlik kullanma-Windows-Azure AD
description: Windows VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Key Vault’a erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd9f85e3bfd11ee655ce581c60a5b65e13f4497b
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75971885"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-key-vault"></a>Öğretici: Azure Key Vault'a erişmek için Windows VM sistem tarafından atanan yönetilen kimlik kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Azure Key Vault'a erişmek amacıyla, Windows sanal makinesi (VM) için sistem tarafından atanmış bir yönetilen kimliği nasıl kullanacağınız gösterilmektedir. Önyükleme işlevi gören Key Vault, istemci uygulamanızın gizli diziyi kullanarak Azure Active Directory (AD) tarafından güvenlik altına alınmamış kaynaklara erişmenize olanak sağlar. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * VM'nize Key Vault'ta depolanan gizli diziye erişim verme 
> * VM kimliği kullanarak erişim belirteci alma ve Key Vault'tan gizli diziyi almak için bunu kullanma 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="enable"></a>Etkinleştirme

[!INCLUDE [msi-tut-enable](../../../includes/active-directory-msi-tut-enable.md)]



## <a name="grant-access"></a>Erişim verme  
 
Bu bölümde, VM 'nizin Key Vault depolanan bir gizli dizi erişimine nasıl izin vereceğiniz gösterilmektedir. Azure kaynakları için yönetilen kimlikler kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için erişim belirteçleri alabilir.  Bununla birlikte, tüm Azure hizmetleri Azure AD kimlik doğrulamasını desteklemez. Bu hizmetlerle Azure kaynakları için yönetilen kimlikleri kullanmak için, hizmet kimlik bilgilerini Azure Key Vault depolayın ve VM 'nin yönetilen kimliğini kullanarak kimlik bilgilerini almak için Key Vault erişin. 

İlk olarak, Key Vault'u oluşturmalı ve VM'mize Key Vault üzerinde sistem tarafından atanan yönetilen kimlik erişimi vermeliyiz.   

1. Sol gezinti çubuğunun üst kısmında **Kaynak oluştur** > **Güvenlik + Kimlik** > **Key Vault**'u seçin.  
2. Yeni Key Vault için bir **Ad** belirtin. 
3. Key Vault'u daha önce oluşturduğunuz VM'yle aynı aboneliğe ve kaynak grubuna yerleştirin. 
4. **Erişim ilkeleri**’ni seçin ve **Yeni ekle**'ye tıklayın. 
5. Şablondan yapılandır'da **Gizli Dizi Yönetimi**'ni seçin. 
6. **Sorumlu Seç**'i seçin ve arama alanına daha önce oluşturduğunuz VM'nin adını girin.  Sonuç listesinde VM 'yi seçin ve **Seç**' e tıklayın. 
7. Yeni erişim ilkesini ekleme işlemini tamamlamak için **Tamam**'a tıklayın ve erişim ilkesi seçimini bitirmek için de **Tamam**'a tıklayın. 
8. Key Vault oluşturmayı tamamlamak için **Oluştur**'a tıklayın. 

    ![Alternatif resim metni](./media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Ardından, Key Vault'a bir gizli dizi ekleyin; böylelikle VM'nizde çalıştırılan kodu kullanarak daha sonra gizli diziyi alabilirsiniz: 

1. **Tüm Kaynaklar**'ı seçin, sonra da oluşturduğunuz Key Vault'u bulun ve seçin. 
2. **Gizli Diziler**'i seçin ve **Ekle**'ye tıklayın. 
3. **Karşıya yükleme seçenekleri**'nden **El ile** seçeneğini belirtin. 
4. Gizli dizi için bir ad ve değer girin.  Değer, istediğiniz herhangi bir şey olabilir. 
5. Etkinleştirme tarihi ile sona erme tarihini boş bırakın ve **Etkin** seçeneğini **Evet** değerinde bırakın. 
6. Gizli diziyi oluşturmak için **Oluştur**'a tıklayın. 
 
## <a name="access-data"></a>Verilere erişme  

Bu bölüm, VM kimliğini kullanarak bir erişim belirtecinin nasıl alınacağını ve Key Vault gizli anahtarı almak için nasıl kullanılacağını gösterir. PowerShell 4.3.1 veya üstünü yüklemediyseniz, [en son sürümü indirip yüklemeniz gerekir](https://docs.microsoft.com/powershell/azure/overview).

İlk olarak, Key Vault'ta kimlik doğrulaması yapmak üzere erişim belirteci almak için VM’nin sistem tarafından atanan yönetilen kimliğini kullanırız:
 
1. Portalda, **Sanal Makineler**'e ve Windows sanal makinenize gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın.
2. **Windows VM'sini** oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin.  
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.  
4. PowerShell'de, VM için belirtilen bağlantı noktasında yerel konağın belirtecini almak üzere kiracıda web isteğini çağırın.  

    PowerShell isteği:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan tam yanıtı ayıklayın.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Ardından, yanıttan erişim belirtecini ayıklayın.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Son olarak, PowerShell’in Invoke-WebRequest komutunu kullanarak Authorization üst bilgisindeki erişim belirtecini geçirerek Key Vault'ta daha önce oluşturmuş olduğunuz gizli bilgiyi alın.  Key Vault **genel bakış** sayfasının **temel** bileşenler bölümünde bulunan Key Vault URL 'sine ihtiyacınız vardır.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Yanıt şöyle görünür: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Key Vault'tan gizli diziyi aldıktan sonra, bunu kullanarak ad ve parola gerektiren bir hizmette kimlik doğrulaması yapabilirsiniz. 


## <a name="disable"></a>Devre dışı bırakma

[!INCLUDE [msi-tut-disable](../../../includes/active-directory-msi-tut-disable.md)]



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Key Vault'a erişmek için Windows VM sistem tarafından atanan yönetilen kimlik kullanmayı öğrendiniz.  Azure Key Vault hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Anahtar Kasası.](/azure/key-vault/key-vault-overview)
