---
title: Azure Marketi Için Bulut İş Ortağı Portalı'nda Sanal makine Test Sürüşü sekmesi
description: Azure Marketi VM teklifi oluştururken kullanılan Test Sürücüsü sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: d4fc8762e25825b21637b16b751d57a0dcbf369e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288810"
---
# <a name="virtual-machine-test-drive-tab"></a>Sanal makine Test Sürüşü sekmesi

**Yeni Teklif** sayfasının **Test Sürüşü** sekmesi, potansiyel müşterilerinize, standart laştırılmış bir senaryoda gösterilen, ürününüzün temel özelliklerinin ve avantajlarının uygulamalı, kendi kendine yönlendirilmiş bir gösterisini sunmanıza olanak tanır.  Test Sürüşü, Test Sürüşü'ni destekleyen teklif türleri için isteğe bağlı bir özelliktir.  Test Sürüşü, destekleyici varlıkların düzgün bir şekilde uygulanmasını gerektirir.  Daha fazla bilgi için [Azure Marketi Test Sürüşü](https://azure.microsoft.com/blog/azure-marketplace-test-drive/)makalesine bakın.  <!--TD: Replace with migrated version of Test Drive article! -->

Bu özelliği etkinleştirmek **için, Test Sürüşü** sekmesinde, **Test Sürüşü Etkinleştir'de** **Evet** seçeneğini tıklatın.  **Test Sürüşü** sekmesi, düzenlenebilen alanları görüntüler.  Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir.

![Sanal makineler için Yeni Teklif formunda Test Sürüşü sekmesi](./media/publishvm_007.png)


## <a name="field-values"></a>Alan değerleri

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır.  Gerekli alanlar yıldız işareti (*) tarafından suçlanır.


|    Alan                  |       Açıklama                                                            |
|  ---------                |     ---------------                                                          |
|  *Şey*   |  |
| **Açıklama\***           | Test Sürüşü senaryonuza genel bir bakış sağlayın. Bu metin, Test Sürüşü verilirken kullanıcıya gösterilir. Biçimlendirilmiş içerik sağlamak istiyorsanız, bu alan temel HTML'yi destekler.  |
| **Kullanım Kılavuzu\***           | Test Sürüşü kullanıcılarının çözümünüzü nasıl kullanacaklarını anlamalarına yardımcı olan ayrıntılı bir kullanım kılavuzu (.pdf) yükleyin.  |
| **Test Sürüşü Demo Videosu** | Çözümünüzü gösteren bir video yükleyin.  Bu seçeneği seçtiyseniz, videonun bir adı, URL'sini (YouTube veya Vimeo'da barındırılan) ve video için bir (533x324 piksel) küçük resmi sağlamanız gerekir. |
| *Teknik Yapılandırma* |  |
| **Örnekler\***             | Bölge kullanılabilirliğini ve vm örneğinin göreceli olarak kullanılabilirliğini belirtin (daha fazla ayrıntı için bilgi simgesini tıklatın).  <br/>Olası eşzamanlı Test Sürüşü oturumları aboneliğiniz için kota sınırını aşmamalıdır.  Bunlardan önceki olarak hesaplanır: [Seçilen Bölge Sayısı] x [Sıcak örnekler] + [Seçilen Bölge Sayısı] x [Sıcak örnekler] + [Seçilen Bölge Sayısı] x [Soğuk örnekler] x [Soğuk örnekler] |
| **Test Sürüşü Süresi\***   | Saat içinde maksimum seans süresi. Bu süre aşıldıktan sonra Test Sürüşü oturumu otomatik olarak sona erer.  |
|**Test Sürüşü ARM Şablonu\***| Bu Test Sürüşüile ilişkili Azure Kaynak Yöneticisi şablonuna yükleyin. Daha fazla bilgi için, [Test Sürüşü için Sanal Makine Dağıtım Şablonu Dönüştürme'ye](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive)bakın. |
| **Erişim Bilgileri\***    | Azure Kaynak Yöneticisi erişim ve deneme giriş bilgileri, düz metin veya basit HTML olarak yazılır. |
| *Test Sürüşü Dağıtım Abonelik Ayrıntıları* |  |
| **Azure Abonelik Kimliği\*** | [Microsoft Azure portalında](https://ms.portal.azure.com) oturum açarak ve sol menü çubuğundaki **Abonelikler'i** tıklatarak elde edilebilir. (Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")    Bu tanımlayıcı formun `a83645ac-1234-5ab6-6789-1h234g764ghty`bir GUID olmalıdır.|
| **Azure AD Kiracı Kimliği\***    | Azure Active Directory kiracı kimliği.  [Microsoft Azure portalında](https://ms.portal.azure.com) oturum açarak ve sol menü çubuğunda **Azure Active Directory'yi** tıklatarak, ardından orta menü çubuğundaki **Özellikler'i** tıklatarak ve ardından formdan **Dizin Kimliği** kopyalayarak elde edilebilir.  Bu tanımlayıcı da bir GUID olmalıdır.  Boşsa, kuruluşunuz için bir kiracı kimliği oluşturmanız gerekir. |
| **Azure AD Uygulama Kimliği\***       | Kayıtlı Azure VM çözümünüz için tanımlayıcı  |
| **Azure AD Uygulama Anahtarı\***      | Kayıtlı çözümünüz için kimlik doğrulama anahtarı |
|   |   |


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki [Pazar Yeri](./cpp-marketplace-tab.md) sekmesinde, çözümünüz hakkında pazarlama ve yasal bilgiler sağlayacaksınız.
