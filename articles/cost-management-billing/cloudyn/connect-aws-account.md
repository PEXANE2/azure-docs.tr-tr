---
title: Azure'da Cloudyn'e Amazon Web Services hesabı bağlama
description: Maliyet ve kullanım verilerini Cloudyn raporlarında görüntülemek için bir Amazon Web Services hesabı bağlayın.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: de0fd1a27ca97545ccdd534908eb802993514017
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690179"
---
# <a name="connect-an-amazon-web-services-account"></a>Amazon Web Services hesabı bağlama

Amazon Web Services (AWS) hesabınızı Cloudyn'e bağlamak için kullanabileceğiniz iki seçenek vardır. Bir IAM rolü ile veya salt okunur IAM kullanıcı hesabı ile bağlayabilirsiniz. Güvenilen varlıklara tanımlı izinlerle erişim yetkisi vermenizi sağladığı için IAM rolü önerilir. IAM rolü uzun süreli erişim anahtarları paylaşmanızı gerektirmez. Bir AWS hesabını Cloudyn'e bağladıktan sonra maliyet ve kullanım verilerini Cloudyn raporlarında görüntüleyebilirsiniz. Bu belgede iki seçenek için de gerekli yönergeler verilmiştir.

AWS IAM kimlikleri hakkında daha fazla bilgi için bkz. [Kimlikler (Kullanıcılar, Gruplar ve Roller)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Ayrıca AWS ayrıntılı faturalama raporlarını etkinleştirmeniz ve bilgileri bir AWS Simple Storage Service (S3) demetinde depolamanız gerekir. Ayrıntılı faturalama raporları, etiketlere ve kaynak b ilgilerine sahip saatlik faturalama ücretlerini kapsar. Raporların depolanması, Cloudyn'in bunları demetinizden alarak raporlarında göstermesini sağlar.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]
## <a name="aws-role-based-access"></a>AWS rol tabanlı erişimi

Aşağıdaki bölümlerde, Cloudyn için erişim sağlama amacıyla salt okunur IAM rolü oluşturma adımları listelenmiştir.

### <a name="get-your-cloudyn-account-external-id"></a>Cloudyn hesabınızın dış kimliğini alma

İlk adım, Cloudyn portalından benzersiz bağlantı parolasını almaktır. Bu parola, AWS'de **Dış Kimlik** olarak kullanılır.

1. Cloudyn portalını Azure portalından açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com) sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin.
3. Hesap Yönetimi bölümünde **AWS Hesapları** sekmesini seçip **Yeni ekle +** öğesine tıklayın.
4. **AWS Hesabı Ekle** iletişim kutusunda **Dış Kimliği** kopyalayın ve sonraki bölümlerde yer alan AWS Rolü oluşturma adımları için saklayın. Dış Kimlik, hesabınıza özgü bir değerdir. Örneğin aşağıdaki görüntüde Dış Kimlik, _Contoso_ ve arkasından gelen telefon numarasıdır. Sizin kimliğiniz farklıdır.  
    ![AWS Hesabı Ekle kutusunda gösterilen dış kimlik](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS salt okunur rol tabanlı erişimi ekleme

1. [https://console.aws.amazon.com/iam/home](https://console.aws.amazon.com/iam/home) adresinden AWS konsolunda oturum açın ve **Roller**'i seçin.
2. **Rol Oluştur**'a tıklayın ve **Başka bir AWS hesabı**'nı seçin.
3. **Hesap kimliği** kutusuna `432263259397` değerini yapıştırın. Bu Hesap Kimliği, AWS tarafından Cloudyn hizmetine atanmış olan Cloudyn veri toplayıcısı hesabıdır. Gösterilen Hesap Kimliği olduğu şekilde kullanın.
4. **Seçenekler**'in yanında **Dış kimlik gerektir**'i seçin. Daha önce, Cloudyn'deki **Dış Kimlik** alanından kopyaladığınız benzersiz değeri yapıştırın. Ardından **İleri: İzinler**'e tıklayın.  
    ![Rol oluştur sayfasına Cloudyn'den alınan Dış Kimlik değerini yapıştırın](./media/connect-aws-account/create-role01.png)
5. **İzin ilkeleri ekle** bölümündeki **İlke türü** filtre kutusunda `ReadOnlyAccess` yazın, **ReadOnlyAccess** girişini seçin ve **İleri: İncele**'ye tıklayın.  
    ![İlke adları listesinde salt okunur erişimini seçin](./media/connect-aws-account/readonlyaccess.png)
6. İnceleme sayfasında seçimlerinizin doğru olduğundan emin olun ve bir **Rol adı** yazın. Örneğin, *Azure-Cost-Mgt*. **Rol açıklaması** girin. Örneğin, _Cloudyn için rol ataması_. Ardından **Rolü oluştur**'a tıklayın.
7. **Roller** listesinde oluşturduğunuz role tıklayın ve Özet sayfasından **Rol ARN'si** değerini kopyalayın. Rol ARN'si (Amazon Kaynak Adı) değerini daha sonra yapılandırmanızı Cloudyn'e kaydederken kullanacaksınız.  
    ![Özet sayfasındaki Rol ARN'sini kopyalayın](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cloudyn"></a>Cloudyn'de AWS IAM rol erişimini yapılandırma

1. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com/ sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin.
3. Hesap Yönetimi bölümünde **AWS Hesapları** sekmesini seçip **Yeni ekle +** öğesine tıklayın.
4. **Hesap Adı** alanına bir hesap adı yazın.
5. **Erişim Türü**'nün yanında **IAM Rolü**'nü seçin.
6. **Rol ARN'si** alanına önceden kopyaladığınız değeri yapıştırın ve **Kaydet**'e tıklayın.  
    ![Rol ARN'sini AWS Hesabı Ekle kutusuna yapıştırın](./media/connect-aws-account/add-aws-account-box.png)


AWS hesabınız hesap listesinde görünür. Listelenen **Sahip Kimliği**, Rol ARN'si değeriyle eşleşir. **Hesap Durumunuzun** yanında Cloudyn'in AWS hesabınıza erişebildiğini gösteren yeşil renkli bir onay işareti bulunmalıdır. Ayrıntılı AWS faturalarını etkinleştirene kadar birleştirme durumunuz **Tek başına** olarak görüntülenir.

![Hesap Yönetimi sayfasında gösterilen AWS hesabı durumu](./media/connect-aws-account/aws-account-status01.png)

Cloudyn verileri toplamaya ve raporları doldurmaya başlar. Sonraki adım için [ayrıntılı AWS faturalandırmasını etkinleştirin](#enable-detailed-aws-billing).


## <a name="aws-user-based-access"></a>AWS kullanıcı tabanlı erişimi

Aşağıdaki bölümlerde, Cloudyn için erişim sağlama amacıyla salt okunur kullanıcı oluşturma adımları listelenmiştir.

### <a name="add-aws-read-only-user-based-access"></a>AWS salt okunur kullanıcı tabanlı erişimi ekleme

1. [https://console.aws.amazon.com/iam/home](https://console.aws.amazon.com/iam/home) adresinden AWS konsolunda oturum açın ve **Kullanıcılar**'ı seçin.
2. **Kullanıcı Ekle**'ye tıklayın.
3. **Kullanıcı adı** alanına bir kullanıcı adı yazın.
4. **Erişim türü** için **Programlı erişim**'i seçin ve **İleri: İzinler**' e tıklayın.  
    ![Kullanıcı ekle sayfasında bir kullanıcı adı girin](./media/connect-aws-account/add-user01.png)
5. İzinler için **Mevcut ilkeleri doğrudan ekle**'yi seçin.
6. **İzin ilkeleri ekle** bölümündeki **İlke türü** filtre kutusunda `ReadOnlyAccess` yazın, **ReadOnlyAccess** girişini seçin ve **İleri: İncele**'ye tıklayın.  
    ![Kullanıcı izinlerini ayarlamak için ReadOnlyAccess'i seçin](./media/connect-aws-account/set-permission-for-user.png)
7. İnceleme sayfasında seçimlerinizin doğru olduğundan emin olun ve **Kullanıcı oluştur**'a tıklayın.
8. Tamamlanma sayfasında Erişim anahtarı kimliğiniz ve Gizli dizi erişim anahtarınız gösterilir. Cloudyn'de kaydı yapılandırmak için bu bilgileri kullanmanız gerekir.
9. **.csv dosyasını indir**'e tıklayıp credentials.csv dosyasını güvenli bir konuma kaydedin.  
    ![Kimlik bilgilerini kaydetmek için .csv dosyasını indir'e tıklayın](./media/connect-aws-account/download-csv.png)

### <a name="configure-aws-iam-user-based-access-in-cloudyn"></a>Cloudyn'de AWS IAM kullanıcı tabanlı erişimi yapılandırma

1. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com/ sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin.
3. Hesap Yönetimi bölümünde **AWS Hesapları** sekmesini seçip **Yeni ekle +** öğesine tıklayın.
4. **Hesap Adı** alanında bir hesap adı yazın.
5. **Erişim Türü**'nün yanında **IAM Kullanıcısı**'nı seçin.
6. **Erişim Anahtarı** bölümünde credentials.csv dosyasındaki **Erişim anahtarı kimliği** değerini yapıştırın.
7. **Gizli Anahtar** bölümünde credentials.csv dosyasındaki **Gizli erişim anahtarı** değerini yapıştırın ve **Kaydet**'e tıklayın.  

AWS hesabınız hesap listesinde görünür. **Hesap Durumu** alanında yeşil onay işareti görünmelidir.

Cloudyn verileri toplamaya ve raporları doldurmaya başlar. Sonraki adım için [ayrıntılı AWS faturalandırmasını etkinleştirin](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Ayrıntılı AWS faturalandırmasını etkinleştirme

AWS Rol ARN'sini almak için aşağıdaki adımları izleyin. Bir faturalama demetine okuma izni vermek için Rol ARN'sini kullanırsınız.

1. [https://console.aws.amazon.com](https://console.aws.amazon.com) adresinden AWS konsolunda oturum açın ve **Hizmetler**'i seçin.
2. Hizmet arama kutusuna *IAM* yazın ve sonuçlardan seçin.
3. Sol taraftaki menüden **Roller**'i seçin.
4. Rol listesinde Cloudyn erişimi için oluşturduğunuz rolü seçin.
5. Rol Özeti sayfasında, **Rol ARN'sini** tıklayarak kopyalayın. Role ARN'sini sonraki adımlar için kaydedin.

### <a name="create-an-s3-bucket"></a>S3 demeti oluşturma

Ayrıntılı faturalandırma bilgilerini depolamak için bir S3 demeti oluşturun.

1. [https://console.aws.amazon.com](https://console.aws.amazon.com) adresinden AWS konsolunda oturum açın ve **Hizmetler**'i seçin.
2. Hizmet arama kutusuna *S3* yazın ve sonuçlardan **S3**'ü seçin.
3. Amazon S3 sayfasında **Demet oluştur**'a tıklayın.
4. Demet oluşturma sihirbazında Demek için bir ad ve Bölge seçip **İleri**'ye tıklayın.  
    ![Demet oluştur sayfasındaki örnek bilgiler](./media/connect-aws-account/create-bucket.png)
5. **Özellikleri ayarla** sayfasında varsayılan değerleri bırakın ve **İleri**'ye tıklayın.
6. İnceleme sayfasında, **Demet oluştur**'a tıklayın. Demet listeniz görüntülenir.
7. Oluşturduğunuz demete tıklayıp **İzinler** sekmesini ve ardından **Demet İlkesi**'ni seçin. Demet ilkesi düzenleyicisi açılır.
8. Aşağıdaki JSON örneğini kopyalayın ve Demet ilkesi düzenleyicisine yapıştırın.
   - `<BillingBucketName>` değerini, S3 demetinizin adıyla değiştirin.
   - `<ReadOnlyUserOrRole>` değerini daha önce kopyaladığınız Rol veya Kullanıcı ARN'si ile değiştirin.

   ```json
   {
      "Version": "2012-10-17",
      "Id": "Policy1426774604000",
      "Statement": [
          {
              "Sid": "Stmt1426774604000",
              "Effect": "Allow",
              "Principal": {
                  "AWS": "arn:aws:iam::386209384616:root"
              },
              "Action": [
                  "s3:GetBucketAcl",
                  "s3:GetBucketPolicy"
              ],
              "Resource": "arn:aws:s3:::<BillingBucketName>"
          },
          {
              "Sid": "Stmt1426774604001",
              "Effect": "Allow",
              "Principal": {
                  "AWS": "arn:aws:iam::386209384616:root"
              },
              "Action": "s3:PutObject",
              "Resource": "arn:aws:s3:::<BillingBucketName>/*"
          },
          {
              "Sid": "Stmt1426774604002",
              "Effect": "Allow",
              "Principal": {
                  "AWS": "<ReadOnlyUserOrRole>"
              },
              "Action": [
                  "s3:List*",
                  "s3:Get*"
              ],
              "Resource": "arn:aws:s3:::<BillingBucketName>/*"
          }
      ]
   }
   ```

9. **Kaydet**’e tıklayın.  
    ![Demet ilke düzenleyicisinde Kaydet'e tıklayın](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>AWS faturalandırma raporlarını etkinleştirme

S3 demeti oluşturup yapılandırdıktan sonra AWS konsolundaki [Faturalandırma Tercihleri](https://console.aws.amazon.com/billing/home?#/preference)'ne gidin.

1. Tercihler sayfasında, **Faturalandırma Raporları Al**'ı seçin.
2. **Faturalandırma Raporları Al** bölümünde oluşturduğunuz demetin adını girin ve ardından **Doğrula**'ya tıklayın.  
3. Dört rapor ayrıntı düzeyi seçeneğini de belirleyin ve ardından **Tercihleri kaydet**'e tıklayın.  
    ![Raporları etkinleştirmek için ayrıntı düzeyi seçin](./media/connect-aws-account/enable-reports.png)

Ayrıntılı faturalandırma etkinleştirildikten sonra Cloudyn, S3 demetinizdeki ayrıntılı fatura bilgilerini alır ve raporlara ekler. Ayrıntılı faturalandırma verilerinin Cloudyn konsolunda görünmesi 24 saate kadar sürebilir. Ayrıntılı faturalandırma verileri kullanılabilir durumda olduğunda hesap birleştirme durumunuz **Birleştirildi** olarak görünür. Hesap durumu **Tamamlandı** olarak görünür.

![AWS Hesapları sekmesinde gösterilen birleştirme durumu](./media/connect-aws-account/consolidated-status.png)

İyileştirme raporlarının bazılarının doğru öneriler için yeterli veri örneği boyutuna ulaşması için birkaç günlük veri gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi edinmek isterseniz Cloudyn için [Kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md) öğreticisine geçin.
