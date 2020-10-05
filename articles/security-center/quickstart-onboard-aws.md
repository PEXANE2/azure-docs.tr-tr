---
title: AWS hesabınızı Azure Güvenlik Merkezi 'ne bağlama
description: AWS kaynaklarınızı Azure Güvenlik Merkezi 'nden izleme
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: quickstart
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: cddae0a7115fc2999b52eaba7df2b49db509981b
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91449045"
---
#  <a name="connect-your-aws-accounts-to-azure-security-center"></a>AWS hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS hesabınızı Güvenlik Merkezi 'ne ekleme, AWS güvenlik hub 'ını ve Azure Güvenlik Merkezi 'ni tümleştirir. Bu nedenle, güvenlik merkezi bu bulut ortamlarının her ikisi arasında görünürlük ve koruma sağlayarak şunları sağlar:

- Otomatik aracı sağlama (Güvenlik Merkezi, Log Analytics aracısını AWS örneklerinize dağıtmak için [Azure yay](../azure-arc/servers/overview.md) kullanır)
- İlke yönetimi
- Güvenlik açığı yönetimi
- Gömülü uç nokta algılama ve yanıtı (EDR)
- Güvenlik yapılandırması hataları algılanamadı
- Güvenlik Merkezi önerilerini ve AWS Güvenlik Merkezi bulgularını gösteren tek bir görünüm
- AWS kaynaklarınızın kurulduğu, güvenlik merkezi 'nin güvenli puanı hesaplamalarına
- AWS kaynaklarınızın yasal uyumluluk değerlendirmeleri

Aşağıdaki ekran görüntüsünde, güvenlik merkezi 'nin genel bakış panosunda görünen AWS hesaplarını görebilirsiniz.

:::image type="content" source="./media/quickstart-onboard-aws/aws-account-in-overview.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi" lightbox="./media/quickstart-onboard-gcp/gcp-account-in-overview.png":::

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme|
|Fiyat|[Sunucular Için Azure Defender](defender-for-servers-introduction.md) gerekir|
|Gerekli roller ve izinler:|İlgili Azure aboneliğinde **sahip** veya **katkıda** bulunan|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||



## <a name="connect-your-aws-account"></a>AWS hesabınıza bağlanma

### <a name="step-1-set-up-aws-security-hub"></a>Adım 1. AWS güvenlik merkezini ayarlama:

1. Birden çok bölgenin güvenlik önerilerini görüntülemek için, ilgili her bölge için aşağıdaki adımları tekrarlayın.

    > [!IMPORTANT]
    > AWS ana hesabı kullanıyorsanız, tüm ilgili bölgelerde ana hesabı ve tüm bağlı üye hesaplarını yapılandırmak için aşağıdaki üç adımı tekrarlayın

    1. [AWS yapılandırmasını](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html)etkinleştirin.
    1. [AWS güvenlik](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html)merkezini etkinleştirin.
    1. Güvenlik Merkezi 'ne veri akışı olduğunu doğrulayın.

        Güvenlik Merkezi 'ni ilk kez etkinleştirdiğinizde, verilerin kullanılabilir olması birkaç saat sürebilir.

### <a name="step-2-set-up-authentication-for-security-center-in-aws"></a>Adım 2. AWS 'de Güvenlik Merkezi için kimlik doğrulamasını ayarlama

Güvenlik Merkezi 'nin AWS 'de kimlik doğrulamasına izin vermek için iki yol vardır:

- **Güvenlik Merkezi için BIR IAM rolü oluşturma** -bu en güvenli yöntemdir ve önerilir
- **Güvenlik Merkezi Için AWS kullanıcısı** -IAM etkin değilse daha az güvenli bir seçenek

#### <a name="create-an-iam-role-for-security-center"></a>Güvenlik Merkezi için bir ıAM rolü oluşturma
1. Amazon Web Services konsolınızdan **güvenlik, kimlik & uyumluluk**altında **IAM**' i seçin.
    :::image type="content" source="./media/quickstart-onboard-aws/aws-identity-and-compliance.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi":::

1. **Roller** ' i ve **rol oluştur**' u seçin.
1. **Başka BIR AWS hesabı**seçin.
1. Şu ayrıntıları girin:

    - **Hesap kimliği** -Güvenlik Merkezi 'ndeki AWS Bağlayıcısı sayfasında gösterildiği gibi MICROSOFT hesap kimliğini (**158177204117**) girin.
    - **Dış kimlik gerektir** -seçilmelidir
    - **Dış kimlik** -Güvenlik Merkezi 'ndeki AWS Bağlayıcısı sayfasında gösterildiği gıbı abonelik kimliğini girin 

