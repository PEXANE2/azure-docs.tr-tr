---
title: Azure Uygulaması teklif test sürüşü | Azure Marketi
description: Azure Marketi'nde Azure Uygulaması teklifi için test sürüşü nasıl yapılandırılabilen.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 2f35dd69c1f29350049ed8f62dd9cbb0e60a7718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289014"
---
# <a name="azure-applications-test-drive-tab"></a>Azure uygulamaları Test Sürüşü sekmesi

Müşterilerinize deneme deneyimi sağlamak için Test Sürüşü sekmesini kullanın.

## <a name="test-drive-benefits"></a>Test Sürüşü avantajları

Müşterileriniz için deneme deneyimi oluşturmak, güvenle satın alabileceklerinden emin olmak için en iyi yöntemdir. Mevcut deneme seçenekleri arasında, Test Sürüşü yüksek kaliteli müşteri adayları oluşturmada ve bu müşteri adaylarının daha fazla dönüşüme dönüştürülmesinde en etkili olandır.

Müşterilere, gerçek dünya uygulama senaryosunda gösterilen, ürününüzün temel özellikleri ve avantajlarının uygulamalı, kendi kendine yönlendirilmiş bir deneme sürümü sağlar.

## <a name="how-a-test-drive-works"></a>Test sürüşü nasıl çalışır?

Olası bir müşteri, Market'te uygulamanızı arar ve keşfeder. Müşteri kabul eder ve kullanım koşullarını kabul eder. Bu noktada, müşteri belirli bir saat sayısını denemek için önceden yapılandırılmış ortamınızı alır, siz de takip etmek için yüksek nitelikli bir müşteri adayı alırsınız. Daha fazla bilgi için test [sürüşü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="setting-up-a-test-drive"></a>Test sürüşü ayarlama

Bir test sürücüsünün etkinleştirilmesini ve yapılandırılmasını sağlamak için aşağıdaki adımları kullanın.

### <a name="to-enable-a-test-drive"></a>Test sürüşü etkinleştirmek için:

1. **Yeni Teklif**altında, **Test Sürüşü** sekmesini seçin.
2. **Test Sürüşü**altında, Test **Sürüşünü Etkinleştir**için **Evet'i** seçin.

   ![Test sürüşü etkinleştirme](./media/managed-app-enable-testdrive.png)

### <a name="to-configure-a-test-drive"></a>Bir test sürüşü yapılandırmak için:

Bir test sürüşü etkinleştirdikten sonra, test sürüşü ayarlamak için aşağıdaki formları doldurursunuz:
  
 - Ayrıntılar
 - Teknik Yapılandırma
 - Test Sürüşü Dağıtım Abonelik Ayrıntıları

Sonraki ekran yakalama tüm Test Drive formlarını gösterir. Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir. 

![Test sürüşü yapılandırma](./media/managed-app-configure-testdrive.png)

Aşağıdaki tabloda yönetilen uygulamanız için test sürücüsünün ayarlamıştırılması gereken alanlar açıklanmaktadır.  Yıldız işaretiyle eklenen alanlar gereklidir.

|      Alan         |  Açıklama      |
|  ---------------   |  ---------------  |
| **Açıklama\***  |  Test Sürüşünüzde neler yapılabileceğini açıklayın. Bu açıklamayı biçimlendirmek için temel HTML etiketlerini kullanabilirsiniz. &lt;Örneğin, p&gt; &lt;,&gt;em&gt; &lt;,&gt; &lt; &lt;ul , li , ol&gt;, ve başlıkları.                |
| **Kullanım Kılavuzu\***  |  Müşterilerinizin Test Sürüşü deneyiminde yürümek için kullanabileceği bir kullanım kılavuzu yükleyin. Bu belge bir .pdf dosyası olmalıdır.    |
| **Test Sürüşü Demo Videosu** |  Test Sürüşünüzün isteğe bağlı video gezintisi. Bir müşteri test sürüşüne çıkmadan önce bu videoyu izleyebilir. YouTube veya Vimeo'daki videonun URL'sini sağlayın. **+ Video Ekle'yi**seçerseniz, aşağıdaki bilgileri sağlamanız istenir:<ul><li>Adı</li><li>URL'si</li><li>Küçük resim (PNG formatında, 533 x 324 piksel)</li></ul>  |
| **Örnekler\***      | İstediğiniz kaç örnek, hangi bölgede ve müşterilerinizin Test Sürüşü'ne ne kadar hızlı ulaşabileceğini yapılandırın. Daha fazla bilgi için test [sürüşü nasıl yayımlandırılabildiğini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive#how-to-publish-a-test-drive)öğrenin.           |
| **Test Sürüşü Süresi (saat)\*** | Saat sayısı için bir arayıcı girin. İzin verilen aralık 1 ile 999 arasındadır. |
| **Test Sürüşü ARM Şablonu\***     | Uygulamanız için Azure Kaynak Yöneticisi Şablonlarınızı içeren sıkıştırılmış (.zip) bir dosya yükleyin. Daha fazla bilgi için Azure [Kaynak Yöneticisi Test Sürücüsü'ne](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)bakın. |
| **Erişim Bilgileri\***          | Müşteriniz Test Sürüşü aldıktan sonra erişim bilgilerini sağlayın. Örneğin, test sürüşüne erişmek ve bilgileri imzalamak için bir URL. . Bu açıklamayı biçimlendirmek için temel HTML etiketlerini kullanabilirsiniz. &lt;Örneğin, p&gt; &lt;,&gt;em&gt; &lt;,&gt; &lt; &lt;ul , li , ol&gt;, ve başlıkları. |
| **Azure Abonelik Kimliği\***       | Bu, Azure hizmetlerine ve Azure portalına erişim izni verir. Abonelik, kaynak kullanımının raporlandığı ve hizmetlerin faturalandırıldığı yerdir. Yalnızca Test Sürücüleri için ayrı bir Azure Aboneliğiniz yoksa, bir abonelik oluşturun.  |
| **Azure AD Kiracı Kimliği\***          | Azure Etkin Dizini'nde varolan bir Kiracı sağlayın veya bu test sürüşü için bir kiracı oluşturun.  |
| **Azure AD Uygulama Kimliği\***             | Yeni bir uygulama oluşturun ve kaydedin. Microsoft, Test Sürücüsü örneğiniz üzerinde işlemleri gerçekleştirmek için bu uygulamayı kullanır.  |
| **Azure AD Uygulama Anahtarı\***            | Uygulama için bir kimlik doğrulama anahtarı oluşturun ve bu alana yapıştırın.   |
|  |  |

Gerekli tüm bilgileri sağladıktan sonra, test sürüşünü ayarlamayı bitirmek için **Kaydet'i** seçin.


## <a name="next-steps"></a>Sonraki adımlar

[Market sekmesi](./cpp-marketplace-tab.md)
