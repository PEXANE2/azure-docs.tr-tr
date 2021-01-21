---
title: Azure AD Connect bulut eşitleme özniteliği Düzenleyicisi
description: Bu makalede, öznitelik düzenleyicisinin nasıl kullanılacağı açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/22/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 80a035f30294449a024bbde76df2d42ddc23396e
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98622720"
---
# <a name="azure-ad-connect-cloud-sync-attribute-mapping"></a>Azure AD Connect bulut eşitleme öznitelik eşlemesi

Azure AD Connect bulut eşitlemesi, şirket içi Kullanıcı/Grup nesneleriniz ile Azure AD içindeki nesneler arasındaki öznitelikleri kolayca eşlemenizi sağlayacak yeni bir özellik getirmiştir.  Bu özellik, bulut eşitleme yapılandırmasına eklenmiştir.

Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, var olan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz.  Eşitlenen özniteliklerin bir listesi için bkz. [eşitlenen öznitelikler](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understanding-attribute-mapping-types"></a>Öznitelik eşleme türlerini anlama
Öznitelik eşlemeleriyle, özniteliklerin Azure AD 'de nasıl doldurulduğunu kontrol edersiniz.
Dört farklı eşleme türü desteklenir:

- **Doğrudan** – Target ÖZNITELIĞI, ad 'deki bağlantılı nesnenin bir özniteliğinin değeri ile doldurulur.
- **Sabit** – Target özniteliği belirttiğiniz belirli bir dizeyle doldurulur.
- **İfade** -Target özniteliği, komut dosyası benzeri bir ifadenin sonucuna göre doldurulur.
  Daha fazla bilgi için bkz. [öznitelik eşlemeleri Için Ifadeler yazma](reference-expressions.md).
- **Hiçbiri** -hedef öznitelik değiştirilmemiş olarak bırakıldı. Ancak, hedef özniteliği boşsa, belirttiğiniz varsayılan değerle doldurulur.

Bu dört temel tür ile birlikte özel öznitelik eşlemeleri, isteğe bağlı **varsayılan** değer atama kavramını destekler. Varsayılan değer atama, Azure AD 'de veya hedef nesnede bir değer olmadığında bir hedef özniteliğin bir değer ile doldurulmasını sağlar. En yaygın yapılandırma bu boş bırakılmamalıdır.

## <a name="understanding-attribute-mapping-properties"></a>Öznitelik eşleme özelliklerini anlama

Önceki bölümde, öznitelik eşleme türü özelliğine zaten sunuldu.
Bu özellik ile birlikte, öznitelik eşlemeleri de aşağıdaki öznitelikleri destekler:

- **Source özniteliği** -kaynak sistemden Kullanıcı özniteliği (örnek: Active Directory).
- **Target özniteliği** – hedef sistemdeki kullanıcı özniteliği (örnek: Azure Active Directory).
- **Null Ise varsayılan değer (isteğe bağlı)** -kaynak özniteliği null ise hedef sisteme geçirilecek değer. Bu değer, yalnızca bir kullanıcı oluşturulduğunda sağlanacak. Mevcut bir Kullanıcı güncelleştirilirken "null olduğunda varsayılan değer" sağlanmayacak.  
- **Bu eşlemeyi Uygula**
  - **Her zaman** : Bu eşlemeyi hem Kullanıcı oluşturma hem de güncelleştirme eylemlerinde uygulayın.
  - **Yalnızca oluşturma sırasında** -bu eşlemeyi yalnızca Kullanıcı oluşturma eylemlerinde uygulayın.

> [!NOTE]
> Bu belgede, Azure portal öznitelikleri eşlemek için nasıl kullanılacağı açıklanmaktadır.  Graph kullanma hakkında daha fazla bilgi için bkz. [dönüşümler](how-to-transformation.md)

## <a name="using-attribute-mapping"></a>Öznitelik eşlemesini kullanma

Yeni özelliği kullanmak için aşağıdaki adımları izleyin.

1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
2.  **Azure AD Connect** seçin.
3.  **Bulut eşitlemesini Yönet**' i seçin.

    ![Sağlamayı Yönet](media/how-to-install/install-6.png)

4. **Yapılandırma** altında yapılandırmanızı seçin.
5. **Eşlemeleri düzenlemek Için tıklayın ' ı** seçin.  Bu işlem öznitelik eşleme ekranını açar.

    ![Öznitelik ekleme](media/how-to-attribute-mapping/mapping-6.png)

6.  **Öznitelik Ekle**' ye tıklayın.

    ![Eşleme türü](media/how-to-attribute-mapping/mapping-1.png)

7. **Eşleme türünü** seçin.  Bu örnekte Ifadesi kullanıyoruz.
8.  Kutuya ifadeyi girin.  Bu örnekte şunu kullanıyoruz: `Replace([mail], "@contoso.com", , ,"", ,).`
9.  Target özniteliğini girin.  Bu örnekte ExtensionAttribute15 kullanacağız.
10. Bunun ne zaman uygulanacağını seçin ve ardından **Uygula** ' ya tıklayın.

    ![Eşlemeleri Düzenle](media/how-to-attribute-mapping/mapping-2a.png)

11. Öznitelik eşleme ekranına geri döndüğünüzde yeni öznitelik eşlemenizi görmeniz gerekir.  
12. **Şemayı kaydet**' e tıklayın.

    ![Şemayı Kaydet](media/how-to-attribute-mapping/mapping-3.png)

## <a name="test-your-attribute-mapping"></a>Öznitelik eşlemenizi test etme

Öznitelik eşlemenizi test etmek için [isteğe bağlı sağlama](how-to-on-demand-provision.md)seçeneğini kullanabilirsiniz.  Öğesinden 

1. Azure portalında **Azure Active Directory** seçeneğini belirleyin.
2. **Azure AD Connect** seçin.
3. **Sağlamayı Yönet**' i seçin.
4. **Yapılandırma** altında yapılandırmanızı seçin.
5. **Doğrula** altında **Kullanıcı sağla** düğmesine tıklayın. 
6. İsteğe bağlı sağlama ekranında.  Bir kullanıcının veya grubun **ayırt edici adını** girin ve **sağla** düğmesine tıklayın.  
7. Tamamlandıktan sonra, başarılı bir ekran ve başarıyla sağlandığını belirten 4 yeşil onay kutusu görmeniz gerekir.  

    ![Sağlama başarısı](media/how-to-attribute-mapping/mapping-4.png)

8. **Eylem gerçekleştir** altında **Ayrıntıları görüntüle**' ye tıklayın.  Sağ tarafta, yeni özniteliği eşitlenmiş ve ifade uygulanmış olarak görmeniz gerekir.

  ![Eylem gerçekleştir](media/how-to-attribute-mapping/mapping-5.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure AD Connect bulut eşitlemesi nedir?](what-is-cloud-sync.md)
- [Öznitelik eşlemeleri için Ifadeler yazma](reference-expressions.md)
- [Eşitlenen öznitelikler](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)
