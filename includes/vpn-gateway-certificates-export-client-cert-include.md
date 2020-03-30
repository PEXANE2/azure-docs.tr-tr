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
ms.openlocfilehash: d16412e4e35714c840516670f520f77daed1676d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059952"
---
İstemci sertifikası oluşturduğunuzda, bu sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, oluşturduğunuz istemci sertifikasını dışa aktarmanız gerekir.

1. Bir istemci sertifikasını dışarı aktarmak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Oluşturduğunuz istemci sertifikaları varsayılan olarak 'Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar'da bulunur. Dışa aktarmak istediğiniz istemci sertifikasını sağ tıklatın, **tüm görevleri**tıklatın ve sonra **Sertifika Dışa Aktarma Sihirbazı'nı**açmak için **Dışa Aktar'ı** tıklatın.

   ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. Sertifika Dışa Aktarma Sihirbazı'nda devam etmek için **İleri'yi** tıklatın.

   ![Sonraki](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. **Evet'i seçin, özel anahtarı dışa aktarın**ve sonra **İleri'yi**tıklatın.

   ![özel anahtarı dışa aktarma](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında, varsayılan ayarları seçili bırakın. **Mümkünse sertifika yolundaki tüm sertifikaları ekle** seçeneğinin işaretli olduğundan emin olun. Bu ayar ayrıca başarılı istemci kimlik doğrulaması için gereken kök sertifika bilgilerini de dışa dışa dışa dışa dışa aktarmaz. İstemci güvenilir kök sertifikasına sahip olmadığından istemci kimlik doğrulaması başarısız olur. Ardından **İleri'yi**tıklatın.

   ![dosya biçimini dışa aktarma](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. **Güvenlik** sayfasında, özel anahtarı korumanız gerekir. Bir parola kullanmayı seçerseniz, bu sertifika için ayarladığınız parolayı kaydettiğinizden ya da unutmayacağınızdan emin olun. Ardından **İleri'yi**tıklatın.

   ![güvenlik](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri'yi**tıklatın.

   ![dosya dışa aktarmak için](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![bitirme](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)