---
title: "Öğretici: Azure HDInsight'ta Apache Ambari e-posta bildirimlerini yapılandırın"
description: Bu makalede, e-posta bildirimleri için Apache Ambari ile SendGrid nasıl kullanılacağı açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: 21376eb40fb40abe67f7e03d15aabd7d89ea62f8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80082317"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta Apache Ambari e-posta bildirimlerini yapılandırın

Bu eğitimde, SendGrid kullanarak Apache Ambari e-posta bildirimlerini yapılandıracaksınız. [Apache Ambari,](./hdinsight-hadoop-manage-ambari.md) kullanımı kolay bir web Kullanıcı Arabirimi ve REST API sağlayarak bir HDInsight kümesinin yönetimini ve izlenmesini kolaylaştırır. Ambari HDInsight kümelerinde yer alan ve kümeizlemek ve yapılandırma değişiklikleri yapmak için kullanılır. [SendGrid,](https://sendgrid.com/solutions/) özel entegrasyonu kolaylaştıran esnek API'lerin yanı sıra güvenilir işlemsel e-posta teslimi, ölçeklenebilirlik ve gerçek zamanlı analiz sağlayan bulut tabanlı ücretsiz bir e-posta hizmetidir. Azure müşterileri her ay 25.000 ücretsiz e-postanın kilidini açabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sendgrid Kullanıcı Adı Edinin
> * Apache Ambari e-posta bildirimlerini yapılandırma

## <a name="prerequisites"></a>Ön koşullar

* SendGrid e-posta hesabı. Talimatlar için [Azure ile SendGrid'i Kullanarak E-posta Gönderme hakkında](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email) bilgi alabiliyorum.

* Bir HDInsight kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](./hdinsight-hadoop-create-linux-clusters-portal.md)

## <a name="obtain-sendgrid-username"></a>SendGrid Kullanıcı Adını Edinin

1. Azure [portalından](https://portal.azure.com)SendGrid kaynağınıza gidin.

1. Genel Bakış sayfasından, hesabınız için SendGrid web sayfasına gitmek için **Yönet'i**seçin.

    ![Azure portalında SendGrid'e genel bakış](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. Sol menüden, hesap adınıza gidin ve ardından **Hesap Ayrıntıları.**

    ![SendGrid pano navigasyonu](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. Hesap **Ayrıntıları** sayfasından **Kullanıcı Adını**kaydedin.

    ![SendGrid hesap detayları](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Ambari e-posta bildirimini yapılandırma

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. **Eylemler** açılır listesinden **Bildirimleri Yönet'i**seçin.

1. Uyarı **Bildirimlerini Yönet** penceresinden **+** simgeyi seçin.

    ![Ambari uyarı bildirimi oluşturmak](./media/apache-ambari-email/azure-portal-create-notification.png)

1. Uyarı **Bildirimi Oluştur** iletişim kutusundan aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama |
    |---|---|
    |Adı|Bildirim için bir ad sağlayın.|
    |Gruplar|İstenildiği gibi yapılandırın.|
    |Severity|İstenildiği gibi yapılandırın.|
    |Açıklama|İsteğe bağlı.|
    |Yöntem|**E-POSTA'da**bırakın.|
    |E-posta gönder|Virgülle ayrılmış bildirimleri almak için e-posta(lar) sağlayın.|
    |SMTP Sunucusu|`smtp.sendgrid.net`|
    |SMTP Bağlantı Noktası|25 veya 587 (şifrelenmemiş/TLS bağlantıları için).|
    |E-posta Gönderen|Bir e-posta adresi girin. Adresin gerçek olması gerekmez.|
    |Kimlik doğrulamayı kullanma|Bu onay kutusunu seçin.|
    |Kullanıcı adı|SendGrid kullanıcı adını sağlayın.|
    |Parola|Azure'da SendGrid kaynağını oluşturduğunuzda kullandığınız parolayı sağlayın.|
    |Şifre Onayı|Parolayı yeniden girin.|
    |TLS'yi başlat|Bu onay kutusunu seçin|

    ![Ambari uyarı bildirimi oluşturmak](./media/apache-ambari-email/ambari-create-alert-notification.png)

    **Kaydet'i**seçin. **Uyarı Bildirimlerini Yönet** penceresine geri dönersiniz.

1. Uyarı **Bildirimlerini Yönet** penceresinden **Kapat'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, SendGrid kullanarak Apache Ambari e-posta bildirimlerini yapılandırmayı öğrendiniz. Apache Ambari hakkında daha fazla bilgi edinmek için aşağıdakileri kullanın:

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](./hdinsight-hadoop-manage-ambari.md)

* [Uyarı bildirimi oluşturma](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)
