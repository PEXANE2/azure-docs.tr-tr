---
title: Azure Bildirim Hub'larında anında iletme bildirimleri ayarlama | Microsoft Dokümanlar
description: Platform bildirim sistemi (PNS) ayarlarını kullanarak Azure portalında Azure Bildirim Hub'larını nasıl ayarlayabilirsiniz öğrenin.
services: notification-hubs
author: sethmanheim
manager: femila
editor: dbradish-microsoft
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 78afb124ee1d1ab9b212197fb7a7140f88de9940
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349523"
---
# <a name="quickstart-set-up-push-notifications-in-a-notification-hub"></a>Hızlı başlatma: Anında iletme bildirimlerini bir bildirim hub'ında ayarlama

Azure Bildirim Hub'ları, kullanımı kolay ve ölçeklendirilebilen bir itme motoru sağlar. Bildirim Hub'larını kullanarak herhangi bir platforma (iOS, Android, Windows, Baidu) ve herhangi bir arka uçtan (bulut veya şirket içi) bildirim gönderin. Daha fazla bilgi için Azure [Bildirim Hub'ları nedir?](notification-hubs-push-notification-overview.md)

Bu hızlı başlatmada, bildirim hub'larında birden çok platformda anında iletme bildirimleri ayarlamak için platform bildirim sistemi (PNS) ayarlarını kullanırsınız. Hızlı başlangıç, Azure portalında atmanız gereken adımları gösterir.  [Google Firebase Bulut Mesajlaşması,](/azure/notification-hubs/configure-notification-hub-portal-pns-settings?tabs=azure-cli#google-firebase-cloud-messaging-fcm) Azure CLI'yi kullanma yönergelerini içerir.

Daha önce bir bildirim hub'ı oluşturmadıysanız, şimdi bir bildirim hub'ı oluşturun. Daha fazla bilgi için Azure [portalında Bir Azure bildirim merkezi oluşturma](create-notification-hub-portal.md) veya Azure [CLI'yi kullanarak bir Azure bildirim merkezi oluşturma](create-notification-hub-azure-cli.md)

## <a name="apple-push-notification-service"></a>Apple Anında İletilen Bildirim Servisi

Apple Anında İtme Bildirim Hizmeti 'ni (APNS) ayarlamak için:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüden **Apple (APNS)** seçeneğini belirleyin.

1. **Kimlik Doğrulama Modu**için **Sertifika** veya **Belirteç'i**seçin.

   a. **Sertifika'yı**seçerseniz:
   * Dosya simgesini seçin ve sonra yüklemek istediğiniz *.p12* dosyasını seçin.
   * Parola girin.
   * **Korumalı alan** modunu seçin. Veya uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modunu seçin.

     ![Azure portalında apns sertifikası yapılandırmasının ekran görüntüsü](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. **Token'ı**seçerseniz:

   * **Anahtar Kimliği,** Paket **Kimliği,** **Takım Kimliği**ve **Belirteç**değerlerini girin.
   * **Korumalı alan** modunu seçin. Veya uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modunu seçin.

     ![Azure portalında apns belirteç yapılandırmasının ekran görüntüsü](./media/configure-notification-hub-portal-pns-settings/notification-hubs-apple-config-token.png)

Daha fazla bilgi için [Azure Bildirim Hub'larını kullanarak iOS'a anında iletme bildirimleri'ne](notification-hubs-ios-apple-push-notification-apns-get-started.md)bakın.

## <a name="google-firebase-cloud-messaging-fcm"></a>Google Firebase Bulut Mesajlaşma (FCM)

# <a name="portal"></a>[Portal](#tab/azure-portal)

Google FCM için anında iletme bildirimleri ayarlamak için:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüden **Google 'ı (GCM/FCM)** seçin.
2. Daha önce kaydettiğiniz Google FCM projesi için **API Anahtarını** yapıştırın.
3. **Kaydet'i**seçin.

   ![Google FCM için Bildirim Hub'larının nasıl yapılandırılabildiğini gösteren ekran görüntüsü](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

Bu adımları tamamladığınızda, bir uyarı bildirim merkezinin başarıyla güncelleştirildiğini gösterir. **Kaydet** düğmesi devre dışı bırakılır.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri almalısınız:

* [Azure CLI](/cli/azure/install-azure-cli) sürümü 2.0.67 veya sonrası.

* [Bildirim Hub'ları için](/cli/azure/ext/notification-hub/notification-hub)Azure CLI uzantısı.
* Bir Google Firebase Bulut Mesajlaşma (FCM) projesi için **API Anahtarı.**

### <a name="set-up-push-notifications-for-google-fcm"></a>Google FCM için anında iletme bildirimleri ayarlama

1. Google API Anahtarınızı bildirim merkezinize eklemek için [az bildirim hub'ı kimlik bilgisi gcm güncelleme](/cli/azure/ext/notification-hub/notification-hub/credential/gcm#ext-notification-hub-az-notification-hub-credential-gcm-update) komutunu kullanın.

   ```azurecli
   az notification-hub credential gcm update --resource-group spnhubrg --namespace-name spnhubns    --notification-hub-name spfcmtutorial1nhub --google-api-key myKey
   ```

2. Android Uygulaması bildirim hub'ına bağlanmak için bir bağlantı dizesine ihtiyaç duyar.  Kullanılabilir erişim ilkelerini listelemek için [az bildirim merkezi yetkilendirme kuralı komutunu](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) kullanın.  Erişim ilkesi bağlantı dizelerini almak için [az bildirim-hub yetkilendirme-kural listesi anahtarları](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu kullanın.  Birincil bağlantı dizesini doğrudan almak `--query` için parametredeki **birincilConnectionString** veya **secondaryConnectionString'i** belirtin.

   ```azurecli
   #list access policies for a notification hub
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table

   #list keys and connection strings for a notification hub access policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name myAccessPolicyName --output json

   #get the primaryConnectionString for an access policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name myAccessPolicyName --query primaryConnectionString
   ```

3. Android Uygulamasına mesaj göndermeyi test etmek için [az bildirim hub test-gönder](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-test-send) komutunu kullanın.

   ```azurecli
   #test with message body
   az notification-hub test-send --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --notification-format gcm --message "my message body"

   #test with JSON string
   az notification-hub test-send --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --notification-format gcm --payload "{\"data\":{\"message\":\"my JSON string\"}}"
   ```

[Az bildirim merkezi kimlik bilgisi](/cli/azure/ext/notification-hub/notification-hub/credential) komutuyla diğer platformlar için Azure CLI başvuruları alın.

---

Bir Android uygulamasına bildirimleri zorlama hakkında daha fazla bilgi için [Firebase'i kullanarak Android cihazlara anında iletme bildirimleri gönder'e](notification-hubs-android-push-notification-google-fcm-get-started.md)bakın.

## <a name="windows-push-notification-service"></a>Windows Push Bildirim Hizmeti

Windows Push Notification Service (WNS) ayarlamak için:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüden **Windows (WNS)** seçeneğini belirleyin.
2. **Paket SID** ve **Güvenlik Anahtarı**için değerleri girin.
3. **Kaydet'i**seçin.

   ![Paket SID ve Güvenlik Anahtarı kutularını gösteren ekran görüntüsü](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Bilgi için Azure [Bildirim Hub'larını kullanarak UWP uygulamalarına bildirim gönder'e](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)bakın.

## <a name="microsoft-push-notification-service-for-windows-phone"></a>Windows Phone için Microsoft Anında Bildirim Hizmeti

Windows Phone için Microsoft Anında Bildirim Hizmeti 'ni (MPNS) ayarlamak için:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüden Windows **Phone (MPNS)** seçeneğini belirleyin.
1. Kimlik doğrulamamış veya kimlik doğrulaması yapılan anında iletme bildirimlerini etkinleştirin:

   a. Kimlik doğrulamamış anında iletme bildirimlerini etkinleştirmek **için, kimliği doğrulanmamış itme Kaydet'i etkinleştir'i** > **Save**seçin.

      ![Kimlik doğrulamamış anında iletme bildirimlerinin nasıl etkinleştirilen gösteren ekran görüntüsü](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Kimlik doğrulaması anında iletme bildirimlerini etkinleştirmek için:
      * Araç çubuğunda **Sertifika Yükle'yi**seçin.
      * Dosya simgesini seçin ve ardından sertifika dosyasını seçin.
      * Sertifika için parola belirtin.
      * **Tamam'ı**seçin.
      * Windows **Phone (MPNS)** sayfasında **Kaydet'i**seçin.

Daha fazla bilgi için [Bildirim Hub'larını kullanarak Windows Phone uygulamalarına yönelik Push bildirimlerine](notification-hubs-windows-mobile-push-notifications-mpns.md)bakın.

## <a name="baidu-android-china"></a>Baidu (Android Çin)

Baidu için anında iletme bildirimleri ayarlamak için:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüden **Baidu 'yu (Android China)** seçin.
2. Baidu bulut itme projesinde Baidu konsolundan aldığınız **Api Anahtarını** girin.
3. Baidu bulut itme projesinde Baidu konsolundan aldığınız **Gizli Anahtarı** girin.
4. **Kaydet'i**seçin.

    ![Anında iletme bildirimleri için Baidu (Android China) yapılandırmasını gösteren Bildirim Hub'larının ekran görüntüsü](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

Bu adımları tamamladığınızda, bir uyarı bildirim merkezinin başarıyla güncelleştirildiğini gösterir. **Kaydet** düğmesi devre dışı bırakılır.

Daha fazla bilgi için Bkz. [Baidu'yu kullanarak Bildirim Hub'larını kullanmaya başlayın.](notification-hubs-baidu-china-android-notifications-get-started.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalındaki bir bildirim merkezi için platform bildirim sistemi ayarlarını nasıl yapılandırabileceğinizi öğrendiniz.

Çeşitli platformlara bildirimleri nasıl iletin hakkında daha fazla bilgi edinmek için aşağıdaki öğreticilere bakın:

-[Bildirim Hub'ları ve](notification-hubs-ios-apple-push-notification-apns-get-started.md)
-Baidu bulut push kullanarak[MPNS](notification-hubs-windows-mobile-push-notifications-mpns.md)
-Push bildirimlerini kullanarak[Windows aygıtında](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
-çalışan bir UWP uygulamasına Bildirim[Hub'ları ve Google FCM](notification-hubs-android-push-notification-google-fcm-get-started.md)
-Push bildirimlerini kullanarak Android cihazlara bildirim hub'ları ve APNS Push bildirimlerini kullanarak iOS[aygıtlarına bildirimleril](notification-hubs-baidu-china-android-notifications-get-started.md)
