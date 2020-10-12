---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
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

