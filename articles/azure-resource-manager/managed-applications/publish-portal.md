---
title: Yönetilen uygulamaları Portal üzerinden yayımlama
description: Kuruluşunuzun üyeleri için tasarlanan bir Azure yönetilen uygulaması oluşturmak için Azure portal nasıl kullanacağınızı gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 05302d92f2304be35a7b88fac6fabfc17b13c63e
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75651728"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Azure portal aracılığıyla bir hizmet kataloğu uygulaması yayımlama

Kuruluşunuzun üyeleri için tasarlanan [yönetilen uygulamaları](overview.md) yayımlamak için Azure Portal kullanabilirsiniz. Örneğin, BT departmanı kurumsal standartlara uyumu sağlamak için yönetilen uygulamalar yayımlayabilir. Bu yönetilen uygulamalara Azure marketten değil, hizmet kataloğu üzerinden erişilebilir.

## <a name="prerequisites"></a>Ön koşullar

Yönetilen bir uygulamayı yayımlarken, kaynakları yönetmek için bir kimlik belirtirsiniz. Bir Azure Active Directory Kullanıcı grubu belirtmenizi öneririz. Azure Active Directory bir Kullanıcı grubu oluşturmak için, bkz. [Grup oluşturma ve Azure Active Directory üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

Yönetilen uygulama tanımını içeren. zip dosyası bir URI üzerinden kullanılabilir olmalıdır. . Zip dosyanızı bir depolama blobuna yüklemenizi öneririz. 

## <a name="create-managed-application-with-portal"></a>Portal ile yönetilen uygulama oluşturma

1. Sol üst köşedeki **+ Yeni**' yi seçin.

   ![Yeni hizmet](./media/publish-portal/new.png)

1. **Hizmet kataloğunu**arayın.

1. Sonuçlarda, **Hizmet kataloğu yönetilen uygulama tanımını**bulana kadar ilerleyin. Onu seçin.

   ![Yönetilen uygulama tanımlarını arayın](./media/publish-portal/select-managed-apps-definition.png)

1. Yönetilen uygulama tanımını oluşturma işlemini başlatmak için **Oluştur** ' u seçin.

   ![Yönetilen uygulama tanımı oluştur](./media/publish-portal/create-definition.png)

1. Ad, görünen ad, açıklama, konum, abonelik ve kaynak grubu için değerler sağlayın. Paket dosyası URI 'SI için, oluşturduğunuz zip dosyasının yolunu belirtin.

   ![Değer sağla](./media/publish-portal/fill-application-values.png)

1. Kimlik doğrulama ve kilit düzeyi bölümüne geldiğinizde, **Yetkilendirme Ekle**' yi seçin.

   ![Yetkilendirme Ekle](./media/publish-portal/add-authorization.png)

1. Kaynakları yönetmek için bir Azure Active Directory grubu seçin ve **Tamam**' ı seçin.

   ![Yetkilendirme grubu Ekle](./media/publish-portal/add-auth-group.png)

1. Tüm değerleri sağladıysanız **Oluştur**' u seçin.

   ![Yönetilen uygulama oluşturma](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Yönetilen uygulama örnekleri için bkz. [Azure yönetilen uygulamalar Için örnek projeler](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.