1. **İleri**’yi seçin.
1. **İzin Ilkeleri Ekle** bölümünde aşağıdaki ilkeleri seçin:

    - Securityauıdıt
    - AmazonSSMAutomationRole
    - AWSSecurityHubReadOnlyAccess

1. İsteğe bağlı olarak etiketler ekleyin. Kullanıcıya Etiketler eklemek bağlantıyı etkilemez.
1. **İleri**’yi seçin.

1. Roller listesinde, oluşturduğunuz rolü seçin

1. Amazon kaynak adını (ARN) daha sonra kaydedin. 

#### <a name="create-an-aws-user-for-security-center"></a>Güvenlik Merkezi için AWS kullanıcısı oluşturma 
1. **Kullanıcılar** sekmesini açın ve **Kullanıcı Ekle**' yi seçin.
1. **Ayrıntılar** adımında, Güvenlik Merkezi için bir Kullanıcı adı girin ve AWS erişim türü için **programlı erişim** ' i seçtiğinizden emin olun. 
1. **Sonraki izinleri**seçin.
1. **Mevcut ilkeleri doğrudan Ekle** ' yi seçin ve aşağıdaki ilkeleri uygulayın:
    - Securityauıdıt
    - AmazonSSMAutomationRole
    - AWSSecurityHubReadOnlyAccess
    
1. **Sonraki: Etiketler**' i seçin. İsteğe bağlı olarak etiketler ekleyin. Kullanıcıya Etiketler eklemek bağlantıyı etkilemez.
1. **Gözden geçir**' i seçin.
1. Otomatik olarak oluşturulan **erişim anahtarı kimliğini** ve **gizli anahtar** CSV dosyasını daha sonra için kaydedin.
1. Özeti gözden geçirin ve **Kullanıcı oluştur**' a tıklayın.


### <a name="step-3-configure-the-ssm-agent"></a>3. Adım SSI aracısını yapılandırma

AWS sistem yöneticisi, AWS kaynaklarınızın tamamında görevleri otomatikleştirmek için gereklidir. EC2 örneklerinizin SSI Aracısı yoksa, Amazon ile ilgili yönergeleri izleyin:

- [Windows örneklerine SSI Aracısı yükleme ve yapılandırma](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-win.html)
- [Amazon EC2 Linux örneklerine SSI Aracısı yükleme ve yapılandırma](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html)


### <a name="step-4-create-a-service-principal-for-onboarding-at-scale"></a>4. Adım: Ölçekte ekleme için bir hizmet sorumlusu oluşturma

Ekleme için kullanmak istediğiniz abonelikte **sahip** olarak, [ölçekte ekleme Için hizmet sorumlusu oluşturma](../azure-arc/servers/onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) bölümünde açıklandığı gibi, Azure Arc ekleme için bir hizmet sorumlusu oluşturun


### <a name="step-5-connect-aws-to-security-center"></a>5. Adım. AWS 'yi güvenlik merkezine bağlama

1. Güvenlik Merkezi 'nin menüsünden **çoklu bulut bağlayıcıları**' nı seçin.
1. **AWS hesabı ekle**' yi seçin.
    :::image type="content" source="./media/quickstart-onboard-aws/add-aws-account.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi":::
