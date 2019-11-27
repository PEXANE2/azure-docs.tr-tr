---
title: Depolama hesaplarını Azure Cloudyn için yapılandırma | Microsoft Docs
description: Bu makalede, Azure depolama hesapları ve AWS depolama demet için Cloudyn nasıl yapılandırılacağını açıklar.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: benshy
ms.custom: secdec18
ms.openlocfilehash: 1ac4442aa5a7e5e4367a03d33169412d37b3f1ea
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74229920"
---
# <a name="configure-storage-accounts-for-cloudyn"></a>Cloudyn depolama hesaplarını yapılandırma

<!--- intent: As a Cloudyn user, I want to configure Cloudyn to use my cloud service provider storage account to store my reports. -->

Cloudyn portalında, Azure depolama veya AWS depolama demet Cloudyn raporlarını kaydedebilirsiniz. Cloudyn portalında raporlarınızı kaydetme ücretsizdir. Ancak, bulut hizmeti sağlayıcısının depolama alanına raporlarınızı kaydetme isteğe bağlıdır ve ek ücret doğurur. Bu makalede, Azure depolama hesapları ve raporlarınızı depolamak için depolama demetler Amazon Web Services (AWS) yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure depolama hesabı ya da bir Amazon depolama aralığı olmalıdır.

Azure depolama hesabınız yoksa, oluşturmanız gerekir. Azure depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).

