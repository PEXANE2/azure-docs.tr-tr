---
title: Hizmet kataloğu uygulamasını dağıtmak için Azure portal kullanma
description: Yönetilen uygulamaların tüketicilerini, Azure portal aracılığıyla bir hizmet kataloğu uygulamasının nasıl dağıtılacağını gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: ce58fc69496f54c078b0a0a55a8a3c7cad82a051
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81391702"
---
# <a name="quickstart-deploy-service-catalog-app-through-azure-portal"></a>Hızlı başlangıç: Azure portal aracılığıyla Hizmet kataloğu uygulaması dağıtma

[Önceki hızlı](publish-service-catalog-app.md)başlangıçta bir yönetilen uygulama tanımı yayımladınız. Bu hızlı başlangıçta, bu tanımdan bir hizmet kataloğu uygulaması oluşturacaksınız.

## <a name="create-service-catalog-app"></a>Hizmet kataloğu uygulaması oluşturma

Azure portal, aşağıdaki adımları kullanın:

1. **Kaynak oluştur**’u seçin.

   ![Kaynak oluşturma](./media/deploy-service-catalog-quickstart/create-new.png)

1. **Hizmet kataloğu yönetilen uygulamasını** arayın ve kullanılabilir seçeneklerden seçin.

   ![Hizmet kataloğu uygulamasını ara](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. Yönetilen uygulama hizmetinin bir açıklamasını görürsünüz. **Oluştur**’u seçin.

   ![Oluştur’u seçin](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. Portal, erişiminiz olan yönetilen uygulama tanımlarını gösterir. Kullanılabilir tanımlardan, dağıtmak istediğiniz birini seçin. Bu hızlı başlangıçta, önceki hızlı başlangıçta oluşturduğunuz **yönetilen depolama hesabı** tanımını kullanın. **Oluştur**’u seçin.

   ![Dağıtılacak tanımı seçin](./media/deploy-service-catalog-quickstart/select-definition.png)

1. **Temel bilgiler** sekmesi için değerler sağlayın. Hizmet kataloğu uygulamanızı dağıtmak için Azure aboneliğini seçin. **Applicationgroup**adlı yeni bir kaynak grubu oluşturun. Uygulamanız için bir konum seçin. İşiniz bittiğinde **Tamam**' ı seçin.

   ![Temel için değer sağlama](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. Depolama hesabı adı için bir ön ek belirtin. Oluşturulacak depolama hesabı türünü seçin. İşiniz bittiğinde **Tamam**' ı seçin.

   ![Depolama için değerler sağlama](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. Özeti gözden geçirin. Doğrulama başarılı olduktan sonra, dağıtıma başlamak için **Tamam** ' ı seçin.

   ![Özeti görüntüle](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>Sonuçları görüntüleme

Hizmet kataloğu uygulaması dağıtıldıktan sonra, iki yeni kaynak grubunuz vardır. Hizmet kataloğu uygulamasını içeren bir kaynak grubu. Diğer kaynak grubu, hizmet kataloğu uygulamasının kaynaklarını tutar.

1. Hizmet kataloğu uygulamasını görmek için **applicationgroup** adlı kaynak grubunu görüntüleyin.

   ![Uygulamayı görüntüleme](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. Hizmet kataloğu uygulamasının kaynaklarını görmek için **applicationgroup {Hash-Characters}** adlı kaynak grubunu görüntüleyin.

   ![Kaynakları görüntüleme](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen bir uygulama için tanım dosyalarını oluşturma hakkında bilgi edinmek için bkz. [yönetilen uygulama tanımı oluşturma ve yayımlama](publish-service-catalog-app.md).
* Azure CLı için bkz. [Azure CLI ile hizmet kataloğu uygulaması dağıtma](./scripts/managed-application-cli-sample-create-application.md).
* PowerShell için bkz. [PowerShell ile hizmet kataloğu uygulaması dağıtma](./scripts/managed-application-poweshell-sample-create-application.md).
