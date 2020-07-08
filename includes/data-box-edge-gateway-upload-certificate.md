---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67448615"
---
Doğru bir SSL sertifikası, doğru sunucuya şifreli bilgiler gönderiyor olmanızı sağlar. Şifrelemenin yanı sıra, sertifika da kimlik doğrulaması için izin verir. Aygıtın PowerShell arabirimi aracılığıyla kendi güvenilen SSL sertifikanızı karşıya yükleyebilirsiniz.

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Set-HcsCertificate`Sertifikayı karşıya yüklemek için cmdlet 'ini kullanın. İstendiğinde, aşağıdaki parametreleri sağlayın:

   - `CertificateFilePath`- *. Pfx* biçiminde sertifika dosyasını içeren paylaşımın yolu.
   - `CertificatePassword`-Sertifikayı korumak için kullanılan parola.
   - `Credentials`-Sertifikayı içeren paylaşıma erişmek için Kullanıcı adı. İstendiğinde ağ paylaşımının parolasını girin.

     Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```

