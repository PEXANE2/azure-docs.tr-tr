---
title: Azure Active Directory Denetim günlükleriyle Azure günlük tümleştirmesi | Microsoft Docs
description: Azure günlük tümleştirme hizmeti 'nin nasıl yükleneceğini ve günlüklerin Azure denetim günlüklerinden nasıl tümleştirileceğini öğrenin
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 0820ca227a4d0e8c71ed3f35cd8fa2841163d38f
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727643"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Azure Active Directory Denetim günlüklerini tümleştirme

Azure Active Directory (Azure AD) denetim olayları, Azure Active Directory oluşan ayrıcalıklı eylemleri belirlemenize yardımcı olur. [Azure Active Directory Denetim raporu olaylarını](../../active-directory/reports-monitoring/concept-audit-logs.md)inceleyerek izleyebilmeniz gereken olay türlerini görebilirsiniz.


>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanım dışı bırakılacak. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Azure Active Directory Denetim günlüklerini tümleştirme adımları

> [!NOTE]
> Bu makaledeki adımları denemeden önce, [kullanmaya başlama](azure-log-integration-get-started.md) makalesini gözden geçirmeniz ve ilgili adımları doldurmanız gerekir.

1. Komut istemi 'ni açın ve şu komutu çalıştırın:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Şu komutu çalıştırın: 
 
   ``azlog createazureid``

   Bu komut sizden Azure oturum açmanızı ister. Bu komut daha sonra Azure AD kiracılarında, oturum açmış kullanıcının yönetici, ortak yönetici veya sahip olduğu Azure aboneliklerini barındıran bir Azure Active Directory hizmet sorumlusu oluşturur. Oturum açan kullanıcı yalnızca Azure AD kiracısındaki bir Konuk Kullanıcı ise, komut başarısız olur. Azure kimlik doğrulaması, Azure AD aracılığıyla yapılır. Azure günlük tümleştirmesi için bir hizmet sorumlusu oluşturmak, Azure aboneliklerinden okumak üzere erişim verilen Azure AD kimliğini oluşturur.

3. Kiracı KIMLIĞINIZI sağlamak için aşağıdaki komutu çalıştırın. Komutu çalıştırmak için kiracı yöneticisi rolünün üyesi olmanız gerekir.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Örnek:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Azure Active Directory Denetim günlüğü JSON dosyalarının oluşturulduğunu onaylamak için aşağıdaki klasörleri denetleyin:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Aşağıdaki videoda, bu makalede ele alınan adımlar gösterilmektedir:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> JSON dosyalarındaki bilgileri güvenlik bilgileri ve olay yönetimi (SıEM) sisteminize getirme hakkında belirli yönergeler için SıEM satıcınıza başvurun.

Topluluk Yardımı [Azure günlük TÜMLEŞTIRMESI MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration)aracılığıyla kullanılabilir. Bu forum, Azure günlük tümleştirme topluluğundaki kişilerin, soruları, yanıtları, ipuçları ve püf noktalarını kullanarak birbirini desteklemesini sağlar. Ayrıca, Azure günlük tümleştirme ekibi bu forumu izler ve her zaman yardımcı olur.

Ayrıca, bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md)açabilirsiniz. Destek istediğiniz hizmet olarak **günlük tümleştirmesini** seçin.

## <a name="next-steps"></a>Sonraki adımlar
Azure günlük tümleştirmesi hakkında daha fazla bilgi edinmek için bkz.:

* [Azure günlükleri için Microsoft Azure günlük tümleştirmesi](https://www.microsoft.com/download/details.aspx?id=53324): Bu Indirme Merkezi sayfası, Azure günlük tümleştirmesi için Ayrıntılar, sistem gereksinimleri ve yükleme yönergeleri sağlar.
* [Azure günlük tümleştirmesine giriş](azure-log-integration-overview.md): Bu makalede Azure günlük tümleştirmesi, temel özellikleri ve nasıl çalıştığı açıklanır.
* [Azure günlük tümleştirmesi hakkında SSS](azure-log-integration-faq.md): Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Azure tanılama ve Azure denetim günlüklerine yönelik yeni özellikler](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Bu blog gönderisi sizi Azure denetim günlükleri ve Azure kaynaklarınızın işlemleriyle ilgili Öngörüler elde etmenize yardımcı olan diğer özellikleri tanıtır.
