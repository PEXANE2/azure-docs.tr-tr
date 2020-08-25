---
title: "Öğretici: Azure HDInsight 'ta Apache ambarı e-posta bildirimlerini yapılandırma"
description: Bu makalede, e-posta bildirimleri için bir Apache ambarı ile SendGrid 'in nasıl kullanılacağı açıklanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: 21376eb40fb40abe67f7e03d15aabd7d89ea62f8
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "80082317"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Öğretici: Azure HDInsight 'ta Apache ambarı e-posta bildirimlerini yapılandırma

Bu öğreticide, SendGrid kullanarak Apache ambarı e-posta bildirimlerini yapılandıracaksınız. [Apache ambarı](./hdinsight-hadoop-manage-ambari.md) , kullanımı kolay bir Web Kullanıcı arabirimi ve REST API sağlayarak bir HDInsight kümesinin yönetimini ve izlenmesini basitleştirir. Ambarı HDInsight kümelerine dahil edilmiştir ve kümeyi izlemek ve yapılandırma değişiklikleri yapmak için kullanılır. [SendGrid](https://sendgrid.com/solutions/) , özel tümleştirmeyi kolaylaştıran esnek API 'lerle birlikte güvenilir işlem e-posta teslimi, ölçeklenebilirlik ve gerçek zamanlı analizler sağlayan ücretsiz bir bulut tabanlı e-posta hizmetidir. Azure müşterileri her ay 25.000 ücretsiz e-postanın kilidini açabilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * SendGrid Kullanıcı adı alma
> * Apache ambarı e-posta bildirimlerini yapılandırma

## <a name="prerequisites"></a>Önkoşullar

* SendGrid e-posta hesabı. Yönergeler için bkz. [Azure Ile SendGrid kullanarak e-posta gönderme](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email) .

* An HDInsight küme. Bkz. [Azure Portal kullanarak Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="obtain-sendgrid-username"></a>SendGrid Kullanıcı adı alma

1. [Azure Portal](https://portal.azure.com), SendGrid kaynağına gidin.

1. Hesabınız için SendGrid Web sayfasına gitmek üzere Genel Bakış sayfasında **Yönet**' i seçin.

    ![Azure portalında SendGrid 'e genel bakış](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. Sol menüden hesap adınız ' ne ve ardından **Hesap ayrıntıları**' na gidin.

    ![SendGrid Pano gezintisi](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. **Hesap ayrıntıları** sayfasında, **Kullanıcı adını**kaydedin.

    ![SendGrid hesap ayrıntıları](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Ambarı e-posta bildirimini yapılandırma

1. Bir Web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts` , `CLUSTERNAME` Kümenizin adı olan ' a gidin.

1. **Eylemler** açılan listesinden **Bildirimleri Yönet**' i seçin.

1. **Uyarı bildirimlerini yönet** penceresinde **+** simgesini seçin.

    ![Ambarı oluşturma uyarı bildirimi](./media/apache-ambari-email/azure-portal-create-notification.png)

1. **Uyarı bildirimi oluştur** iletişim kutusunda aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama |
    |---|---|
    |Ad|Bildirim için bir ad girin.|
    |Gruplar|İstediğiniz şekilde yapılandırın.|
    |Önem Derecesi|İstediğiniz şekilde yapılandırın.|
    |Açıklama|İsteğe bağlı.|
    |Yöntem|**E-postada**bırakın.|
    |E-posta|Bildirimleri alacak e-postaları virgülle ayırarak belirtin.|
    |SMTP Sunucusu|`smtp.sendgrid.net`|
    |SMTP bağlantı noktası|25 veya 587 (şifrelenmemiş/TLS bağlantıları için).|
    |E-posta|Bir e-posta adresi sağlayın. Adresin gerçek olması gerekmez.|
    |Kimlik doğrulaması kullan|Bu onay kutusunu seçin.|
    |Kullanıcı adı|SendGrid Kullanıcı adını sağlayın.|
    |Parola|Azure 'da SendGrid kaynağını oluştururken kullandığınız parolayı girin.|
    |Parola onayı|Parolayı yeniden girin.|
    |TLS 'yi Başlat|Bu onay kutusunu seçin|

    ![Ambarı oluşturma uyarı bildirimi](./media/apache-ambari-email/ambari-create-alert-notification.png)

    **Kaydet**’i seçin. **Uyarı bildirimlerini yönet** penceresine geri dönersiniz.

1. **Uyarı bildirimlerini yönet** penceresinde **Kapat**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, SendGrid kullanarak Apache ambarı e-posta bildirimlerinin nasıl yapılandırılacağını öğrendiniz. Apache ambarı hakkında daha fazla bilgi edinmek için aşağıdakileri kullanın:

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](./hdinsight-hadoop-manage-ambari.md)

* [Uyarı bildirimi oluşturma](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)
