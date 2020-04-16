---
title: Gizli değişikliklerin Key Vault durumu e-posta
description: Key Vault sırları değişikliklerine yanıt vermek için Mantık Uygulamaları kullanma kılavuzu
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 11/11/2019
ms.author: mbaldwin
ms.openlocfilehash: 53e8586486d9a9ebf870de350d5607f58977c0f5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423281"
---
# <a name="use-logic-apps-to-receive-email-about-status-changes-of-key-vault-secrets"></a>Önemli kasa sırlarının durum değişiklikleri hakkında e-posta almak için Mantık Uygulamalarını kullanın

Bu kılavuzda [Azure Mantıksal Uygulamaları](../../logic-apps/index.yml)kullanarak Azure Olay [Kılavuzu](../../event-grid/index.yml) üzerinden alınan Azure Anahtar Kasası etkinliklerine nasıl yanıt vereceğinizi öğreneceksiniz. Sonunda, Azure Anahtar Kasası'nda her sır oluşturulduğunda bir bildirim e-postası göndermek üzere ayarlanmış bir Azure mantık uygulamasına sahip olursunuz.

Azure Key Vault / Azure Olay Ağı tümleştirmesi hakkında genel bilgi [için](event-grid-overview.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

- Azure Logic Apps (Office 365 Outlook gibi) tarafından desteklenen herhangi bir e-posta sağlayıcısından gelen bir e-posta hesabı. Bu e-posta hesabı olay bildirimlerini göndermek için kullanılır. Desteklenen Logic App bağlayıcılarının tam listesi için bkz. [Bağlayıcılara genel bakış](/connectors)
- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
- Azure Aboneliğinizde önemli bir kasa. Set'teki adımları izleyerek hızlı bir şekilde yeni bir anahtar kasası oluşturabilir [ve Azure CLI'yi kullanarak Azure Key Vault'tan bir sır alabilirsiniz.](../secrets/quick-create-cli.md)

## <a name="create-a-logic-app-via-event-grid"></a>Olay Izgarası üzerinden Bir Mantık Uygulaması Oluşturma

İlk olarak, olay ızgara işleyicisi ile Mantık Uygulaması oluşturun ve Azure Key Vault "SecretNewVersionCreated" etkinliklerine abone olun.

Azure Olay Ağı aboneliği oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında, anahtar kasanıza gidin, **Etkinlikler > Başlayın'ı** seçin ve **Mantık Uygulamaları'nı** tıklatın

    
    ![Key Vault - etkinlikler sayfası](../media/eventgrid-logicapps-kvsubs.png)

1. **Logic Apps Designer'da** bağlantıyı doğrulayın ve **Devam et'i** tıklatın 
 
    ![Mantık App Designer - bağlantı](../media/eventgrid-logicappdesigner1.png)

1. Kaynak **olayı oluştuğunda** aşağıdakileri yapın:
    - **Abonelik** ve **Kaynak Adını** varsayılan olarak bırakın.
    - **Kaynak Türü**için **Microsoft.KeyVault.vaults'u** seçin.
    - Etkinlik Türü Öğesi için **Microsoft.KeyVault.SecretNewVersionCreated** **'ı**seçin - 1 .

    ![Mantık App Designer - olay işleyicisi](../media/eventgrid-logicappdesigner2.png)

1. Select **+ Yeni Adım** Bu bir eylem seçmek için bir pencere açar.
1. **E-posta**'yı arayın. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğreticide **Office 365 Outlook** kullanılır. Diğer e-posta sağlayıcılarının adımları da bunlara benzer.
1. **E-posta Gönder (V2)** eylemini seçin.

   ![Mantık App Designer - e-posta eklemek](../media/eventgrid-logicappdesigner3.png)

1. E-posta şablonunuzu oluşturun:
    - **Bunun için:** Bildirim e-postalarını almak için e-posta adresini girin. Bu öğreticide, test etmek için erişebileceğiniz bir e-posta hesabı kullanın.
    - **Konu** ve **Gövde**: E-postanızın metnini yazın. Olay verileri temelinde dinamik içerik eklemek için seçici aracından JSON özelliklerini seçin. Olayın verilerini kullanarak `@{triggerBody()?['Data']}`alabilirsiniz.

    E-posta şablonunuzun görünümü bu örnek gibi görünebilir.

    ![Mantık App Designer - e-posta eklemek](../media/eventgrid-logicappdesigner4.png)

8. **Olarak Kaydet'i**tıklatın.
9. Yeni mantık uygulaması için bir **ad** girin ve **Oluştur'a**tıklayın.
    
    ![Mantık App Designer - e-posta eklemek](../media/eventgrid-logicappdesigner5.png)

## <a name="test-and-verify"></a>Test edin ve doğrulayın

1.  Azure portalındaki anahtar kasanıza gidin ve **Etkinlik Abonelikleri > Etkinlikler'i**seçin.  Yeni bir aboneliğin oluşturulduğunu doğrulama
    
    ![Mantık App Designer - e-posta eklemek](../media/eventgrid-logicapps-kvnewsubs.png)

1.  Anahtar kasanıza gidin, **Sırlar'ı**seçin ve **+ Oluştur/İçe'yi**seçin. Test amacıyla yeni bir sır oluşturun ve kalan parametreleri varsayılan ayarlarında tutun.

    ![Anahtar Vault - Gizli oluşturun](../media/eventgrid-logicapps-kv-create-secret.png)

1. Gizli bir ekranda herhangi bir ad, herhangi bir değer sağlamak ve **oluştur'u** **seçin.**

Gizli oluşturulduğunda, yapılandırılan adreslerden bir e-posta alınır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel Bakış: [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](event-grid-overview.md)
- Nasıl yapilir: [Anahtar kasabildirimlerini Azure Otomasyonu'na yönlendirin.](event-grid-tutorial.md)
- [Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)](../../event-grid/event-schema-key-vault.md)
- [Azure Event Grid](../../event-grid/index.yml) hakkında daha fazla bilgi edinin.
- [Azure App Service’in Logic Apps özelliği](../../logic-apps/index.yml) hakkında daha fazla bilgi edinin.
