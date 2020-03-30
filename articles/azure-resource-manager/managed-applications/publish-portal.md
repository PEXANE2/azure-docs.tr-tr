---
title: Yönetilen uygulamaları portal üzerinden yayımlama
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulama oluşturmak için Azure portalını nasıl kullanacağınızı gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 05302d92f2304be35a7b88fac6fabfc17b13c63e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651728"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Azure portalı üzerinden hizmet kataloğu uygulaması yayınlama

Azure portalını, kuruluşunuzun üyelerine yönelik [yönetilen uygulamaları](overview.md) yayımlamak için kullanabilirsiniz. Örneğin, BT departmanı kurumsal standartlara uyumu sağlamak için yönetilen uygulamalar yayımlayabilir. Bu yönetilen uygulamalara Azure marketten değil, hizmet kataloğu üzerinden erişilebilir.

## <a name="prerequisites"></a>Ön koşullar

Yönetilen bir uygulama yayımlarken, kaynakları yönetmek için bir kimlik belirtirsiniz. Bir Azure Active Directory kullanıcı grubu belirtmenizi öneririz. Bir Azure Etkin Dizin kullanıcı grubu oluşturmak için [bkz.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) 

Yönetilen uygulama tanımını içeren .zip dosyası bir URI aracılığıyla kullanılabilir olmalıdır. .zip dosyanızı bir depolama blob'una yüklemenizi öneririz. 

## <a name="create-managed-application-with-portal"></a>Portal ile yönetilen uygulama oluşturma

1. Sol üst köşede **+ Yeni'yi**seçin.

   ![Yeni hizmet](./media/publish-portal/new.png)

1. Hizmet **kataloğunu**arayın.

1. Sonuçlarda, Hizmet Kataloğu Yönetilen Uygulama Tanımı'nı bulana kadar **kaydırın.** Kalem simgesini seçin.

   ![Yönetilen uygulama tanımlarını arama](./media/publish-portal/select-managed-apps-definition.png)

1. Yönetilen uygulama tanımıoluşturma işlemini başlatmak için **Oluştur'u** seçin.

   ![Yönetilen uygulama tanımı oluşturma](./media/publish-portal/create-definition.png)

1. Ad, görüntü adı, açıklama, konum, abonelik ve kaynak grubu için değerler sağlayın. Paket dosyası URI için, oluşturduğunuz zip dosyasına giden yolu sağlayın.

   ![Değerler sağlama](./media/publish-portal/fill-application-values.png)

1. Kimlik Doğrulama ve Kilit Düzeyi bölümüne ulaştığınızda, **Yetki Ekle'yi**seçin.

   ![Yetkilendirme ekleme](./media/publish-portal/add-authorization.png)

1. Kaynakları yönetmek için bir Azure Etkin Dizin grubu seçin ve **Tamam'ı**seçin.

   ![Yetkilendirme grubu ekleme](./media/publish-portal/add-auth-group.png)

1. Tüm değerleri sağladığınızda **Oluştur'u**seçin.

   ![Yönetilen uygulama oluşturma](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Yönetilen uygulama örnekleri [için, Azure yönetilen uygulamalar için Örnek projelere](sample-projects.md)bakın.
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.