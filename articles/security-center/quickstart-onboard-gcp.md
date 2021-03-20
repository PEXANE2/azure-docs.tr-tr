---
title: GCP hesabınızı Azure Güvenlik Merkezi 'ne bağlama
description: Azure Güvenlik Merkezi 'nden GCP kaynaklarınızı izleme
author: memildin
ms.author: memildin
ms.date: 02/08/2021
ms.topic: quickstart
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 94c7a800fc551faf6650b8e30fe7c2188f7d2dbb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100008392"
---
#  <a name="connect-your-gcp-accounts-to-azure-security-center"></a>GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

GCP hesaplarınızı Güvenlik Merkezi 'ne ekleme, GCP Güvenlik komutunu ve Azure Güvenlik Merkezi 'ni tümleştirir. Bu nedenle, güvenlik merkezi bu bulut ortamlarının her ikisi arasında görünürlük ve koruma sağlayarak şunları sağlar:

- Güvenlik yapılandırması hataları algılanamadı
- Güvenlik Merkezi önerilerini ve GCP güvenlik Komut Merkezi bulgularını gösteren tek bir görünüm
- Kurulduğu of the GCP kaynaklarınızın güvenlik merkezi 'Ndeki güvenli puan hesaplamalarına
- Güvenlik Merkezi 'nin mevzuat uyumluluk panosuna CIS standardına dayalı GCP güvenlik Komut Merkezi önerilerini tümleştirme

Aşağıdaki ekran görüntüsünde, güvenlik merkezi 'nin genel bakış panosunda GCP projelerinin görüntülendiğini görebilirsiniz.

:::image type="content" source="./media/quickstart-onboard-gcp/gcp-account-in-overview.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi" lightbox="./media/quickstart-onboard-gcp/gcp-account-in-overview.png":::


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik (GA)|
|Fiyat|[Sunucular Için Azure Defender](defender-for-servers-introduction.md) gerekir|
|Gerekli roller ve izinler:|İlgili Azure aboneliğinde **sahip** veya **katkıda** bulunan|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="connect-your-gcp-account"></a>GCP hesabınıza bağlanma

Güvenlik Merkezi 'nden izlemek istediğiniz her kuruluş için bir bağlayıcı oluşturun.

