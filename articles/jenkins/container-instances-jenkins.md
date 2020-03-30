---
title: Jenkins konteyner örneği üzerine inşa
description: Azure Kapsayıcı Örnekleri'nde isteğe bağlı iş lerinizi çalıştırmak için Jenkins sunucusunu nasıl yapılandıracaklarınız öğrenin
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 8bb84895fb581053248fbad326ea7b2c8d1873a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617976"
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Azure Kapsayıcı Örnekleri'ni Jenkins yapı aracısı olarak kullanma

Azure Kapsayıcı Örnekleri (ACI), kapsayıcılaştırılmış iş yüklerini çalıştırmak için isteğe bağlı, patlamalı ve yalıtılmış bir ortam sağlar. Bu özellikleri nedeniyle, ACI Jenkins büyük bir ölçekte iş kurmak çalıştırmak için büyük bir platform yapar. Bu makale, aci ile yapılı hedef olarak önceden yapılandırılmış bir Jenkins sunucusunun dağıtımı ve kullanımı üzerinden yürür.

Azure Kapsayıcı Örnekleri hakkında daha fazla bilgi için [Azure Kapsayıcı Örnekleri Hakkında'ya](../container-instances/container-instances-overview.md)bakın.

## <a name="deploy-a-jenkins-server"></a>Jenkins sunucusu dağıtma

1. Azure portalında kaynak **oluştur'u** ve **Jenkins'i**arayın'ı seçin. **Microsoft'un**bir yayımcısıyla Jenkins teklifini seçin ve ardından **Oluştur'u**seçin.

2. **Temel Bilgiler** formuna aşağıdaki bilgileri girin ve ardından **Tamam'ı**seçin.

   - **Adı**: Jenkins konuşlandırmaiçin bir ad girin.
   - **Kullanıcı adı**: Jenkins sanal makinenin yönetici kullanıcısı için bir ad girin.
   - **Kimlik doğrulama türü**: Kimlik doğrulama için bir SSH ortak anahtarı öneririz. Bu seçeneği belirlerseniz, Jenkins sanal makinesinde oturum açmak için kullanılacak bir SSH ortak anahtarına yapıştırın.
   - **Abonelik**: Azure aboneliği seçin.
   - **Kaynak grubu**: Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.
   - **Konum**: Jenkins sunucusu için bir konum seçin.

   ![Jenkins portal dağıtımı için temel ayarlar](./media/container-instances-jenkins/jenkins-portal-01.png)

3. Ek **Ayarlar** formunda aşağıdaki öğeleri tamamlayın:

   - **Boyut**: Jenkins sanal makineniz için uygun boyutlandırma seçeneğini seçin.
   - **VM disk türü**: Jenkins sunucusu için **HDD** (sabit disk sürücüsü) veya **SSD** (katı hal sürücüsü) belirtin.
   - **Sanal ağ**: Varsayılan ayarları değiştirmek istiyorsanız oku seçin.
   - **Alt ağlar**: Oku seçin, bilgileri doğrulayın ve **Tamam'ı**seçin.
   - **Genel IP adresi**: Genel IP adresine özel bir ad vermek, SKU'yu yapılandırmak ve atama yöntemini ayarlamak için oku seçin.
   - **Alan adı etiketi**: Jenkins sanal makinesine tam nitelikli bir URL oluşturmak için bir değer belirtin.
   - **Jenkins sürüm türü**: Seçeneklerden istediğiniz sürüm türünü seçin: **LTS**, **Haftalık yapı**veya Azure **Doğrulandı**.

   ![Jenkins portal dağıtımı için ek ayarlar](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Hizmet temel tümleştirmesi [için, Azure kaynaklarının kimliklerini otomatik](../active-directory/managed-identities-azure-resources/overview.md) olarak yönetmiş olmak için **Otomatik (MSI)** seçeneğini belirleyin, Jenkins örneği için otomatik olarak bir kimlik doğrulama kimliği oluşturun. Kendi hizmet temel kimlik bilgilerinizi sağlamak için **El Kitabı'nı** seçin.

5. Bulut aracıları Jenkins için bulut tabanlı bir platform yapılandırır. Bu makalenin **hatırına, ACI'yi**seçin. ACI bulut aracısı ile, her Jenkins yapı işi bir kapsayıcı örneğinde çalıştırılır.

   ![Jenkins portal dağıtımı için bulut tümleştirme ayarları](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Tümleştirme ayarlarını bitirdikten sonra **Tamam'ı**seçin ve doğrulama özetinde **tamam'ı** yeniden seçin. **Kullanım Koşulları** özetinde **Oluştur'u** seçin. Jenkins sunucusunun dağıtılması birkaç dakika sürer.

## <a name="configure-jenkins"></a>Jenkins’i yapılandırma

1. Azure portalında Jenkins kaynak grubuna göz atın, Jenkins sanal makinesini seçin ve DNS adını not alın.

   ![Jenkins sanal makine hakkında ayrıntılarda DNS adı](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Jenkins VM'nin DNS adına göz atın ve döndürülen SSH dizesini kopyalayın.

   ![SSH dizesi ile Jenkins giriş talimatları](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Geliştirme sisteminizde bir terminal oturumu açın ve son adımdan itibaren SSH dizesini yapıştırın. Jenkins `username` sunucusunu dağıtırken belirttiğiniz kullanıcı adına güncelleştirin.

4. Oturum bağlandıktan sonra, ilk yönetici parolasını almak için aşağıdaki komutu çalıştırın:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. SSH oturumunu ve tüneli çalışır `http://localhost:8080` durumda bırakın ve bir tarayıcıya gidin. İlk yönetici parolasını kutuya yapıştırın ve sonra **Devam et'i**seçin.

   ![Yönetici şifresi için kutu ile "Kilidini Jenkins" ekranı](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Önerilen tüm Jenkins eklentilerini yüklemek için **önerilen eklentileri yükle'yi** seçin.

   !["Önerilen eklentileri yükle" ile "Jenkins'i özelleştir" ekranı](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Bir yönetici kullanıcı hesabı oluşturun. Bu hesap, Jenkins örneğinize giriş yapmak ve bunlarla çalışmak için kullanılır.

   !["İlk Yönetici Kullanıcı Oluştur" ekranı, kimlik bilgileri doldurulmuş](./media/container-instances-jenkins/jenkins-portal-07.png)

8. **Kaydet ve Bitir'i**seçin ve yapılandırmayı tamamlamak için **Jenkins'i kullanmaya başlat'ı** seçin.

Jenkins şimdi yapılandırıldı ve kod oluşturmaya ve dağıtmaya hazır. Bu örnekte, Azure Kapsayıcı Örnekleri'nde Jenkins yapısını göstermek için basit bir Java uygulaması kullanılır.

## <a name="create-a-build-job"></a>Yapı işi oluşturma

Şimdi, Jenkins yapı işi, Jenkins'in Azure kapsayıcısı üzerine inşa sını göstermek için oluşturulur.

1. **Yeni Öğe'yi**seçin, yapı projesine **aci-demo**gibi bir ad verin, **Serbest Stil projesini**seçin ve **Tamam'ı**seçin.

   ![Yapı işinin adı ve proje türlerinin listesi için kutu](./media/container-instances-jenkins/jenkins-new-job.png)

2. **Genel**altında, **bu projenin çalıştırılabildiği yeri Kısıtla'nın** seçildiğinden emin olun. Etiket ifadesi için **linux** girin. Bu yapılandırma, bu yapı işinin ACI bulutu üzerinde çalışan olmasını sağlar.

   ![Yapılandırma ayrıntılarıyla "Genel" sekmesi](./media/container-instances-jenkins/jenkins-job-01.png)

3. **Yapı**altında, **Yapı adımı ekle'yi** seçin ve **Shell'i Yürüt'ün'u**seçin. Komut `echo "aci-demo"` olarak girin.

   ![Yapı adımı için seçimlerle "Oluştur" sekmesi](./media/container-instances-jenkins/jenkins-job-02.png)

5. **Kaydet'i**seçin.

## <a name="run-the-build-job"></a>Yapı işini çalıştırma

Yapı işini sınamak ve Yapı platformu olarak Azure Kapsayıcı Örnekleri'ni gözlemlemek için, el ile bir yapı başlatın.

1. Bir yapı işi başlatmak için **Şimdi Oluştur'u** seçin. İşin başlaması birkaç dakika sürer. Aşağıdaki resme benzer bir durum görmeniz gerekir:

   ![İş durumu yla "Geçmiş Oluşturma" bilgileri](./media/container-instances-jenkins/jenkins-job-status.png)

2. İş çalışırken Azure portalını açın ve Jenkins kaynak grubuna bakın. Bir kapsayıcı örneğinin oluşturulduğunu görmeniz gerekir. Jenkins işi bu olayiçinde çalışıyor.

   ![Kaynak grubunda kapsayıcı örneği](./media/container-instances-jenkins/jenkins-aci.png)

3. Jenkins, yapılandırılan Jenkins uygulayıcısayısından (varsayılan 2) daha fazla iş çalıştırdarken, birden çok kapsayıcı örneği oluşturulur.

   ![Yeni oluşturulan kapsayıcı örnekleri](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Tüm yapı işleri tamamlandıktan sonra, kapsayıcı örnekleri kaldırılır.

   ![Kapsayıcı örnekleri kaldırılmış kaynak grubu](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure App Service'e CI/CD](java-deploy-webapp-tutorial.md)