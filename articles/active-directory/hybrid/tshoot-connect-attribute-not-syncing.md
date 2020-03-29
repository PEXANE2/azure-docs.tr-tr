---
title: Azure AD Connect'te eşitlemeyen bir özniteliği giderme | Microsoft Dokümanlar'
description: Bu konu, sorun giderme görevini kullanarak öznitelik eşitlemesi ile sorunları giderme için nasıl adımlar sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a639b14c9313179816f6376aa0c5642a645ea344
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60455998"
---
# <a name="troubleshoot-an-attribute-not-synchronizing-in-azure-ad-connect"></a>Azure AD Connect'te eşitlemeyen bir özniteliği sorun giderme

## <a name="recommended-steps"></a>**Önerilen Adımlar**

Öznitelik eşitleme sorunlarını araştırmadan önce, **Azure AD Connect** eşitleme işlemini anlayalım:

  ![Azure AD Connect Senkronizasyon Süreci](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminoloji**

* **CS:** Bağlayıcı Alanı, veritabanında bir tablo.
* **MV:** Metaverse, veritabanında bir tablo.
* **AD:** Aktif Dizin
* **AAD:** Azure Etkin Dizin

### <a name="synchronization-steps"></a>**Eşitleme Adımları**

* AD'den alma: Etkin Dizin nesneleri AD CS'ye getirilir.

* AAD'den alma: Azure Active Directory nesneleri AAD CS'ye getirilir.

* Eşitleme: **Gelen Eşitleme Kuralları** ve **Giden Eşitleme Kuralları,** öncelik sayısının daha düşükten yükseğe doğru sırasına göre çalıştırılır. Eşitleme Kurallarını görüntülemek için masaüstü uygulamalarından **Eşitleme Kuralları** Düzenleyicisi'ne gidebilirsiniz. **Gelen Eşitleme Kuralları** CS'den MV'ye veri getirir. **Giden Eşitleme Kuralları** verileri MV'den CS'ye taşır.

* AD'ye dışa aktarma: Eşitleme çalıştırdıktan sonra nesneler AD CS'den **Active Directory'ye**dışa aktarılır.

* AAD'ye dışa aktarma: Eşitleme çalıştırdıktan sonra nesneler AAD CS'den **Azure Etkin Dizini'ne**dışa aktarılır.

### <a name="step-by-step-investigation"></a>**Adım Adım Araştırma**

* Araştırmamıza **Metaverse'den** başlayacağız ve kaynaktan hedefe öznitelik eşlenemelerine bakacağız.

* Masaüstü uygulamalarından, aşağıda gösterildiği gibi **Eşitleme Hizmet Yöneticisi'ni** başlatın:

  ![Başlatma Senkronizasyon Servis Yöneticisi](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/startmenu.png)

* **Eşitleme Hizmeti Yöneticisi'nde** **Metaverse Arama'yı**seçin, **Nesne Türüne Göre Kapsam'ı**seçin, öznitelik kullanarak nesneyi seçin ve **Arama** düğmesini tıklatın.

  ![Metaverse Arama](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvsearch.png)

* Tüm özniteliklerini görüntülemek için **Metaverse** aramasında bulunan nesneyi çift tıklatın. Tüm **Bağlayıcı Boşluklarında**karşılık gelen nesneye bakmak için **Bağlayıcılar** sekmesine tıklayabilirsiniz.

  ![Metaverse Nesne Konektörleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvattributes.png)

* **Bağlayıcı Alanı** özniteliklerini görüntülemek için **Active Directory Connector'a** çift tıklayın. **Önizleme** düğmesine tıklayın, aşağıdaki iletişim kutusunda **Önizleme Oluştur** düğmesine tıklayın.

  ![Bağlayıcı Alan Öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/csattributes.png)

* Şimdi Alma **Öznitelik Akışı**tıklayın , Bu **Metaverse** **Aktif Dizin Bağlayıcı Alanı** öznitelikleri akışını gösterir. **Eşitleme Kuralı** sütunu, bu özniteliğe hangi **Eşitleme Kuralının** katkıda bulunduğunu gösterir. **Veri Kaynağı** sütunu Bağlayıcı **Alanı'ndaki**öznitelikleri gösterir. **Metaverse Öznitelik** **sütunmetaverse**öznitelikleri gösterir. Burada eşitleme değil öznitelik için bakabilirsiniz. Özniteliği burada bulamazsanız, bu eşlenmez ve özniteliği eşlemek için yeni özel **Eşitleme Kuralı** oluşturmanız gerekir.

  ![Bağlayıcı Alan Öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/cstomvattributeflow.png)

* Giden Eşitleme **Kuralları'nı**kullanarak **Metaverse'den** **Active Directory Connector Space'e** öznitelik akışını görüntülemek için sol bölmedeki Dışa Aktarma **Özniteliği Akışını** tıklatın.

  ![Bağlayıcı Alan Öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvtocsattributeflow.png)

* Benzer şekilde, Azure **Active Directory Connector Space** nesnesini görüntüleyebilir ve **Metaverse'den** **Bağlayıcı Alanı'na** öznitelik akışını görüntülemek için **Önizleme'yi** oluşturabilirsiniz ve bunun tersi de bu şekilde bir özniteliğin neden eşitlenmediğini araştırabilirsiniz.

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect eşitleme: Teknik Kavramlar](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect eşitlemi: Mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect eşitlemi: Bildirimsel Sağlama'yı Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect eşitlemi: Bildirimsel Sağlama İfadelerini Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect eşitleme: Varsayılan yapılandırmayı anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect eşitleme: Kullanıcıları, Grupları ve Kişileri Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect eşitleme: Gölge öznitelikleri](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure AD Connect eşitleme.](how-to-connect-sync-whatis.md)
- [Melez kimlik nedir?](whatis-hybrid-identity.md)