AWS yoksa basit depolama hizmeti (S3 için) demetine bir oluşturmanız gerekir. S3 demeti oluşturma hakkında daha fazla bilgi için bkz. [demet oluşturma](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Azure depolama hesabınızı yapılandırın

Yapılandırdığınız Cloudyn tarafından kullanılmak üzere Azure depolama oldukça basittir. Depolama hesabı ayrıntılarını toplayın ve bunları Cloudyn portalında kopyalayın.

1. https://portal.azure.com adresinden Azure portalında oturum açın.
2. **Tüm hizmetler**' e tıklayın, **depolama hesapları**' nı seçin, kullanmak istediğiniz depolama hesabına gidin ve sonra hesabı seçin.
3. Depolama Hesabınız sayfasında, **Ayarlar**altında **erişim anahtarları**' na tıklayın.
4. **Depolama hesabı adınızı** ve **Bağlantı dizenizi** KEY1 altına kopyalayın.  
   ![depolama hesabı adını ve bağlantı dizesini kopyalama](./media/storage-accounts/azure-storage-access-keys.png)  
5. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com sayfasına gidip oturum açın.
6. Dişli simgesine tıklayın ve ardından **raporlar depolama yönetimi**' ni seçin.
7. **Yeni Ekle +** ' ye tıklayın ve Microsoft Azure seçili olduğundan emin olun. Azure depolama hesabınızın adını **ad** alanına yapıştırın. **Bağlantı dizenizi** ilgili alana yapıştırın. Bir kapsayıcı adı girin ve ardından **Kaydet**' e tıklayın.  
   Azure depolama hesabı adını ve bağlantı dizesini yeni rapor depolama alanı ekle kutusuna yapıştırmayı ![](./media/storage-accounts/azure-cloudyn-storage.png)

   Yeni Azure rapor depolama giriş depolama hesabı listesinde görüntülenir.  
    ![Yeni Azure rapor depolama giriş listesi](./media/storage-accounts/azure-storage-entry.png)


Raporları, artık Azure depolama alanına kaydedebilirsiniz. Herhangi bir raporda, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Rapor adı ve ardından kendi URL ekleyebilir veya otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet** ' i seçin ve depolama hesabını seçin. Raporu dosya adına eklenmiş bir ön eki girin. CSV veya JSON dosya biçimi seçin ve ardından raporu kaydedin.

## <a name="configure-an-aws-storage-bucket"></a>Bir AWS depolama aralığı yapılandırın

Mevcut AWS kimlik bilgilerini Cloudyn kullanır: kullanıcı veya rol, raporları, demetine kaydetmek için. Cloudyn, erişimi test etmek için, _Check-Bucket-Permission. txt_dosya adı ile sepete küçük bir metin dosyası kaydetmeye çalışır.

Cloudyn rolü ya da kullanıcı PutObject izinle, demet için sağladığınız. Ardından, var olan bir demet kullanın veya raporları kaydetmek için yeni bir tane oluşturun. Son olarak, depolama sınıfı yönetmek, yaşam döngüsü kurallar veya gereksiz dosyaları kaldırın nasıl karar verin.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>AWS kullanıcı veya rol izin atama

Yeni bir ilke oluşturduğunuzda, bir rapor için bir S3 demetini kaydetmek için gereken tam izinleri sağlar.

1. AWS konsolunda oturum açın ve **Hizmetler**' i seçin.
2. Hizmetler listesinden **IAM** ' i seçin.
3. Konsolunun sol tarafındaki **ilkeler** ' i seçin ve ardından **ilke oluştur**' a tıklayın.
4. **JSON** sekmesine tıklayın.
5. Aşağıdaki ilke bir rapor için bir S3 demetini kaydetmenize olanak tanır. Aşağıdaki ilke örneğini kopyalayıp **JSON** sekmesine yapıştırın. &lt;bucketname&gt; değerini demet adınızla değiştirin.

   ```json
   {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
   }
   ```

6. **Ilkeyi gözden geçir**' e tıklayın.  
    örnek bilgileri gösteren AWS JSON ilkesi ![](./media/storage-accounts/aws-policy.png)  
7. İnceleme İlkesi sayfasında, ilke için bir ad yazın. Örneğin, _CloudynSaveReport2S3_.
8. **Ilke oluştur**' a tıklayın.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>İlkeyi bir Cloudyn rolü ya da kullanıcı hesabı ekleme

Yeni ilke eklemek için AWS konsolunu açın ve Cloudyn rolü ya da kullanıcı düzenleyin.

1. AWS konsolunda oturum açın ve **Hizmetler**' i seçin ve ardından hizmetler listesinden **IAM** ' i seçin.
2. Konsolunun sol tarafındaki **rol** veya **Kullanıcı** seçeneklerinden birini belirleyin.

**Roller için:**

  1. Cloudyn rol adınızı tıklatın.
  2. **İzinler** sekmesinde, **ilke Ekle**' ye tıklayın.
  3. Oluşturduğunuz ilkeyi arayın ve seçin ve ardından **Ilke Ekle**' ye tıklayın.
    Cloudyn rolünüze ![örnek ilke eklendi](./media/storage-accounts/aws-attach-policy-role.png)

**Kullanıcılar için:**

1. Cloudyn kullanıcı seçin.
2. **İzinler** sekmesinde **izin Ekle**' ye tıklayın.
3. **Izin verme** bölümünde, **var olan ilkeleri doğrudan Ekle**' yi seçin.
4. Oluşturduğunuz ilkeyi arayın ve seçin ve ardından **İleri: gözden geçir**' e tıklayın.
5. Rol adına izin Ekle sayfasında, **Izin Ekle**' ye tıklayın.  
    Cloudyn Kullanıcı](./media/storage-accounts/aws-attach-policy-user.png) bağlı ![örnek ilke


### <a name="optional-set-permission-with-bucket-policy"></a>İsteğe bağlı: demetine ilkesiyle izni ayarlayın

Bir demet İlkesi'ni kullanarak, S3 demetini üzerinde raporlar oluşturma izni de ayarlayabilirsiniz. Klasik S3 Görünümü'nde:

1. Oluşturun veya mevcut bir aralığı seçin.
2. **İzinler** sekmesini seçin ve ardından **demet ilkesi**' ne tıklayın.
3. Aşağıdaki ilke örneği kopyalayıp yeniden açın. &lt;Bucket\_adı&gt; ve &lt;Cloudyn\_prensibi&gt; demet 'larınızın ARN 'iyle değiştirin. Cloudyn tarafından kullanılan kullanıcı veya rol ARN değiştirin.

   ```
   {
   "Id": "Policy1485775646248",
   "Version": "2012-10-17",
   "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
   ]
   }
   ```

4. Demet İlkesi düzenleyicisinde **Kaydet**' e tıklayın.

### <a name="add-aws-report-storage-to-cloudyn"></a>Cloudyn'e AWS rapor depolama ekleme

1. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve ardından **raporlar depolama yönetimi**' ni seçin.
3. **Yeni Ekle** ' ye tıklayın ve AWS 'nin seçili olduğundan emin olun.
4. Bir hesabı ve depolama aralığı seçin. AWS depolama demet adını otomatik olarak doldurulmuş bileşenidir.  
    ![Örnek bilgiler, yeni bir rapor depolama Kutusu Ekle](./media/storage-accounts/aws-cloudyn-storage.png)  
5. **Kaydet** ' e ve ardından **Tamam**' a tıklayın.

    Yeni AWS rapor depolama giriş depolama hesabı listesinde görüntülenir.  
    ![Depolama giriş show depolama hesabı listesinde yeni AWS rapor](./media/storage-accounts/aws-storage-entry.png)


Raporları, artık Azure depolama alanına kaydedebilirsiniz. Herhangi bir raporda, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Rapor adı ve ardından kendi URL ekleyebilir veya otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet** ' i seçin ve depolama hesabını seçin. Raporu dosya adına eklenmiş bir ön eki girin. CSV veya JSON dosya biçimi seçin ve ardından raporu kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn raporlarının temel yapısı ve işlevleri hakkında bilgi edinmek için [Cloudyn raporlarını](understanding-cost-reports.md) anlayın.
