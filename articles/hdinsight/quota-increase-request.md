---
title: CPU çekirdeği kota artışı isteği-Azure HDInsight
description: Aboneliğinize ayrılan CPU çekirdekleri için bir artış isteme işlemini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 05/07/2020
ms.openlocfilehash: f8411176e0979bdb894983bcf866abd0e1109e21
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89291679"
---
# <a name="requesting-quota-increases-for-azure-hdinsight"></a>Azure HDInsight için kota artışı isteniyor

CPU çekirdeği kotaları, kaynak kullanımının belirli bir Azure bölgesindeki tüm müşteriler arasında oldukça şekilde dağıtılmasını sağlamaya yardımcı olur. Ancak, bazı durumlarda, iş gereksinimleriniz geçerli kotasından izin verilecek şekilde daha fazla küme kaynağı talep edebilir. Bu gibi durumlarda, veri işleme gereksinimlerinizle eşleşen kümeler dağıtabilmeniz için bir CPU çekirdek kotası artışı isteyebilirsiniz.

Bir kota sınırına ulaştığınızda, yeni kümeler dağıtamazsınız veya daha fazla çalışan düğümü ekleyerek var olan kümelerin ölçeğini düzenleyemezsiniz. Tek kota sınırı, her abonelik için bölge düzeyinde bulunan CPU çekirdekleri kotasına yöneliktir. Örneğin, aboneliğiniz, Doğu ABD bölgesinde 30 CPU çekirdek sınırına sahip olabilir ve bu, Doğu ABD 2 30 ' a kadar CPU çekirdeğine izin verilir.

## <a name="gather-required-information"></a>Gerekli bilgileri toplayın

Kota sınırına ulaşmamızı belirten bir hata aldıysanız, önemli bilgiler toplamak ve bir kota artışı isteği göndermek için bu bölümde açıklanan işlemi kullanın.

1. İstediğiniz küme VM 'nizin boyutunu, ölçeğini ve türünü saptayın.
1. Aboneliğinizin geçerli kota kapasitesi sınırlarını denetleyin. Kullanılabilir çekirdekleri denetlemek için aşağıdaki adımları uygulayın:

    1. [Azure portalında](https://portal.azure.com/) oturum açın.
    1. HDInsight kümesinin **genel bakış** sayfasına gidin.
    1. Sol taraftaki menüden **kota sınırları**' nı seçin. Sayfada kullanılan çekirdek sayısı, mevcut çekirdek sayısı ve toplam çekirdek sayısı görüntülenir.

Kota artışı istemek için aşağıdaki adımları uygulayın:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Sayfanın sol alt tarafında **Yardım + Destek** ' i seçin.

    ![Yardım ve destek düğmesi](./media/quota-increase-request/help-support-button.png)

1. **Yeni destek isteği**’ni seçin.
1. **Yeni destek isteği** sayfasının **Temel Bilgiler** sekmesinde aşağıdaki seçenekleri belirtin:

   - **Sorun türü**: **hizmet ve abonelik limitleri (kotalar)**
   - **Abonelik**: değiştirmek istediğiniz abonelik
   - **Kota türü**: **HDInsight**

     ![HDInsight çekirdek kotasını artırmak için bir destek isteği oluşturma](./media/quota-increase-request/hdinsight-quota-support-request.png)

1. **İleri ' yi seçin: çözümler >>**.
1. **Ayrıntılar** sayfasında, sorun için bir açıklama girin, sorunun önem derecesini, tercih ettiğiniz iletişim yöntemini ve diğer gerekli alanları seçin. Gerekli bilgileri sağladığınızdan emin olmak için aşağıda listelenen şablonu kullanın. Kota artışı istekleri, HDInsight ürün ekibi tarafından değil, Azure kapasite ekibi tarafından değerlendirilir. Sağladığınız daha fazla bilgi, isteğiniz daha büyük olasılıkla onaylanır.

    ```
    I would like to request [SPECIFY DESIRED AMOUNT] on [DESIRED SKU] for [SUBSCRIPTION ID].
    
    My current quota on this subscription is [CURRENT QUOTA AMOUNT].
    
    I would like to use the extra cores for [DETAIL REASON].
    ```

    ![Sorun ayrıntıları](./media/quota-increase-request/problem-details.png)

1. **İleri ' yi seçin: gözden geçir + oluştur >>**.
1. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.

> [!NOTE]  
> Özel bir bölgedeki HDInsight Core kotasını artırmanız gerekiyorsa, [onaylı bir liste isteği gönderebilirsiniz](https://aka.ms/canaryintwhitelist).

[Bir kota artışı istemek için desteğe başvurabilirsiniz](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request).

Bazı sabit kota sınırları vardır. Örneğin, tek bir Azure aboneliğinde en fazla 10.000 çekirdek bulunabilir. Bu limitlere ilişkin ayrıntılar için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 'ta Apache Hadoop, Spark, Kafka ve daha fazlası ile kümeleri](hdinsight-hadoop-provision-linux-clusters.md)ayarlama: HDInsight 'ta kümeleri ayarlamayı ve yapılandırmayı öğrenin.
* [Küme performansını izleme](hdinsight-key-scenarios-to-monitor.md): HDInsight kümenizi izlemek için kümenizin kapasitesini etkileyebilecek önemli senaryolar hakkında bilgi edinin.
