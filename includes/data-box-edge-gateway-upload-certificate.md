---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67448615"
---
Uygun bir SSL sertifikası, şifreli bilgileri doğru sunucuya göndermenizi sağlar. Şifrelemenin yanı sıra, sertifika kimlik doğrulamasına da izin verir. Cihazın PowerShell arabirimi üzerinden kendi güvenilir SSL sertifikanızı yükleyebilirsiniz.

1. [PowerShell arabirimine bağlanın.](#connect-to-the-powershell-interface)
2. Sertifikayı `Set-HcsCertificate` yüklemek için cmdlet'i kullanın. İstendiğinde, aşağıdaki parametreleri sağlayın:

   - `CertificateFilePath`- Sertifika dosyasını *.pfx* formatında içeren paylaşıma giden yol.
   - `CertificatePassword`- Sertifikayı korumak için kullanılan bir parola.
   - `Credentials`- Sertifikayı içeren paylaşıma erişmek için kullanıcı adı. İstendiğinde ağ paylaşımının parolasını sağlayın.

     Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```

