---
title: Hizmet kataloğu uygulamasını dağıtmak için Azure portalını kullanma
description: Yönetilen Uygulamalar tüketicilerine Azure portalı üzerinden bir hizmet kataloğu uygulamasını nasıl dağıtacaklarını gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 9a69296ddfc93fd7e8a6650df91876829631f5d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473073"
---
# <a name="quickstart-deploy-service-catalog-app-through-azure-portal"></a>Quickstart: Hizmet kataloğu uygulamasını Azure portalı üzerinden dağıtma

Önceki [quickstart'](publish-managed-app-definition-quickstart.md)ta yönetilen bir uygulama tanımı yayımladınız. Bu hızlı başlangıçta, bu tanımdan bir hizmet kataloğu uygulaması oluşturursunuz.

## <a name="create-service-catalog-app"></a>Hizmet kataloğu uygulaması oluşturma

Azure portalında aşağıdaki adımları kullanın:

1. **Kaynak oluştur**’u seçin.

   ![Kaynak oluşturma](./media/deploy-service-catalog-quickstart/create-new.png)

1. Hizmet **Kataloğu Yönetilen Uygulamayı** arayın ve kullanılabilir seçeneklerarasından seçin.

   ![Hizmet kataloğu uygulaması ara](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. Yönetilen Uygulama hizmetinin açıklamasını görürsünüz. **Oluştur'u**seçin.

   ![Oluştur’u seçin](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. Portal, erişiminiz olan yönetilen uygulama tanımlarını gösterir. Kullanılabilir tanımlardan, dağıtmak istediğinizi seçin. Bu hızlı başlatmada, önceki hızlı başlatmada oluşturduğunuz **Yönetilen Depolama Hesabı** tanımını kullanın. **Oluştur'u**seçin.

   ![Dağıtmak için tanımı seçin](./media/deploy-service-catalog-quickstart/select-definition.png)

1. **Temel Bilgiler** sekmesi için değerler sağlayın. Hizmet kataloğu uygulamanızı dağıtmak için Azure aboneliğini seçin. **applicationGroup**adlı yeni bir kaynak grubu oluşturun. Uygulamanız için bir konum seçin. Tamamlandığında **Tamam'ı**seçin.

   ![Temel değerler sağlama](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. Depolama hesabı adı için bir önek sağlayın. Oluşturmak için depolama hesabı türünü seçin. Tamamlandığında **Tamam'ı**seçin.

   ![Depolama için değerler sağlama](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. Özeti gözden geçirin. Doğrulama başarılı olduktan sonra, dağıtıma başlamak için **Tamam'ı** seçin.

   ![Özeti görüntüle](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>Sonuçları görüntüleme

Hizmet kataloğu uygulaması dağıtıldıktan sonra iki yeni kaynak grubunuz vardır. Bir kaynak grubu hizmet kataloğu uygulamasını tutar. Diğer kaynak grubu, hizmet kataloğu uygulaması için kaynakları tutar.

1. Hizmet kataloğu uygulamasını görmek için **applicationGroup** adlı kaynak grubunu görüntüleyin.

   ![Uygulamayı görüntüleme](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. Hizmet kataloğu uygulamasının kaynaklarını görmek için **applicationGroup{hash-characters}** adlı kaynak grubunu görüntüleyin.

   ![Kaynakları görüntüleme](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen bir uygulama için tanım dosyalarının nasıl oluşturulacağını öğrenmek için, yönetilen bir uygulama tanımlayıcı [yapımına ve yayınlayıcı](publish-service-catalog-app.md)
* Azure CLI için Azure [CLI ile hizmet kataloğu uygulamasını dağıt'a](./scripts/managed-application-cli-sample-create-application.md)bakın.
* PowerShell için [PowerShell ile servis kataloğu uygulamasını dağıt'a](./scripts/managed-application-poweshell-sample-create-application.md)bakın.
