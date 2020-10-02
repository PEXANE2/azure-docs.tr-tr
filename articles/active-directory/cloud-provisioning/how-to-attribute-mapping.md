---
title: Azure AD Connect bulut sağlama özniteliği Düzenleyicisi
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
ms.openlocfilehash: 58b7aa0bf8c82990a00b4e41041145a67ee2f02b
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91637366"
---
# <a name="azure-ad-connect-cloud-provisioning-attibute-mapping"></a>Azure AD Connect bulut sağlama parentid kimliğiyle eşleme

Azure AD Connect bulut sağlaması, şirket içi Kullanıcı/Grup nesneleriniz ile Azure AD içindeki nesneler arasındaki öznitelikleri kolayca eşlemenizi sağlayacak yeni bir özellik getirmiştir.  Bu özellik, bulut sağlama yapılandırmasına eklenmiştir.

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

 1.  Azure portal **Azure Active Directory**' ni seçin.
 2.  **Azure AD Connect**seçin.
 3.  **Sağlamayı Yönet**' i seçin.

   ![Sağlamayı Yönet](media/how-to-configure/manage1.png)
 
 4. **Yapılandırma**altında yapılandırmanızı seçin.
 5. **Eşlemeleri düzenlemek Için tıklayın ' ı**seçin.  Bu işlem öznitelik eşleme ekranını açar.

 ![Öznitelik ekleme](media/how-to-attribute-mapping/mapping6.png)
 6.  **Öznitelik Ekle**' ye tıklayın.

 ![Eşleme türü](media/how-to-attribute-mapping/mapping1.png)
 
 7. **Eşleme türünü**seçin.  Bu örnekte Ifadesi kullanıyoruz.
 8.  Kutuya ifadeyi girin.  Bu örnekte şunu kullanıyoruz: `Replace([mail], "@contoso.com", , ,"", ,).`
 9.  Target özniteliğini girin.  Bu örnekte ExtensionAttribute15 kullanacağız.
 10. Bunun ne zaman uygulanacağını seçin ve ardından **Uygula** ' ya tıklayın.
   
   ![Eşlemeleri Düzenle](media/how-to-attribute-mapping/mapping2a.png)
 11. Öznitelik eşleme ekranına geri döndüğünüzde yeni öznitelik eşlemenizi görmeniz gerekir.  
 12. **Şemayı kaydet**' e tıklayın.

 ![Şemayı Kaydet](media/how-to-attribute-mapping/mapping3.png)

## <a name="test-your-attribute-mapping"></a>Öznitelik eşlemenizi test etme
Öznitelik eşlemenizi test etmek için [isteğe bağlı sağlama](how-to-on-demand-provision.md)seçeneğini kullanabilirsiniz.  Öğesinden 

1.  Azure portal **Azure Active Directory**' ni seçin.
2.  **Azure AD Connect**seçin.
3.  **Sağlamayı Yönet**' i seçin.
4. **Yapılandırma**altında yapılandırmanızı seçin.
5. **Doğrula** altında **Kullanıcı sağla** düğmesine tıklayın. 
6. İsteğe bağlı sağlama ekranında.  Bir kullanıcının veya grubun **ayırt edici adını** girin ve **sağla** düğmesine tıklayın.  
7. Tamamlandıktan sonra, başarılı bir ekran ve başarıyla sağlandığını belirten 4 yeşil onay kutusu görmeniz gerekir.  
  ![Sağlama başarısı](media/how-to-attribute-mapping/mapping4.png)
1. **Eylem gerçekleştir** altında **Ayrıntıları görüntüle**' ye tıklayın.  Sağ tarafta, syncrhon, ve Expression uygulanmış yeni özniteliğini görmeniz gerekir.

  ![Eylem gerçekleştir](media/how-to-attribute-mapping/mapping5.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
- [Öznitelik eşlemeleri için Ifadeler yazma](reference-expressions.md)
- [Eşitlenen öznitelikler](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)