---
title: Quickstart - Azure Analiz Hizmetleri sunucu güvenlik duvarLarını yapılandırma | Microsoft Dokümanlar
description: Bu hızlı başlangıç, Azure portalını kullanarak bir Azure Çözümleme Hizmetleri sunucusu için bir güvenlik duvarı yapılandırmanıza yardımcı olur.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 68994f9b79af55b32527eed52bbc4e5866c89538
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79205150"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Hızlı başlangıç: Sunucu güvenlik duvarını yapılandırma - Portal

Bu hızlı başlangıç Azure Analysis Services sunucunuz için güvenlik duvarını yapılandırmanıza yardımcı olur. Güvenlik duvarını etkinleştirmek ve yalnızca sunucunuza erişen bilgisayarlar için IP adresi aralıkları yapılandırmak, sunucunuzu ve verilerinizi güvenlik altına almanın önemli bir parçasıdır.

## <a name="prerequisites"></a>Ön koşullar

- Aboneliğinizde bir Analysis Services sunucusu. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Sunucu oluşturma - Portal](analysis-services-create-server.md) veya [Hızlı başlangıç: Sunucu oluşturma - PowerShell](analysis-services-create-powershell.md)
- İstemci bilgisayarlar için bir veya birden çok IP adresi aralığı (gerekiyorsa).
- Power BI Premium'un veri alma (yenileme) ve paginated raporlar da dahil olmak üzere Azure Analiz Hizmetleri'ne bağlandığı bazı senaryolar, Power BI'den erişime izin ver etkinleştirildiğinde bile şu anda desteklenmez. Power BI Premium'dan Live Connect'i kullanmanın daha yaygın senaryosu desteklenir. Tüm Power BI Pro senaryoları desteklenir.


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın 

[Portalda oturum açın](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Güvenlik duvarını yapılandırma

1. Genel Bakış sayfasını açmak için sunucunuza tıklayın. 
2. **AYARLAR** > **Güvenlik Duvarı** > **Etkinleştir güvenlik duvarı**, **Üzerine**tıklayın .
3. Power BI hizmetinden DirectQuery erişimine izin vermek için, **Power BI'dan erişime izin ver** alanında **Açık** seçeneğine tıklayın.  
4. (İsteğe bağlı) Bir veya birden çok IP adresi aralığı belirtin. Her aralık için bir ad, başlangıç ve bitiş IP adresi girin. Güvenlik duvarı kural adı 128 karakterle sınırlı olmalıdır ve yalnızca büyük harfler karakterler, küçük karakterler, sayılar, alt puan ve tire içerebilir. Beyaz alanlara ve diğer özel karakterlere izin verilmez.
5. **Kaydet**'e tıklayın.

     ![Güvenlik duvarı ayarları](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, IP adresi aralıklarını silin veya güvenlik duvarını devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, sunucunuz için güvenlik duvarı yapılandırmayı öğrendiniz. Artık sunucunuz olduğuna ve bir güvenlik duvarıyla sunucunun güvenliğini sağladığınıza göre, ona portaldan temel bir örnek veri modeli ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenmeye yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