GCP hesaplarınızı belirli Azure aboneliklerine bağlarken, [Google Cloud kaynak hiyerarşisini](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#resource-hierarchy-detail) ve bu yönergeleri göz önünde bulundurun:

- GCP hesaplarınızı, *kuruluş* düzeyinde ASC 'ye bağlayabilirsiniz
- Birden çok kuruluşun bir Azure aboneliğine bağlanmasını sağlayabilirsiniz
- Birden çok kuruluşun birden çok Azure aboneliğine bağlanmasını sağlayabilirsiniz
- Bir kuruluşa bağlandığınızda, bu kuruluştaki tüm *Projeler* Güvenlik Merkezi 'ne eklenir

GCP bulut bağlayıcınızı oluşturmak için aşağıdaki adımları izleyin. 

### <a name="step-1-set-up-gcp-security-command-center-with-security-health-analytics"></a>Adım 1. Güvenlik sistem durumu analizi ile GCP güvenlik komut merkezini ayarlama

Kuruluşunuzdaki tüm GCP projeleri için de şunları yapmanız gerekir:

1. [GCP belgelerindeki bu yönergeleri](https://cloud.google.com/security-command-center/docs/quickstart-scc-setup)kullanarak **GCP güvenlik komut merkezini** ayarlayın.
1. [Bu yönergeleri kullanarak GCP belgelerinden](https://cloud.google.com/security-command-center/docs/how-to-use-security-health-analytics) **güvenlik sistem durumu** analizini etkinleştirin.
1. Güvenlik komut merkezine veri akışı olduğunu doğrulayın.

Güvenlik yapılandırması için GCP ortamınızı bağlama yönergeleri, Google 'ın güvenlik yapılandırma önerilerini tüketme önerilerini izler. Tümleştirme Google güvenlik komut merkezinden yararlanır ve faturanızı etkileyebilecek ek kaynaklar kullanacaktır.

Güvenlik sistem durumu analizlerini ilk kez etkinleştirdiğinizde, verilerin kullanılabilir olması birkaç saat sürebilir.


### <a name="step-2-enable-gcp-security-command-center-api"></a>Adım 2. GCP güvenlik Komut Merkezi API 'sini etkinleştir

1. Google 'ın **bulut konsolu API kitaplığından**, Azure Güvenlik Merkezi 'ne bağlanmak istediğiniz her bir projeyi seçin.
1. API kitaplığı 'nda **Güvenlik Komut Merkezi API 'sini** bulun ve seçin.
1. API 'nin sayfasında **Etkinleştir**' i seçin.

[Güvenlik Komut Merkezi API 'si](https://cloud.google.com/security-command-center/docs/reference/rest/)hakkında daha fazla bilgi edinin.


### <a name="step-3-create-a-dedicated-service-account-for-the-security-configuration-integration"></a>3. Adım Güvenlik Yapılandırması tümleştirmesi için adanmış bir hizmet hesabı oluşturma

1. **GCP konsolunda**, gereken hizmet hesabını oluşturmakta olduğunuz kuruluştan bir proje seçin. 

    > [!NOTE]
    > Bu hizmet hesabı kuruluş düzeyine eklendiğinde, kuruluştaki diğer tüm etkin projelerden güvenlik Komut Merkezi tarafından toplanan verilere erişmek için kullanılacaktır. 

1. **Gezinti menüsünde**, **IAM & yönetici** seçenekleri altında **hizmet hesapları**' nı seçin.
1. **HIZMET hesabı oluştur**' u seçin.
1. Hesap adı girin ve **Oluştur**' u seçin.
1. **Rolü** **Güvenlik Merkezi yönetici Görüntüleyicisi** olarak belirtin ve **devam**' ı seçin.
1. **Kullanıcılara bu hizmet hesabına erişim Izni ver** bölümü isteğe bağlıdır. **Bitti** seçeneğini belirleyin.
1. Oluşturulan hizmet hesabının **e-posta değerini** kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. **Gezinti menüsünde**, **IAM & yönetici** seçenekleri altında **IAM** ' i seçin.
    1. Kuruluş düzeyine geçin.
    1. **Ekle**' yi seçin.
    1. **Yeni Üyeler** alanında, daha önce kopyaladığınız **e-posta değerini** yapıştırın.
    1. Rolü **Güvenlik Merkezi yönetici Görüntüleyicisi** olarak belirtip **Kaydet**' i seçin.
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


## <a name="faq-for-connecting-gcp-accounts-to-azure-security-center"></a>GCP hesaplarını Azure Güvenlik Merkezi 'ne bağlama hakkında SSS

### <a name="can-i-connect-multiple-gcp-organizations-to-security-center"></a>Birden fazla GCP kurumumu Güvenlik Merkezi 'ne bağlayabilirim miyim?
Evet. Güvenlik Merkezi 'nin GCP Bağlayıcısı, Google bulut kaynaklarınızı *kuruluş* düzeyinde bağlar. 

Güvenlik Merkezi 'nden izlemek istediğiniz her GCP organizasyonu için bir bağlayıcı oluşturun. Bir kuruluşa bağlandığınızda, bu kuruluştaki tüm projeler güvenlik merkezi 'ne eklenir.

[Google 'ın çevrimiçi belgelerinden](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)Google bulut kaynak hiyerarşisi hakkında bilgi edinin.


### <a name="is-there-an-api-for-connecting-my-gcp-resources-to-security-center"></a>GCP kaynaklarımı Güvenlik Merkezi 'ne bağlamak için bir API var mı?
Evet. Güvenlik Merkezi bulut bağlayıcılarını bir REST API oluşturmak, düzenlemek veya silmek için, [BAĞLAYıCıLAR API](/rest/api/securitycenter/connectors)'sinin ayrıntılarına bakın.

## <a name="next-steps"></a>Sonraki adımlar

GCP hesabınızı bağlamak, Azure Güvenlik Merkezi 'nde bulunan çok bulut deneyiminin bir parçasıdır. İlgili bilgiler için şu sayfaya bakın:

- [AWS hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-aws.md)
- [Google Cloud kaynak hiyerarşisi](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)-Google 'ın çevrimiçi belgelerinden Google bulut kaynak hiyerarşisi hakkında bilgi edinin