---
title: Quickstart - Azure portalında Analiz Hizmetleri sunucusu oluşturun | Microsoft Dokümanlar
description: Azure portalını kullanarak bir Azure Analiz Hizmetleri sunucusu örneğini nasıl hızlı bir şekilde oluşturabileceğinizi öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c1b22d4871d3e490f59be71b63569fdf563ce45
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75442832"
---
# <a name="quickstart-create-a-server---portal"></a>Hızlı başlangıç: Sunucu oluşturma - Portal

Bu hızlı başlangıçta, portalı kullanarak Azure aboneliğinizde bir Analysis Services sunucusu kaynağı oluşturma adımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar 

* **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
* **Azure Active Directory**: Aboneliğinizin bir Azure Active Directory Kiracısı ile ilişkilendirilmiş olması gerekir. Ayrıca bu Azure Active Directory içindeki bir hesapla Azure'da oturum açmış olmanız gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın 

[Portalda oturum açın](https://portal.azure.com)


## <a name="create-a-server"></a>Sunucu oluşturma

1. Tıklayın + Bir > kaynak**Analiz** > **Hizmetleri** **oluşturun.**

    ![Portal](./media/analysis-services-create-server/aas-create-server-portal.png)

2. **Analysis Services** sayfasında gerekli alanları doldurduktan sonra **Oluştur**'a basın.
   
   * **Sunucu adı**: Sunucuya başvurmak için kullanılacak benzersiz bir ad yazın. Sunucu adı küçük bir karakterle başlamalı ve 3 ila 128 küçük karakter ve sayı içermelidir. Beyaz alanlara ve özel karakterlere izin verilmez.
   * **Abonelik**: Bu sunucuyla ilişkilendirilecek aboneliği seçin.
   * **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya zaten var olan bir kaynak grubunu seçin. Kaynak grupları, bir Azure kaynağı koleksiyonunu yönetmenize yardımcı olmak üzere tasarlanmıştır. Daha fazla bilgi edinmek için bkz. [kaynak grupları](../azure-resource-manager/management/overview.md).
   * **Konum**: Sunucuyu barındıran Azure veri merkezi konumudur. En büyük kullanıcı tabanınıza en yakın konumu seçin.
   * **Fiyatlandırma katmanı**: Bir fiyatlandırma katmanı seçin. Test yapıyorsanız ve örnek model veritabanını yükleyecekseniz ücretsiz **D1** katmanını seçin. Daha fazla bilgi için bkz. [Azure Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/). 
   * **Yönetici**: Varsayılan olarak oturum açmış olduğunuz hesap olacaktır. Azure Active Directory'den farklı bir hesap seçebilirsiniz.
   * **Yedekleme Depolama Alanı**: İsteğe bağlıdır. [Depolama hesabınız](../storage/common/storage-introduction.md) varsa model yedekleme veritabanı olarak varsayılan yapabilirsiniz. [Yedekleme ve geri yükleme](analysis-services-backup.md) ayarlarını daha sonra da yapabilirsiniz.
   * **Depo anahtarı süre sonu**: İsteğe bağlıdır. Depo anahtarı için süre sonu belirtin.

Sunucunun oluşturulması genellikle bir dakikadan kısa sürer. **Portala Ekle**'yi seçtiyseniz yeni sunucunuzu görmek için portalınıza gidin. Veya sunucunuzun hazır olup olmadığını görmek için **Tüm Hizmetler** > **Çözümleme Hizmetleri'ne** gidin. Sunucular 1200 ve daha yüksek uyumluluk düzeylerinde tabular modelleri destekler. Model uyumluluk düzeyi Visual Studio veya SSMS'te belirtilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İhtiyacınız kalmadıysa sunucunuzu silebilirsiniz. Sunucunuzun **Genel Bakış** sayfasında **Sil**'e tıklayın. 

 ![Temizleme](./media/analysis-services-create-server/aas-create-server-cleanup.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Azure aboneliğinizde sunucu oluşturmayı öğrendiniz. Sunucuyu oluşturduğunuza göre bir sunucu güvenlik duvarı yapılandırarak (isteğe bağlı) güvenliğinin sağlanmasına yardımcı olacaksınız. Sunucunuza doğrudan portaldan örnek bir veri modeli de ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenme konusunda yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Quickstart: Sunucu güvenlik duvarLarını yapılandırma - Portal](analysis-services-qs-firewall.md)   
> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
