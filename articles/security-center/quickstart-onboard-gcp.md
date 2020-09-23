---
title: GCP hesabınızı Azure Güvenlik Merkezi 'ne bağlama
description: Azure Güvenlik Merkezi 'nden GCP kaynaklarınızı izleme
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: d5f478c279eb4c07332d047935f7c5dab89fd4b7
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941954"
---
#  <a name="connect-your-gcp-accounts-to-azure-security-center"></a>GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

GCP hesabınızı Güvenlik Merkezi 'ne ekleme, GCP Güvenlik komutunu ve Azure Güvenlik Merkezi 'ni tümleştirir. Bu nedenle, güvenlik merkezi bu bulut ortamlarının her ikisi arasında görünürlük ve koruma sağlayarak şunları sağlar:

- Güvenlik yapılandırması hataları algılanamadı
- Güvenlik Merkezi önerilerini ve GCP güvenlik Komut Merkezi bulgularını gösteren tek bir görünüm
- Kurulduğu of the GCP kaynaklarınızın güvenlik merkezi 'Ndeki güvenli puan hesaplamalarına
- Güvenlik Merkezi 'nin mevzuat uyumluluk panosuna CIS standardına dayalı GCP güvenlik Komut Merkezi önerilerini tümleştirme

Aşağıdaki ekran görüntüsünde, güvenlik merkezi 'nin genel bakış panosunda GCP projelerinin görüntülendiğini görebilirsiniz.

:::image type="content" source="./media/quickstart-onboard-gcp/gcp-account-in-overview.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi" lightbox="./media/quickstart-onboard-gcp/gcp-account-in-overview.png":::


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme|
|Fiyat|[Sunucular Için Azure Defender](defender-for-servers-introduction.md) gerekir|
|Gerekli roller ve izinler:|İlgili Azure aboneliğinde **sahip** veya **katkıda** bulunan|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="connect-your-gcp-account"></a>GCP hesabınıza bağlanma

### <a name="step-1-set-up-gcp-security-command-center-with-security-health-analytics"></a>Adım 1. Güvenlik sistem durumu analizi ile GCP güvenlik komut merkezini ayarlama

Kuruluşunuzdaki tüm GCP projeleri için de şunları yapmanız gerekir:

