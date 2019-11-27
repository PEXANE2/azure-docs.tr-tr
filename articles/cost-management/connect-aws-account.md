---
title: Amazon Web Services hesabınız Azure Cloudyn bağlanın | Microsoft Docs
description: Cloudyn raporlarında maliyet ve kullanım verilerini görüntülemek için bir Amazon Web Services hesabına bağlanın.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 676c01a26d67b395340e5b1ed2dacc6b3b824742
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74219733"
---
# <a name="connect-an-amazon-web-services-account"></a>Amazon Web Services hesabına bağlanma

Amazon Web Services (AWS) hesabınızı Cloudyn'e bağlamak için iki seçeneğiniz vardır. IAM rolü veya bir salt okunur IAM kullanıcı hesabı ile bağlanabilirsiniz. IAM rol temsilci erişimi güvenilir varlıklar için tanımlanmış izinlerle izin verdiği için önerilir. IAM rolünü uzun süreli erişim anahtarlarınızı paylaşmak gerektirmez. AWS hesabı Cloudyn'e bağlandıktan sonra maliyet ve kullanım verileri, Cloudyn raporlarında kullanılabilir. Bu belge, her iki çalışma seçeneklerde size yol gösterir.

AWS ıAM kimlikleri hakkında daha fazla bilgi için bkz. [kimlikler (kullanıcılar, gruplar ve roller)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Ayrıca, AWS ayrıntılı sağlayan raporlar faturalama ve bir AWS basit depolama hizmeti (S3 için) demetine bilgileri depolar. Ayrıntılı bir faturalandırma raporlarını saatlik olarak herhangi bir etiket ve kaynak bilgileriyle faturalandırma ücretleri dahildir. Depolama raporları, bunları, alanı almak ve onun raporlarında bilgileri görüntülemek Cloudyn sağlar.


## <a name="aws-role-based-access"></a>AWS rol tabanlı erişim

Aşağıdaki bölümlerde, Cloudyn'e erişim sağlamak için bir salt okunur IAM rol oluşturmak adım adım açıklanmaktadır.

### <a name="get-your-cloudyn-account-external-id"></a>Cloudyn hesap dış kimliği alma

İlk adım, Cloudyn portaldan benzersiz bağlantı parolası almaktır. AWS 'de **dış kimlik**olarak kullanılır.

1. Azure portal Cloudyn portalını açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com) ve oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**' nı seçin.
3. Hesap yönetimi 'nde **AWS hesapları** sekmesini seçin ve ardından **Yeni + Ekle**' ye tıklayın.
4. **AWS hesabı ekle** iletişim kutusunda, **dış kimliği** kopyalayın ve bır sonraki bölümde AWS rolü oluşturma adımları için değeri kaydedin. Dış hesabınıza benzersiz kimliğidir. Aşağıdaki görüntüde, örnek dış KIMLIĞI _contoso_ 'dan sonra bir sayı izler. Kimliğinizi farklıdır.  
    AWS hesabı ekle kutusunda gösterilen dış KIMLIK ![](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS salt okunur rol tabanlı erişim Ekle

1. AWS konsolunda https://console.aws.amazon.com/iam/home ' de oturum açın ve **Roller**' i seçin.
2. **Rol oluştur** ' a tıklayın ve ardından **başka bir AWS hesabı**seçin.
3. **Hesap kimliği** kutusunda `432263259397`yapıştırın. Bu hesap kimliği, Cloudyn hizmete AWS tarafından atanan Cloudyn veri toplayıcı hesabıdır. Tam hesap kimliği kullanın.
4. **Seçenekler**' **ın yanındaki dış kimlik gerektir**' i seçin. Daha önce Cloudyn içindeki **dış kimlik** alanından kopyaladığınız benzersiz değeri yapıştırın. **İleri**' ye tıklayın.  
    Rol oluştur sayfasında Cloudyn 'ten dış KIMLIK ![yapıştırın](./media/connect-aws-account/create-role01.png)
5. **İzin Ilkeleri Ekle**' nin altında, **ilke türü** filtre kutusunda arama ' yı `ReadOnlyAccess`yazın, **readonlyaccess**' ı seçin ve ardından **İleri: gözden geçir**' e tıklayın.  
    ![ilke adları listesinde salt okunurdur erişimi seçin](./media/connect-aws-account/readonlyaccess.png)
6. Inceleme sayfasında, seçimlerinizin doğru olduğundan emin olun ve bir **rol adı**yazın. Örneğin, *Azure-maliyet-yön*. **Rol açıklaması**girin. Örneğin, _Cloudyn Için rol ataması_ve ardından **rol oluştur**' a tıklayın.
7. **Roller** listesinde, oluşturduğunuz role tıklayın ve **rol ARN** değerini Özet sayfasından kopyalayın. Rol ARN (Amazon kaynak adı) değeri, daha sonra yapılandırmanızı Cloudyn'de kaydettiğinizde kullanın.  
    Özet sayfasından ![rolünü kopyalayın](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cloudyn"></a>Cloudyn'de AWS IAM rol erişimini yapılandırma

1. Azure portal Cloudyn portalını açın veya https://azure.cloudyn.com/ ve oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**' nı seçin.
3. Hesap yönetimi 'nde **AWS hesapları** sekmesini seçin ve ardından **Yeni + Ekle**' ye tıklayın.
4. **Hesap adı**alanına hesap için bir ad yazın.
5. **Erişim türü**' nün yanındaki **IAM rolü**' nü seçin.
6. **Rol ARN** alanında, daha önce kopyaladığınız değeri yapıştırın ve **Kaydet**' e tıklayın.  
    ![AWS hesabı ekle kutusunda ARN rolünü yapıştırın](./media/connect-aws-account/add-aws-account-box.png)


AWS hesabınız hesapları listesinde görünür. Listelenen **sahıp kimliği** , rol ARN değeri ile eşleşiyor. **Hesap durumunuz** , Cloudyn 'nin AWS hesabınıza erişebileceğini belirten yeşil bir onay işareti simgesine sahip olmalıdır. Ayrıntılı AWS faturalandırmayı etkinleştirene kadar birleştirme durumunuz **tek başına**görüntülenir.

![Hesap Yönetimi sayfasında gösterilen AWS hesabı durumu](./media/connect-aws-account/aws-account-status01.png)

Cloudyn, veri toplama ve raporları doldurma başlatır. Sonra, [ayrıntılı AWS faturalandırmasını etkinleştirin](#enable-detailed-aws-billing).


## <a name="aws-user-based-access"></a>AWS kullanıcı tabanlı erişim

Aşağıdaki bölümlerde, Cloudyn'e erişim sağlamak için salt okunur bir kullanıcı oluşturmak adım adım açıklanmaktadır.

### <a name="add-aws-read-only-user-based-access"></a>AWS salt okunur kullanıcı tabanlı erişim Ekle

1. AWS konsolunda https://console.aws.amazon.com/iam/home ' de oturum açın ve **Kullanıcılar**' ı seçin.
2. **Kullanıcı Ekle**' ye tıklayın.
3. **Kullanıcı adı** alanına bir Kullanıcı adı yazın.
4. **Erişim türü**için **programlı erişim** ' i seçin ve **İleri**' ye tıklayın.  
    ![Kullanıcı Ekle sayfasında bir Kullanıcı adı girin](./media/connect-aws-account/add-user01.png)
5. İzinler için, **mevcut ilkeleri doğrudan Ekle**' yi seçin.
6. **İzin Ilkeleri Ekle**' nin altında, **ilke türü** filtre kutusunda arama ' yı `ReadOnlyAccess`yazın, **readonlyaccess**' ı seçin ve ardından **İleri: gözden geçir**' e tıklayın.  
    Kullanıcı izinlerini ayarlamak için ReadOnlyAccess ![seçin](./media/connect-aws-account/set-permission-for-user.png)
7. Inceleme sayfasında, seçimlerinizin doğru olduğundan emin olun ve ardından **Kullanıcı oluştur**' a tıklayın.
8. Tam sayfada erişim anahtarı kimliği ve parolası erişim anahtarınızı gösterilir. Cloudyn'de kaydını yapılandırmak için bu bilgileri kullanın.
9. **. Csv dosyasını indir** ' e tıklayın ve kimlik bilgileri. csv dosyasını güvenli bir konuma kaydedin.  
    ![kimlik bilgilerini kaydetmek için. csv ' ye tıklayın](./media/connect-aws-account/download-csv.png)

### <a name="configure-aws-iam-user-based-access-in-cloudyn"></a>AWS IAM kullanıcı tabanlı erişim Cloudyn'de yapılandırın

1. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com/ sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**' nı seçin.
3. Hesap yönetimi 'nde **AWS hesapları** sekmesini seçin ve ardından **Yeni + Ekle**' ye tıklayın.
4. **Hesap adı**için bir hesap adı yazın.
5. **Erişim türü**' nün yanındaki **IAM kullanıcısı**' nı seçin.
6. **Erişim anahtarı**' nda, kimlik bilgileri. csv dosyasından **erişim anahtarı kimliği** değerini yapıştırın.
7. **Gizli anahtar**' da, kimlik bilgileri. csv dosyasından **gizli erişim anahtarı** değerini yapıştırın ve **Kaydet**' e tıklayın.  

AWS hesabınız hesapları listesinde görünür. **Hesap durumunuz** yeşil bir onay işareti simgesine sahip olmalıdır.

Cloudyn, veri toplama ve raporları doldurma başlatır. Sonra, [ayrıntılı AWS faturalandırmasını etkinleştirin](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Ayrıntılı AWS faturalamayı etkinleştirme

AWS rol ARN almak için aşağıdaki adımları kullanın. Rol ARN fatura bir demet için Okuma izinleri vermek için kullanın.

1. https://console.aws.amazon.com adresinden AWS konsolunda oturum açın ve **Hizmetler**' i seçin.
2. Hizmet arama kutusuna *IAM*yazın ve bu seçeneği belirleyin.
3. Sol taraftaki menüden **Roller** ' i seçin.
4. Rolleri listesinde, oluşturduğunuz Cloudyn erişimi için rolü seçin.
5. Rol Özeti sayfasında, **ARN rolünü**kopyalamak için tıklayın. Rol daha sonraki adımlar için kullanışlı tutun.

### <a name="create-an-s3-bucket"></a>Bir S3 demetini oluşturma

Bir S3 demetini ayrıntılı faturalandırma bilgileri depolamak için oluşturduğunuz.

1. https://console.aws.amazon.com adresinden AWS konsolunda oturum açın ve **Hizmetler**' i seçin.
2. Hizmet arama kutusuna *S3*yazın ve **S3**' u seçin.
3. Amazon S3 sayfasında **Demet oluştur**' a tıklayın.
4. Demet oluşturma sihirbazında, bir demet adı ve bölgesi seçin ve ardından **İleri**' ye tıklayın.  
    ![demet oluşturma sayfasına örnek bilgi](./media/connect-aws-account/create-bucket.png)
5. **Özellikleri ayarla** sayfasında, varsayılan değerleri koruyun ve ardından **İleri**' ye tıklayın.
6. Inceleme sayfasında **Demet oluştur**' a tıklayın. Demet listenizde görüntülenir.
7. Oluşturduğunuz sepete tıklayın ve **izinler** sekmesini seçin ve ardından **demet ilkesi**' ni seçin. Demet İlke Düzenleyicisi açılır.
8. Aşağıdaki JSON örneği kopyalayın ve demet İlkesi Düzenleyicisi'nde yapıştırın.
   - `<BillingBucketName>`, S3 demet 'nizin adıyla değiştirin.
   - `<ReadOnlyUserOrRole>`, daha önce kopyaladığınız rol veya Kullanıcı ile değiştirin.

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

9. **Save (Kaydet)** düğmesine tıklayın.  
    ![demet ilke düzenleyicisinde Kaydet ' e tıklayın](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>AWS raporları faturalama etkinleştir

S3 demeti oluşturup yapılandırdıktan sonra AWS konsolundaki [Faturalandırma Tercihleri](https://console.aws.amazon.com/billing/home?#/preference) ' ne gidin.

1. Tercihler sayfasında **faturalandırma raporları al**' ı seçin.
2. **Faturalama raporlarını al**' ın altında, oluşturduğunuz demet 'in adını girin ve ardından **Doğrula**' ya tıklayın.  
3. Dört rapor ayrıntı düzeyi seçeneğini belirleyin ve ardından **tercihleri kaydet**' e tıklayın.  
    Raporları etkinleştirmek için ayrıntı düzeyi ![seçin](./media/connect-aws-account/enable-reports.png)

Cloudyn, S3 demetini ayrıntılı fatura bilgilerini alır ve ayrıntılı bir faturalandırma etkinleştirildikten sonra raporları doldurur. Uygulamanın ayrıntılı faturalama verileri Cloudyn konsolunda görünene kadar en fazla 24 saat sürebilir. Ayrıntılı faturalandırma verileri kullanılabilir olduğunda, hesap birleştirme durumunuz **birleştirilmiş**olarak görünür. Hesap durumu **tamamlandı**olarak görünür.

![AWS hesapları sekmesinde gösterilen birleştirme durumu](./media/connect-aws-account/consolidated-status.png)

Bazı iyileştirme raporlar, birkaç günlük bir yeterli veri örnek boyutu için doğru öneriler almak için veri gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi edinmek için Cloudyn için [kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md) öğreticisine geçin.
