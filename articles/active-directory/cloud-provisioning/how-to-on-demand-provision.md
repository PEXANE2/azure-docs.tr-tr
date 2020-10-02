---
title: Azure AD Connect bulut sağlama isteğe bağlı sağlama
description: Bu makalede, isteğe bağlı sağlama özelliği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72623d69f9fb0cbc68758a362bb977e5e02d2c00
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91637306"
---
# <a name="azure-ad-connect-cloud-provisioning-on-demand-provisioning"></a>Azure AD Connect bulut sağlama isteğe bağlı sağlama

Azure AD Connect bulut sağlama, bu değişiklikleri tek bir kullanıcıya uygulayarak yapılandırma değişikliklerini sınamanızı sağlayacak yeni bir özellik getirmiştir.  Yapılandırma üzerinde yapılan değişikliklerin düzgün şekilde uygulandığını ve Azure AD ile doğru eşitlendiğini doğrulamak için bunu kullanabilirsiniz.  

> [!IMPORTANT] 
> İsteğe bağlı sağlama kullandığınızda, kapsam filtreleri seçtiğiniz kullanıcıya uygulanmaz.  Bu, belirttiğiniz OU 'Lar dışındaki kullanıcılar üzerinde isteğe bağlı sağlama kullanabileceğiniz anlamına gelir.


## <a name="using-on-demand-provisioning"></a>İsteğe bağlı sağlama kullanma
Yeni özelliği kullanmak için aşağıdaki adımları izleyin.


1.  Azure portal **Azure Active Directory**' ni seçin.
2.  **Azure AD Connect**seçin.
3.  **Sağlamayı Yönet**' i seçin.

    ![Sağlamayı Yönet](media/how-to-configure/manage1.png)
4. **Yapılandırma**altında yapılandırmanızı seçin.
5. **Doğrula** altında **Kullanıcı sağla** düğmesine tıklayın. 

 ![Kullanıcı sağlama](media/how-to-on-demand-provision/on-demand2.png)

6. İsteğe bağlı sağlama ekranında.  Kullanıcının **ayırt edici adını** girin ve **sağlama** düğmesine tıklayın.  
 
 ![İsteğe bağlı sağlama](media/how-to-on-demand-provision/on-demand3.png)
7. Tamamlandıktan sonra, başarılı bir ekran ve başarıyla sağlandığını belirten 4 yeşil onay kutusu görmeniz gerekir.  Herhangi bir hata, sol tarafta görüntülenir.

  ![Başarılı](media/how-to-on-demand-provision/on-demand4.png)

Artık kullanıcıya bakabilir ve yapılandırmada yaptığınız değişikliklerin uygulanmış olup olmadığını belirleyebilirsiniz.  Bu belgenin geri kalanında, başarıyla eşitlenen bir kullanıcının ayrıntılarında görüntülenen tek tek bölümler açıklanır.

## <a name="import-user-details"></a>Kullanıcı ayrıntılarını içeri aktar
Bu bölüm, Active Directory içeri aktarılan Kullanıcı hakkında bilgiler sağlar.  Kullanıcı Azure AD 'ye sağlanmadan önce bu şekilde görünür.  Bu bilgileri görüntülemek için **Ayrıntıları görüntüle** bağlantısına tıklayın.

![Kullanıcı al](media/how-to-on-demand-provision/on-demand5.png)

Bu bilgileri kullanarak, çeşitli öznitelikleri ve içeri aktarılan değerlerini görebilirsiniz.  Özel bir öznitelik eşlemesi oluşturduysanız, bu değeri burada görebilirsiniz.
![Kullanıcı ayrıntılarını içeri aktar](media/how-to-on-demand-provision/on-demand6.png)

## <a name="determine-if-user-is-in-scope-details"></a>Kullanıcının kapsam ayrıntılarında olup olmadığını belirleme
Bu bölümde, Azure AD 'ye içeri aktarılan kullanıcının kapsamda olup olmadığı hakkında bilgi sağlanır.  Bu bilgileri görüntülemek için **Ayrıntıları görüntüle** bağlantısına tıklayın.

![Kullanıcı kapsamı](media/how-to-on-demand-provision/on-demand7.png)

Bu bilgileri kullanarak kullanıcılarınızın kapsamıyla ilgili ek bilgileri görebilirsiniz.

![Kullanıcı kapsamı ayrıntıları](media/how-to-on-demand-provision/on-demand10a.png)

## <a name="match-user-between-source-and-target-system-details"></a>Kaynak ve hedef sistem ayrıntıları arasında kullanıcıyı Eşleştir
Bu bölümde, kullanıcının Azure AD 'de zaten var olup olmadığı ve yeni bir kullanıcı sağlamak yerine bir JOIN oluşması gerektiği hakkında bilgi verilmektedir.  Bu bilgileri görüntülemek için **Ayrıntıları görüntüle** bağlantısına tıklayın.
![Ayrıntıları görüntüle](media/how-to-on-demand-provision/on-demand8.png)

Bu bilgileri kullanarak, bir eşleşmenin bulunup bulunamamadığını veya yeni bir Kullanıcı oluşturulup oluşturulmayacağını görebilirsiniz.

![Kullanıcı bilgileri](media/how-to-on-demand-provision/on-demand11.png)

Eşleşen ayrıntılar, aşağıdaki üç işlemden birine sahip bir ileti gösterir.  Bunlar:
- Oluşturma-Azure AD 'de bir Kullanıcı oluşturulur
- Güncelleştirme-bir Kullanıcı, yapılandırmada yapılan bir değişikliğe göre güncelleştirilir
- Sil-bir Kullanıcı Azure AD 'den kaldırılır.

Gerçekleştirdiğiniz işlem türüne bağlı olarak, ileti farklılık gösterecektir.

## <a name="perform-action-details"></a>Eylem ayrıntılarını gerçekleştir
Bu bölümde, yapılandırma uygulandıktan sonra sağlanan veya Azure AD 'ye aktarılmış Kullanıcı hakkında bilgi sağlanır.  Kullanıcı Azure AD 'ye sağlandıktan sonra bu şekilde görünür.  Bu bilgileri görüntülemek için **Ayrıntıları görüntüle** bağlantısına tıklayın.
![Eylem ayrıntılarını gerçekleştir](media/how-to-on-demand-provision/on-demand9.png)

Bu bilgileri kullanarak, yapılandırma uygulandıktan sonra özniteliklerin değerlerini görebilirsiniz.  İçeri aktarılmakta olan veya farklı olan özellikler ile aynı şekilde görünüyor mu?  Yapılandırma başarılı mı?  

Bu işlem, bulut ve Azure AD kiracınıza geçiş yaparken öznitelik dönüşümünü izlemenizi sağlar.

![Trace özniteliği](media/how-to-on-demand-provision/on-demand12.png)

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
- [Azure AD Connect bulut sağlama nasıl yüklenir](how-to-install.md)
 