1. [GCP belgelerindeki bu yönergeleri](https://cloud.google.com/security-command-center/docs/quickstart-scc-setup)kullanarak **GCP güvenlik komut merkezini** ayarlayın.
1. [Bu yönergeleri kullanarak GCP belgelerinden](https://cloud.google.com/security-command-center/docs/how-to-use-security-health-analytics) **güvenlik sistem durumu** analizini etkinleştirin.
1. Güvenlik komut merkezine veri akışı olduğunu doğrulayın.

Güvenlik yapılandırması için GCP ortamınızı bağlama yönergeleri, Google 'ın güvenlik yapılandırma önerilerini tüketme önerilerini izler. Tümleştirme Google güvenlik komut merkezinden yararlanır ve faturanızı etkileyebilecek ek kaynaklar kullanacaktır.

Güvenlik sistem durumu analizlerini ilk kez etkinleştirdiğinizde, verilerin kullanılabilir olması birkaç saat sürebilir.


### <a name="step-2-enable-gcp-security-command-center-api"></a>Adım 2. GCP güvenlik Komut Merkezi API 'sini etkinleştir

1. Google 'ın **bulut konsolu API kitaplığından**Azure Güvenlik Merkezi 'ne bağlanmak istediğiniz projeyi seçin.
1. API kitaplığı 'nda **Güvenlik Komut Merkezi API 'sini**bulun ve seçin.
1. API 'nin sayfasında **Etkinleştir**' i seçin.

[Güvenlik Komut Merkezi API 'si](https://cloud.google.com/security-command-center/docs/reference/rest/)hakkında daha fazla bilgi edinin.


### <a name="step-3-create-a-dedicated-service-account-for-the-security-configuration-integration"></a>3. Adım Güvenlik Yapılandırması tümleştirmesi için adanmış bir hizmet hesabı oluşturma

1. **GCP konsolunda**, güvenlik merkezi 'ne bağlanmak istediğiniz projeyi seçin.
1. **Gezinti menüsünde**, **IAM & yönetici** seçenekleri altında **hizmet hesapları**' nı seçin.
1. **HIZMET hesabı oluştur**' u seçin.
1. Hesap adı girin ve **Oluştur**' u seçin.
1. **Rolü** **Güvenlik Merkezi yönetici Görüntüleyicisi**olarak belirtin ve **devam**' ı seçin.
1. **Kullanıcılara bu hizmet hesabına erişim Izni ver** bölümü isteğe bağlıdır. **Bitti** seçeneğini belirleyin.
1. Oluşturulan hizmet hesabının **e-posta değerini** kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. **Gezinti menüsünde**, **IAM & yönetici** seçenekleri altında **IAM** ' i seçin.
    1. Kuruluş düzeyine geçin.
    1. **Ekle**' yi seçin.
    1. **Yeni Üyeler** alanında, daha önce kopyaladığınız **e-posta değerini** yapıştırın.
    1. Rolü **Güvenlik Merkezi yönetici Görüntüleyicisi** olarak belirtip Kaydet ' i seçin.
        :::image type="content" source="./media/quickstart-onboard-gcp/iam-settings-gcp-permissions-admin-viewer.png" alt-text="İlgili GCP izinlerini ayarlama":::


### <a name="step-4-create-a-private-key-for-the-dedicated-service-account"></a>4. Adım: Adanmış hizmet hesabı için özel anahtar oluşturma
1. Proje düzeyine geçin.
1. **Gezinti menüsünde**, **IAM & yönetici** seçenekleri altında **hizmet hesapları**' nı seçin.
1. Adanmış hizmet hesabını açın ve Düzenle ' yi seçin.
1. **Anahtarlar** bölümünde **anahtar Ekle** ' yi ve ardından **Yeni anahtar oluştur**' u seçin.
1. Özel anahtar oluştur ekranında **JSON**' ı seçin ve ardından **Oluştur**' u seçin.
1. Bu JSON dosyasını daha sonra kullanmak üzere kaydedin.


### <a name="step-5-connect-gcp-to-security-center"></a>5. Adım. GCP 'yi Güvenlik Merkezi 'ne bağlama 
1. Güvenlik Merkezi menüsünden **bulut bağlayıcıları**' nı seçin.
1. GCP hesabı Ekle ' yi seçin.
1. Ekleme sayfasında, aşağıdakileri yapın ve ardından **İleri**' yi seçin.
    1. Seçilen aboneliği doğrulayın.
    1. **Görünen ad** alanına bağlayıcı için bir görünen ad girin.
    1. **Kuruluş kimliği** alanında, kuruluşunuzun kimliğini girin. Bunu bilmiyorsanız bkz. [kuruluş oluşturma ve yönetme](https://cloud.google.com/resource-manager/docs/creating-managing-organization).
    1. **Özel anahtar** dosyası kutusunda, adım 4 ' te indirdiğiniz JSON dosyasına gidin [. Adanmış hizmet hesabı için özel bir anahtar oluşturun](#step-4-create-a-private-key-for-the-dedicated-service-account).


### <a name="step-6-confirmation"></a>6. Adım. Onay

Bağlayıcı başarıyla oluşturulduğunda ve GCP güvenlik Komut Merkezi düzgün şekilde yapılandırıldığında:

- GCP CIS standardı, güvenlik merkezi 'nin mevzuat uyumluluk panosunda gösterilir.
- GCP kaynaklarınız için güvenlik önerileri, güvenlik merkezi portalında ve düzenleme sonrasında yasal uyumluluk panosu 5-10 dakika sonra görüntülenir:   :::image type="content" source="./media/quickstart-onboard-gcp/gcp-resources-in-recommendations.png" alt-text="Güvenlik Merkezi 'nin öneriler sayfasında GCP kaynakları ve önerileri":::


## <a name="monitoring-your-gcp-resources"></a>GCP kaynaklarınızı izleme

Yukarıda gösterildiği gibi, Azure Güvenlik Merkezi 'nin güvenlik önerileri sayfası, doğru bir çoklu bulut görünümü için Azure ve AWS kaynaklarıyla birlikte GCP kaynaklarınızı görüntüler.

Kaynaklarınızın tüm etkin önerilerini kaynak türüne göre görüntülemek için, güvenlik merkezi 'nin varlık envanteri sayfasını kullanın ve ilgilendiğiniz GCP kaynak türüne filtre uygulayın:

:::image type="content" source="./media/quickstart-onboard-gcp/gcp-resource-types-in-inventory.png" alt-text="Varlık envanteri sayfasının kaynak türü filtresi, GCP seçeneklerini gösterir"::: 


## <a name="next-steps"></a>Sonraki adımlar

GCP hesabınızı bağlamak, Azure Güvenlik Merkezi 'nde bulunan çok bulut deneyiminin bir parçasıdır. İlgili bilgiler için şu sayfaya bakın:

- [AWS hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-aws.md)
