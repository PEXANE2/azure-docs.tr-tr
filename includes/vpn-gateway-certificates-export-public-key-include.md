---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d2dba0f657b418267db90c07014dc8996ed12a10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059904"
---
Kendi imzalı kök sertifikası oluşturduktan sonra, kök sertifikası ortak anahtarı .cer dosyasını (özel anahtar değil) dışa aktarın. Daha sonra bu dosyayı Azure'a yüklersiniz. Aşağıdaki adımlar, kendi imzalı kök sertifikanız için .cer dosyasını dışa aktarmanıza yardımcı olur:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Otomatik olarak imzalanan kök sertifikayı bulun (genellikle 'Certificates - Current User\Personal\Certificates' konumundadır) ve sağ tıklayın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Sertifikayı Geçerli Kullanıcı\Kişisel\Sertifikalar altında bulamıyorsanız, yanlışlıkla "Sertifikalar - Geçerli Kullanıcı" yerine yanlışlıkla "Sertifikalar - Yerel Bilgisayar" olarak açmış olabilirsiniz. PowerShell'i kullanarak geçerli kullanıcı kapsamında Sertifika Yöneticisi'ni açmak istiyorsanız, konsol penceresine *certmgr* yazarsınız.

   ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Sihirbazda **İleri'yi**tıklatın.

   ![Sertifikayı dışarı aktarma](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

   ![Özel anahtarı dışa aktarmayın](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

   ![Base-64 kodlanmış](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. **DosyaNın Dışa Aktarılaması**için, sertifikayı dışa aktarmak istediğiniz konuma **göz atın.** **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri'yi**tıklatın.

   ![Göz at](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Son](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Sertifikanız başarıyla dışa aktarılır.

   ![Başarılı](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Dışa aktarılan sertifika şuna benzer:

   ![Verilen](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Dışa aktarılan sertifikayı Not Defteri'ni kullanarak açarsanız, bu örneğe benzer bir şey görürsünüz. Mavi deki bölüm Azure'a yüklenen bilgileri içerir. Sertifikanızı Not Defteri ile açarsanız ve buna benzer görünmüyorsa, genellikle bu, Base-64 kodlu X.509() kodlu Kullanarak dışa aktarmadığınız anlamına gelir. CER) biçimi. Ayrıca, farklı bir metin düzenleyicisi kullanmak istiyorsanız, bazı düzenleyicilerin arka planda istenmeyen biçimlendirme ler sunabileceğini anlayın. Bu, bu sertifikadaki metni Azure'a yüklediğinizde sorun yaratabilir.

   ![Notepad ile aç](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
