---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 01/27/2021
ms.author: mimart
ms.openlocfilehash: 3458a4f3ba337d4c470c21fa5d89b28f5f2701a3
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98916936"
---
Henüz bir sertifikanız yoksa, bu öğretici için otomatik olarak imzalanan bir sertifika kullanabilirsiniz. Otomatik olarak imzalanan sertifika, bir sertifika yetkilisi (CA) tarafından imzalanmamış bir güvenlik sertifikasıdır. Sertifika yetkilisi tarafından imzalanan bir sertifika için tüm güvenlik garantisi vermez. 

# <a name="windows"></a>[Windows](#tab/windows)

Windows 'da, bir sertifika oluşturmak için PowerShell 'in [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 'ini kullanın.

1. Otomatik olarak imzalanan bir sertifika oluşturmak için bu PowerShell komutunu yürütün. `-Subject`Bağımsız değişkeni, uygulamanız için uygun şekilde değiştirin ve kiracı adı Azure AD B2C. Ayrıca, `-NotAfter` sertifika için farklı bir süre sonu belirtmek üzere tarihi de ayarlayabilirsiniz.

    ```PowerShell
    New-SelfSignedCertificate `
        -KeyExportPolicy Exportable `
        -Subject "CN=yourappname.yourtenant.onmicrosoft.com" `
        -KeyAlgorithm RSA `
        -KeyLength 2048 `
        -KeyUsage DigitalSignature `
        -NotAfter (Get-Date).AddMonths(12) `
        -CertStoreLocation "Cert:\CurrentUser\My"
    ```

1. **Kullanıcı sertifikalarını Yönet**  >  **Geçerli Kullanıcı**  >  **Kişisel**  >  **sertifikalarını** aç  >  *YourAppName.yourtenant.onmicrosoft.com*
1.   >  **Tüm görevler**  >  **dışarı aktarma** işlemini > sertifikayı seçin
1. **Evet**  >  **İleri**  >  **Evet ' i seçin, bir sonraki özel anahtarı dışarı aktar**  >  
1. **Dışarı aktarma dosya biçimi** için varsayılanları kabul et
1. Sertifika için bir parola girin.

Azure AD B2C. pfx dosya parolasını kabul edebilmesi için, parola, AES256-SHA256 ' nin aksine Windows sertifika deposu dışa aktarma yardımcı programındaki TripleDES-SHA1 seçeneğiyle şifrelenmelidir.

# <a name="macos"></a>[macOS](#tab/macos)

MacOS 'ta sertifika oluşturmak için anahtar zinciri erişimi 'nde [sertifika Yardımcısı](https://support.apple.com/guide/keychain-access/aside/glosa3ed0609/11.0/mac/11.0) 'nı kullanın.

1. [Mac 'Te Anahtarlık erişimi için otomatik olarak imzalanan sertifikalar oluşturma](https://support.apple.com/guide/keychain-access/kyca8916/mac)yönergelerini izleyin.
1. Mac 'inizde Anahtarlık erişimi uygulamasında oluşturduğunuz sertifikayı seçin.
1. Öğeleri dışarı aktar > dosya seçin.
1. Sertifikanızın kaydedileceği bir dosya adı seçin. Örneğin, **otomatik olarak imzalanan sertifika. p12**. 
1. **Dosya biçimi** Için **kişisel bilgi değişimi (. p12)** öğesini seçin.
1. **Kaydet**’i seçin.
1. Bir **parola** girin ve parolayı **doğrulayın** .
1. Dosya uzantısını olarak değiştirin `.pfx` . Örneğin, **self-signed-Certificate. pfx**. 

---
