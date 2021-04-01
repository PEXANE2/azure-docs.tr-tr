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
ms.openlocfilehash: 12e9bec0c560f1b068b07a1b6afe218a112e439f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94553238"
---
Otomatik olarak imzalanan bir kök sertifika oluşturduktan sonra kök sertifika ortak anahtar. cer dosyasını (özel anahtarı değil) dışarı aktarın. Daha sonra bu dosyayı Azure 'a yükleyeceksiniz. Aşağıdaki adımlar, otomatik olarak imzalanan kök sertifikanız için. cer dosyasını dışarı aktarmaya yardımcı olur:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Otomatik olarak imzalanan kök sertifikayı bulun (genellikle 'Certificates - Current User\Personal\Certificates' konumundadır) ve sağ tıklayın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Geçerli Kullanıcı \ kişisel \ sertifikalar altındaki sertifikayı bulamazsanız, yanlışlıkla "Sertifikalar-Geçerli Kullanıcı" yerine "Sertifikalar-Yerel bilgisayar" seçeneğini açmış olabilirsiniz. PowerShell kullanarak geçerli kullanıcı kapsamında sertifika yöneticisi 'ni açmak istiyorsanız konsol penceresine *certmgr* yazın.

   ![Ekran görüntüsü, geçerli kullanıcı için sertifikalar penceresini ve tüm görevlerden seçilen dışarı aktarma içeren bağlamsal menüyü gösterir.](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Sihirbazda, **İleri**' ye tıklayın.

   ![Sertifikayı dışarı aktarma](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

   ![Özel anahtarı dışarı aktarma](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

   ![Base-64 kodlu](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. **Dışarı aktarılacak dosya** için, sertifikayı dışarı aktarmak istediğiniz konuma **gidin** . **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**' ye tıklayın.

   ![Ekran görüntüsü, sertifika dışarı aktarma Sihirbazı 'Nı bir dosya adı metin kutusuyla ve bir tarama seçeneğiyle gösterir.](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Ekran görüntüsü, seçilen ayarlarla sertifika dışarı aktarma Sihirbazı ' nı gösterir.](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Sertifikanız başarıyla verildi.

   ![Ekran görüntüsü dışarı aktarmanın başarılı olduğunu belirten bir ileti gösterir.](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Verdiğiniz sertifika şuna benzer:

   ![Ekran görüntüsünde, c e r dosya adı uzantısına sahip bir sertifika simgesi ve dosya adı gösterilmektedir.](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Not defteri 'Ni kullanarak, dışarıya aktarılmış sertifikayı açarsanız, bu örneğe benzer bir şey görürsünüz. Blue bölümünde Azure 'a yüklenen bilgiler yer alır. Sertifikanızı Not defteri ile açarsanız ve buna benzemezse, genellikle bu, temel-64 ile kodlanmış X. 509.440 (. CER) biçiminde. Ayrıca, farklı bir metin Düzenleyicisi kullanmak istiyorsanız, bazı düzenleyicilerin arka planda istenmeden biçimlendirme getirebileceğini anlayın. Bu, metni Bu sertifikadan Azure 'a yüklerken sorun oluşturabilir.

   ![Not defteri ile aç](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
