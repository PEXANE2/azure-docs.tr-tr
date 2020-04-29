---
title: Azure Marketi için Bulut İş Ortağı Portalı sanal makine test sürücüsü sekmesi
description: Azure Marketi VM teklifi oluşturma bölümünde kullanılan test sürücüsü sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: adac73d64feb6280c5043776249072e9f7595faa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142981"
---
# <a name="virtual-machine-test-drive-tab"></a>Sanal makine sınama sürücüsü sekmesi

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

**Yeni teklif** sayfasının **test sürücüsü** sekmesi, olası müşterilerinizi, standart bir senaryoda gösterilen, ürününüzün temel özelliklerinin ve avantajlarının uygulamalı, kendinden kılavuzlu bir tanıtımı ile sağlamanıza olanak sağlar.  Test sürücüsü, test sürücüsünü destekleyen teklif türleri için isteğe bağlı bir özelliktir.  Sınama sürücüsü, destekleyici varlıkların düzgün bir şekilde uygulandığından emin olmanızı gerektirir.  Daha fazla bilgi için [Azure Marketi sınama sürücüsü](https://azure.microsoft.com/blog/azure-marketplace-test-drive/)makalesine bakın.  <!--TD: Replace with migrated version of Test Drive article! -->

Bu özelliği etkinleştirmek için, **test sürücüsü** sekmesinde, **Test sürücüsünü etkinleştir**' de **Evet** seçeneğine tıklayın.  **Test sürücüsü** sekmesi, düzenlenecek alanları görüntüler.  Alan adı üzerinde eklenen bir yıldız işareti (*) gerekli olduğunu gösterir.

![Sanal makineler için yeni teklif formunda sürücü sına sekmesi](./media/publishvm_007.png)


## <a name="field-values"></a>Alan değerleri

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır.  Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.


|    Alan                  |       Açıklama                                                            |
|  ---------                |     ---------------                                                          |
|  *Bilgileri*   |  |
| **Açıklama\***           | Test sürücünüzün senaryosuna genel bir bakış sağlar. Sınama sürücüsü sağlanırken bu metin kullanıcıya gösterilir. Bu alan, biçimlendirilen içerik sağlamak istiyorsanız temel HTML 'yi destekler.  |
| **Kullanıcı El Ile\***           | Test sürücüsü kullanıcılarının çözümünüzü nasıl kullanacağınızı anlamalarına yardımcı olan ayrıntılı bir kullanıcı el ile (. PDF) yükleyin.  |
| **Test sürücüsü tanıtım videosu** | Çözümünüze servis talebi gösteren bir video yükleyin.  Bu seçeneği tercih ederseniz, video için bir ad, video URL 'SI (YouTube veya Vimeo 'da barındırılır) ve (533x324 piksel) küçük resmini sağlamanız gerekir. |
| *Teknik yapılandırma* |  |
| **Örnekler\***             | Bölge kullanılabilirliğini ve sanal makine örneğinin görece kullanılabilirliğini belirtin (daha fazla ayrıntı için bilgi simgesine tıklayın).  <br/>Potansiyel eşzamanlı test sürücüsü oturumları, aboneliğinizin kota sınırını aşmamalıdır.  Bu değer, şu şekilde hesaplanır: [Seçili bölge sayısı] x [etkin örnekler] + [Seçili bölge sayısı] x [normal örnekler] + [Seçili bölge sayısı] x [soğuk örnekleri] |
| **Sınama sürücüsü süresi\***   | Saat cinsinden maksimum oturum süresi. Test sürücüsü oturumu bu süre dolduktan sonra otomatik olarak sonlandırılır.  |
|**Test sürücüsü ARM şablonu\***| Bu test sürücüsüyle ilişkili Azure Resource Manager şablonunu karşıya yükleyin. Daha fazla bilgi için bkz. [test sürücüsü Için sanal makine Dağıtım şablonunu dönüştürme](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive). |
| **Erişim bilgileri\***    | Azure Resource Manager, salt metin veya basit HTML olarak yazılmış erişim ve deneme oturum açma bilgileri. |
| *Test sürücüsü dağıtım aboneliği ayrıntıları* |  |
| **Azure Abonelik Kimliği\*** | [Microsoft Azure Portal](https://ms.portal.azure.com) oturum açarak ve sol menü çubuğundan **abonelikler** ' e tıklayarak elde edilebilir. (Örnek: "a83645ac-1234-5AB6-345-1h234g764ghty")    Bu tanımlayıcı formun `a83645ac-1234-5ab6-6789-1h234g764ghty`bir GUID olmalıdır.|
| **Azure AD Kiracı kimliği\***    | Azure Active Directory kiracı KIMLIĞI.  [Microsoft Azure Portal](https://ms.portal.azure.com) oturum açarak ve sol menü çubuğundan **Azure Active Directory** ' a tıklayıp ortadaki menü çubuğunda **Özellikler** ' e tıklayıp sonra da **dizin kimliğini** formdan kopyalayarak elde edilebilir.  Bu tanımlayıcı aynı zamanda bir GUID olmalıdır.  Boşsa, kuruluşunuz için bir kiracı KIMLIĞI oluşturmanız gerekir. |
| **Azure AD Uygulaması Kimliği\***       | Kayıtlı Azure VM çözümünüz için tanımlayıcı  |
| **Azure AD Uygulaması anahtarı\***      | Kayıtlı çözümünüzün kimlik doğrulama anahtarı |
|   |   |


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki [Market](./cpp-marketplace-tab.md) sekmesinde, çözümünüz hakkında pazarlama ve yasal bilgiler sunacaktır.