1. **AWS kimlik doğrulama** sekmesindeki seçenekleri yapılandırın:
    1. Bağlayıcı için bir **görünen ad** girin.
    1. Aboneliğin doğru olduğunu onaylayın. Bu, bağlayıcı ve AWS Güvenlik Merkezi önerilerini içerecek olan abonelikdir.
    1. 2. adımda seçtiğiniz kimlik doğrulama seçeneğine bağlı olarak [. AWS 'de Güvenlik Merkezi için kimlik doğrulamasını ayarlama](#step-2-set-up-authentication-for-security-center-in-aws):
        - **Rol varsay** ' ı seçin ve [güvenlik MERKEZI için bir IAM rolü oluşturma](#create-an-iam-role-for-security-center) :::image type="content" source="./media/quickstart-onboard-aws/paste-arn-in-portal.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi"::: yapıştırın.

            VEYA

        - **Kimlik bilgileri** ' ni seçin **ve erişim anahtarı** ve **gizli anahtarı** [Güvenlik Merkezi için AWS kullanıcısı oluşturma](#create-an-aws-user-for-security-center)' ya kaydettiğiniz. csv dosyasından yapıştırın.
1. **İleri**’yi seçin.
1. **Azure Arc yapılandırma** sekmesindeki seçenekleri yapılandırın:

    Güvenlik Merkezi, bağlı AWS hesabındaki EC2 örneklerini bulur ve bunları Azure yaya eklemek için SSD kullanır. 

    > [!TIP]
    > Desteklenen işletim sistemlerinin listesi aşağıdaki SSS bölümünde verilmiştir.

    1. Bulunan AWS EC2s 'nin seçili abonelikte eklendi olacağı **kaynak grubunu** ve **Azure bölgesini** seçin.
    1. Burada açıklandığı gibi, Azure Arc için **hizmet sorumlusu kimliği** ve **hizmet sorumlusu istemci gizli anahtarı** ' nı, [ölçekte ekleme için bir hizmet sorumlusu oluşturun](../azure-arc/servers/onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale)
    1. Makine bir ara sunucu üzerinden İnternet 'e bağlanıyorsa, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullandığı ad ve bağlantı noktası numarasını belirtin. Değeri şu biçimde girin ```http://<proxyURL>:<proxyport>```
    1. **Gözden geçir ve oluştur**’u seçin.

        Özet bilgilerini gözden geçirin

        Etiketler bölümleri, her eklendi EC2 için otomatik olarak oluşturulacak tüm Azure etiketlerini, Azure 'da kolayca tanımak için ilgili ayrıntılarla birlikte listeler. 

        Azure [kaynakları ve yönetim hiyerarşinizi düzenlemek için etiketleri kullanma etiketli](../azure-resource-manager/management/tag-resources.md)Azure etiketleri hakkında daha fazla bilgi edinin.

### <a name="step-7-confirmation"></a>7. Adım. Onay

Bağlayıcı başarıyla oluşturulduğunda ve AWS Güvenlik Merkezi düzgün şekilde yapılandırıldığında:

- Güvenlik Merkezi, AWS EC2 örnekleri için ortamı tarar, Azure Arc 'a ekleyerek Log Analytics aracısını yüklemeyi ve tehdit koruması ve güvenlik önerileri sağlamayı sağlar. 
- ASC hizmeti, her 6 saatte bir yeni AWS EC2 örneğini tarar ve bunları yapılandırmaya göre yapar.
- AWS CIS standardı, güvenlik merkezi 'nin mevzuat uyumluluk panosunda gösterilir.
- Güvenlik Merkezi ilkesi etkinse, öneriler, güvenlik merkezi portalında ve düzenleme tamamlandıktan sonra yasal uyumluluk panosu 5-10 dakika içinde görüntülenir.
    :::image type="content" source="./media/quickstart-onboard-aws/aws-resources-in-recommendations.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi":::



## <a name="monitoring-your-aws-resources"></a>AWS kaynaklarınızı izleme

Yukarıda gösterildiği gibi, Azure Güvenlik Merkezi 'nin güvenlik önerileri sayfası, doğru bir çoklu bulut görünümü için AWS kaynaklarınızı Azure ve GCP kaynaklarıyla birlikte görüntüler.

Kaynaklarınızın tüm etkin önerilerini kaynak türüne göre görüntülemek için, güvenlik merkezi 'nin varlık envanteri sayfasını kullanın ve ilgilendiğiniz AWS kaynak türüne filtre uygulayın:

:::image type="content" source="./media/quickstart-onboard-aws/aws-resource-types-in-inventory.png" alt-text="Güvenlik Merkezi 'nin genel bakış panosunda listelenen 3 GCP projesi"::: 


## <a name="aws-in-security-center-faq"></a>Güvenlik Merkezi 'nde AWS hakkında SSS

### <a name="what-operating-systems-for-my-ec2-instances-are-supported"></a>EC2 örneklerim için hangi işletim sistemleri destekleniyor?

AWS makineleri için Azure yaya otomatik olarak ekleme için desteklenen işletim sistemi

- Ubuntu 16,04-SSI Aracısı varsayılan olarak önceden yüklenir
- Ubuntu 18,04-SSI Aracısı varsayılan olarak önceden yüklenir
- Windows Server-SSI Aracısı varsayılan olarak önceden yüklenir
- CentOS Linux 7 – SSI el ile yüklenmeli veya ayrı olarak eklenmelidir
- SUSE Linux Enterprise Server (SLES) 15 (x64)-SSI el ile veya eklendi olarak yüklenmelidir
- Red Hat Enterprise Linux (RHEL) 7 (x64)-SSM el ile veya eklendi olarak yüklenmelidir


## <a name="next-steps"></a>Sonraki adımlar

AWS hesabınızı bağlamak, Azure Güvenlik Merkezi 'nde bulunan çok bulut deneyiminin bir parçasıdır. İlgili bilgiler için şu sayfaya bakın:

- [GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-gcp.md)
