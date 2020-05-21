---
title: Hızlı başlangıç-Azure Analysis Services sunucusu güvenlik duvarını yapılandırma | Microsoft Docs
description: Bu hızlı başlangıç, Azure portal kullanarak bir Azure Analysis Services sunucusu için bir güvenlik duvarı yapılandırmanıza yardımcı olur.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 48618815519fad31bff5d6a8d2d2edc82535f437
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697906"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Hızlı başlangıç: Sunucu güvenlik duvarını yapılandırma - Portal

Bu hızlı başlangıç Azure Analysis Services sunucunuz için güvenlik duvarını yapılandırmanıza yardımcı olur. Güvenlik duvarını etkinleştirmek ve yalnızca sunucunuza erişen bilgisayarlar için IP adresi aralıkları yapılandırmak, sunucunuzu ve verilerinizi güvenlik altına almanın önemli bir parçasıdır.

## <a name="prerequisites"></a>Ön koşullar

- Aboneliğinizde bir Analysis Services sunucusu. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Sunucu oluşturma - Portal](analysis-services-create-server.md) veya [Hızlı başlangıç: Sunucu oluşturma - PowerShell](analysis-services-create-powershell.md)
- İstemci bilgisayarlar için bir veya birden çok IP adresi aralığı (gerekiyorsa).
- Power BI Premium, veri içeri aktarma (yenileme) ve sayfalandırılmış raporlar dahil olmak üzere Azure Analysis Services bağlandığı bazı senaryolar, Power BI erişimine Izin ver özelliğinin etkin olduğu durumlarda bile şu anda desteklenmemektedir. Power BI Premium 'den canlı bağlantı kullanmanın daha yaygın senaryosu desteklenir. Tüm Power BI Pro senaryolar desteklenir.


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın 

[Portalda oturum açın](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Güvenlik duvarını yapılandırma

1. Genel Bakış sayfasını açmak için sunucunuza tıklayın. 
2. **Ayarlar**  >  **güvenlik duvarı**güvenlik  >  **duvarını etkinleştir**bölümünde **Açık**' a tıklayın.
3. Power BI hizmetinden DirectQuery erişimine izin vermek için, **Power BI'dan erişime izin ver** alanında **Açık** seçeneğine tıklayın.  
4. (İsteğe bağlı) Bir veya birden çok IP adresi aralığı belirtin. Her aralık için bir ad, başlangıç ve bitiş IP adresi girin. Güvenlik duvarı kuralı adı 128 karakterle sınırlı olmalıdır ve yalnızca büyük harf, küçük harf karakter, rakam, alt çizgi ve kısa çizgi karakterlerini içerebilir. Boşluk ve diğer özel karakterlere izin verilmez.
5. **Kaydet**’e tıklayın.

     ![Güvenlik duvarı ayarları](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, IP adresi aralıklarını silin veya güvenlik duvarını devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, sunucunuz için güvenlik duvarı yapılandırmayı öğrendiniz. Artık sunucunuz olduğuna ve bir güvenlik duvarıyla sunucunun güvenliğini sağladığınıza göre, ona portaldan temel bir örnek veri modeli ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenmeye yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
