---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67448615"
---
Düzgün bir SSL sertifikası şifrelenmiş bilgileri doğru sunucuya gönderdiğinizden emin olmanızı sağlar. Şifrelemenin yanı sıra, sertifika kimlik doğrulamasına da olanak tanır. Aygıtın PowerShell arabirimi aracılığıyla kendi güvenilen SSL sertifikanızı karşıya yükleyebilirsiniz.

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Set-HcsCertificate`Sertifikayı karşıya yüklemek için cmdlet 'ini kullanın. İstendiğinde, aşağıdaki parametreleri sağlayın:

   - `CertificateFilePath` - *. Pfx* biçiminde sertifika dosyasını içeren paylaşımın yolu.
   - `CertificatePassword` -Sertifikayı korumak için kullanılan parola.
   - `Credentials` -Sertifikayı içeren paylaşıma erişmek için Kullanıcı adı. İstendiğinde ağ paylaşımının parolasını girin.

     Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```

