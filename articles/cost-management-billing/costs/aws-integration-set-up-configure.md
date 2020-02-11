---
title: Azure Maliyet Yönetimi ile AWS tümleştirmesini ayarlama
description: Bu makale, AWS Maliyet ve Kullanım raporu ile Azure Maliyet Yönetimi tümleştirmesini ayarlama ve yapılandırma adımlarında size yol gösterecektir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 5dbc6ca836c8d1c8b717fd4bf23eab5aa360a288
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "75988701"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>AWS Maliyet ve Kullanım raporu tümleştirmesini ayarlama ve yapılandırma

Amazon Web Services (AWS) Maliyet ve Kullanım raporu (CUR) tümleştirmesi sayesinde AWS harcamalarınızı Azure Maliyet Yönetimi ile izleyebilir ve denetleyebilirsiniz. Bu tümleştirme, hem Azure hem de AWS harcamalarınızı Azure portalından izlemenizi ve denetlemenizi sağlar. Bu makalede maliyetleri analiz etmek ve bütçeleri gözden geçirmek için Azure Maliyet Yönetimi'ni kullanabilmeniz için tamamlamanız gereken tümleştirme ve yapılandırma adımları anlatılmaktadır.

Maliyet Yönetimi, rapor tanımlarını almak ve rapor GZIP CSV dosyalarını indirmek için AWS erişimi kimlik bilgilerinizi kullanarak bir S3 demetinde depolanan AWS Maliyet ve Kullanım raporunuzu işler.

## <a name="create-a-cost-and-usage-report-in-aws"></a>AWS'de Maliyet ve Kullanım raporu oluşturma

Maliyet ve Kullanım raporu kullanmak, AWS maliyetlerini toplamak ve işlemek için AWS tarafından önerilen yöntemdir. Daha fazla bilgi için bkz. [AWS Maliyet ve Kullanım Raporu](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html).

Aşağıdaki adımları izleyerek bir Maliyet ve Kullanım raporu oluşturmak için AWS'deki Faturalandırma ve Maliyet Yönetimi konsolunuzun **Maliyet ve Kullanım Raporları** sayfasını kullanın:

1. AWS Yönetim Konsolunda oturum açıp [Faturalandırma ve Maliyet Yönetimi konsolunu](https://console.aws.amazon.com/billing) açın.
2. Gezinti bölmesinde **Maliyet ve Kullanım Raporları**'nı seçin.
3. **Rapor oluştur**’u seçin.
4. **Rapor adı** alanına raporunuz için bir ad girin.
5. **Ek rapor ayrıntıları** bölümünde **Kaynak kimliklerini dahil et**'i seçin.
6. **Veri yenileme ayarları** bölümünde faturanız tamamlandıktan sonra AWS tarafından hesabınızda para iadesi, kredi veya destek ücreti uygulanması halinde AWS Maliyet ve Kullanım raporunun yenilenmesini isteyip istemediğinizi seçin. Rapor yenilendiğinde Amazon S3'e yeni bir rapor yüklenir. Bu ayarı seçili olarak bırakmanız önerilir.
7. **İleri**’yi seçin.
8. **S3 demeti** için **Yapılandır**'ı seçin.
9. S3 Demetini Yapılandır iletişim kutusunda aşağıdakilerden birini yapın:
    1. Açılan listeden bir demet seçin ve **İleri**'yi seçin.
    2. Demet adı ve yeni demeti oluşturmak istediğiniz Bölgeyi yazıp **İleri**'yi seçin.
10. **Bu ilkenin doğru olduğunu onayladım**'ı seçip **Kaydet**'e tıklayın.
11. (İsteğe bağlı) Rapor yolu ön eki alanına raporunuzun adına eklenmesini istediğiniz rapor yolu ön ekini girin.
Ön ek belirtmezseniz varsayılan ön ek rapor için belirttiğiniz ad olur. Tarih aralığı `/report-name/date-range/` biçimindedir.
12. **Zaman birimi** alanında **Her saat** girişini seçin.
13. **Rapor sürümü oluşturma** alanında raporunuzun yeni sürümlerinin eski sürümün üzerinde yazması veya ek yeni rapor oluşturulması yönündeki tercihinizi belirtin.
14. **Şunun için veri tümleştirmesini etkinleştir:** alanında seçim yapmanıza gerek yoktur.
15. **Sıkıştırma** bölümünde **GZIP**'i seçin.
16. **İleri**’yi seçin.
17. Raporunuzun ayarlarını inceledikten sonra **Gözden geçir ve tamamla**'yı seçin.

    Rapor adını not edin. Sonraki adımlarda kullanacaksınız.

AWS'nin raporları Amazon S3 demetinize göndermeye başlaması 24 saat sürebilir. Teslim işlemi başladıktan sonra AWS, AWS Maliyet ve Kullanım raporu dosyalarını her gün en az bir kez güncelleştirir. Teslimat işleminin başlamasını beklemeden AWS ortamınızı yapılandırmaya devam edebilirsiniz.

## <a name="create-a-role-and-policy-in-aws"></a>AWS'de rol ve ilke oluşturma

Azure Maliyet Yönetimi, Maliyet ve Kullanım raporunun bulunduğu S3 demetine günde birkaç kez erişir. Hizmet, yeni verileri denetleyebilmek için kimlik bilgilerine ihtiyaç duyar. Maliyet Yönetimi'ne erişim izni vermek için AWS'de bir rol ve ilke oluşturmanız gerekir.

Maliyet Yönetimi'nde AWS hesabına rol tabanlı erişin sağlamak için gerekli rol, AWS konsolundan oluşturulur. AWS konsolunda _rol ARN_ ve _dış kimlik_ değerlerini almanız gerekir. Bunları daha sonra Maliyet Yönetimi'nin **AWS bağlayıcısı oluşturma** sayfasında kullanacaksınız.

Yeni Rol Oluştur sihirbazını kullanın:

1. AWS konsolunuzda oturum açın ve **Hizmetler**'i seçin.
2. Hizmetler listesinde **IAM** girişini seçin.
3. **Roller**'i ve ardından **Rol Oluştur**'u seçin.
4. Bir sonraki sayfada **Başka bir AWS hesabı**'nı seçin.
5. **Hesap Kimliği** alanına **432263259397** girin.
6. **Seçenekler** bölümünde **Dış kimlik gerekir (Bu rolün üçüncü tarafın kullanımında olacağı durumlarda en iyi yöntemdir)** girişini seçin.
7. **Dış Kimlik** bölümünde AWS rolü ile Azure Maliyet Yönetimi arasındaki paylaşılan geçiş kodu olan dış kimliği girin. Bu dış kimlik, Maliyet Yönetimi'ndeki **Yeni Bağlayıcı** sayfasında da kullanılır. Microsoft, dış kimliği girerken güçlü bir geçiş kodu ilkesi kullanmanızı önerir.

    > [!NOTE]
    > **MFA Gerektir** seçimini değiştirmeyin. İşaretlenmemiş şekilde kalmalıdır.
8. Şunu seçin: **İleri: İzinler**.
9. **İlke oluştur**'u seçin. Yeni bir tarayıcı sekmesi açılır. İlkeler burada oluşturulur.
10. **Bir hizmet seçin**'i seçin.

Maliyet ve Kullanım raporu için izinleri yapılandırın:

1. **Maliyet ve Kullanım Raporu** yazın.
2. **Erişim düzeyi** > **Okuma** > **DescribeReportDefinitions** yolunu izleyin. Bu adım, Maliyet Yönetimi'nin CUR raporlarının nasıl tanımlandığını okumasını ve rapor tanımı önkoşulunu karşılayıp karşılamadığını belirlemesini sağlar.
3. **Ek izin ekle**'yi seçin.

S3 demetiniz ve nesneleriniz için izinleri yapılandırın:

1. **Bir hizmet seçin**'i seçin.
2. **S3** yazın.
3. **Erişim düzeyi** > **Listeleme** > **ListBucket** yolunu izleyin. Bu eylem, S3 Demetinin içindeki nesnelerin listesini alır.
4. **Erişim düzeyi** > **Okuma** > **GetObject** yolunu izleyin. Bu eylem faturalandırma dosyalarının indirilmesini sağlar.
5. **Kaynaklar**'ı seçin.
6. **Demet - ARN Ekle**'yi seçin.
7. **Demet adı** alanına CUR dosyalarını depolamak için kullanılan demeti girin.
8. **Nesne - ARN Ekle**'yi seçin.
9. **Demet adı** alanına CUR dosyalarını depolamak için kullanılan demeti girin.
10. **Nesne adı** bölümünde **Herhangi biri**'ni seçin.
11. **Ek izin ekle**'yi seçin.

Maliyet Gezgini için gerekli izinleri yapılandırın:

1. **Bir hizmet seçin**'i seçin.
2. **Maliyet Gezgini Hizmeti** yazın.
3. **Tüm Maliyet Gezgini Hizmeti eylemleri (ce:\*)** girişini seçin. Bu eylem, koleksiyonun doğru olup olmadığını doğrular.
4. **Ek izin ekle**'yi seçin.

AWS Kuruluşları için izin ekleme:

1. **Kuruluşlar**'a girin.
2. **Erişim düzeyi** > **Listeleme** > **ListAccounts** yolunu izleyin. Bu eylem, hesap adlarını alır.
3. **Gözden Geçirme İlkesi** alanında yeni ilke için bir ad girin. Doğru bilgileri girdiğinizden emin olun ve ardından **İlke Oluştur**'u seçin.
4. Önceki sekmeye geri dönün ve tarayıcınızdaki Web sayfasını yenileyin. Arama çubuğunda yeni ilkenizi arayın.
5. Şunu seçin: **İleri: Gözden Geçir**.
6. Yeni rolün adını girin. Doğru bilgileri girdiğinizden emin olun ve ardından **Rol Oluştur**'u seçin.

    Yukarıdaki adımlarda rolü oluştururken kullandığınız rol ARN'si ve dış kimlik değerlerini not edin. Bunları daha sonra Azure Maliyet Yönetimi bağlayıcısını ayarlarken kullanacaksınız.

İlkenin JSON kodu aşağıdaki örneğe benzemelidir. _bucketname_ yerine S3 demetinizin adını yazın.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Azure'da yeni bir AWS bağlayıcısı ayarlama

Bir AWS bağlayıcısı oluşturmak ve AWS maliyetlerinizi izlemeye başlamak için aşağıdaki bilgileri kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi**'ne gidin.
3. **Ayarlar**'ın altında **Bulut bağlayıcıları (Önizleme)** girişini seçin.  
    ![Bulut bağlayıcıları (Önizleme) ayarını gösteren örnek](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Bağlayıcı oluşturmak için sayfanın üst kısmındaki **+ Ekle**'yi seçin.
5. **AWS bağlayıcısı oluşturma** sayfasının **Görünen ad** alanında bağlayıcınız için bir ad girin.  
    ![AWS bağlayıcısı oluşturma sayfasının örneği](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. İsterseniz varsayılan yönetim grubunu seçebilirsiniz. Bulunan tüm bağlı hesaplar orada depolanır. Bunu daha sonra da ayarlayabilirsiniz.
7. Önizleme sona erdikten sonra bağlayıcınızın çalışmaya devam etmesi için **Faturalama** bölümünde **Genel kullanılabilirlikte otomatik olarak %1 ücretlendir** seçeneğini belirtin. Otomatik seçeneğini belirtirseniz bir faturalandırma aboneliği seçmeniz gerekir.
8. **Rol ARN'si** için AWS'de rolü ayarlarken kullandığınız değeri girin.
9. **Dış Kimlik** için AWS'de rolü ayarlarken kullandığınız değeri girin.
10. **Rapor adı** için AWS'de oluşturduğunuz adı girin.
11. **İleri**’yi ve ardından **Oluştur**’u seçin.

Yeni AWS kapsamlarının, AWS birleştirilmiş hesabının, AWS bağlı hesaplarının ve maliyet verilerinin görünmesi birkaç saat sürebilir.

Bağlayıcıyı oluşturduktan sonra, erişim denetimi atamanızı öneririz. Kullanıcılara yeni bulunan kapsamlar için izinler atanır: AWS birleştirilmiş hesabı ve AWS bağlı hesapları. Bağlayıcıyı oluşturan kullanıcı bağlayıcının, birleştirilmiş hesabın ve bağlı tüm hesapların sahibi olur.

Bulma işleminden sonra kullanıcılara bağlayıcı izinlerinin atanması, bu izinleri mevcut AWS kapsamlarına atamaz. Bunun yerine yalnızca yeni bağlı hesaplara izin atanır.

## <a name="take-additional-steps"></a>Ek adımları gerçekleştirin

- Henüz yapmadıysanız [yönetim gruplarını ayarlayın](../../governance/management-groups/overview.md#initial-setup-of-management-groups).
- Kapsam seçiciye yeni kapsamların ayarlandığından emin olun. En son verileri görüntülemek için **Yenile**'yi seçin.
- **Bulut bağlayıcıları** sayfasından bağlayıcınızı seçin ve **Faturalama hesabına gidin**'i seçerek bağlı hesabı yönetim gruplarına atayın.

## <a name="manage-cloud-connectors"></a>Bulut bağlayıcılarını yönetme

**Bulut bağlayıcıları** sayfasında bir bağlayıcı seçtiğinizde şunları yapabilirsiniz:

- **Faturalama hesabına gidin**'i seçerek birleştirilmiş AWS hesabıyla ilgili bilgileri görüntüleyebilirsiniz.
- Bağlayıcının rol atamasını yönetmek için **Erişim Denetimi**'ni seçebilirsiniz.
- Bağlayıcıyı güncelleştirmek için **Düzenle**'yi seçebilirsiniz. Rol ARN'sinde göründüğünden AWS hesap numarasını değiştiremezsiniz. Ancak yeni bir bağlayıcı oluşturabilirsiniz.
- Maliyet Yönetimi'nin bağlayıcı ayarlarını kullanarak verileri toplayabildiğini doğrulamak için doğrulama testini yeniden çalıştırmak üzere **Doğrula**'yı seçin.

![Oluşturulan AWS bağlayıcılarının örnek listesi](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="set-up-azure-management-groups"></a>Azure yönetim gruplarını ayarlama

Farklı bulutlardaki sağlayıcı bilgilerini görebileceğiniz tek bir konum oluşturmak için Azure aboneliklerinizle bağlı AWS hesaplarınızı aynı yönetim grubuna ekleyin. Yönetim gruplarıyla Azure ortamınızı yapılandırmadıysanız bkz. [Yönetim gruplarının ilk ayarı](../../governance/management-groups/overview.md#initial-setup-of-management-groups).

Maliyetleri ayırmak istiyorsanız yalnızca bağlı AWS hesaplarını içeren bir yönetim grubu oluşturabilirsiniz.

## <a name="set-up-an-aws-consolidated-account"></a>AWS birleştirilmiş hesabı ayarlama

AWS birleştirilmiş hesabı, birden fazla AWS hesabı için faturalandırma ve ödeme süreçlerini birleştirir. Ayrıca bağlı AWS hesabı olarak görev yapar.

![Birleştirilmiş AWS hesabı için örnek ayrıntılar](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Sayfada şunları yapabilirsiniz:

- **Güncelleştir**'i seçerek bağlı AWS hesaplarını toplu olarak bir yönetim grubuyla güncelleştirebilirsiniz.
- Kapsam için rol atamasını yönetmek üzere **Erişim Denetimi**'ni seçebilirsiniz.

### <a name="permissions-for-an-aws-consolidated-account"></a>AWS birleştirilmiş hesabı için izinler

Varsayılan olarak AWS birleştirilmiş hesabının izinleri, AWS bağlayıcısı izinlerine göre hesap oluşturulduğu anda ayarlanır. Bağlayıcıyı oluşturan kişi sahibi olur.

Erişim düzeyini yönetmek için birleştirilmiş AWS hesabının **Erişim Düzeyi** sayfasını kullanabilirsiniz. Ancak bağlı AWS hesapları birleştirilmiş AWS hesabının izinlerini devralmaz.

## <a name="set-up-an-aws-linked-account"></a>Bağlı AWS hesabı ayarlama

Bağlı AWS hesabı, AWS kaynaklarının oluşturulduğu ve yönetildiği yerdir. Bağlı hesap ayrıca bir güvenlik sınırı olarak da kullanılır.

Bu sayfada şunları yapabilirsiniz:

- **Güncelleştir**'i seçerek bağlı AWS hesabını bir yönetim grubuyla güncelleştirebilirsiniz.
- Kapsam için rol atamasını yönetmek üzere **Erişim Denetimi**'ni seçebilirsiniz.

![Bağlı AWS hesabı sayfası örneği](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="permissions-for-an-aws-linked-account"></a>Bağlı AWS hesabı için izinler

Varsayılan olarak bağlı AWS hesabının izinleri, AWS bağlayıcısı izinlerine göre oluşturulduğu anda ayarlanır. Bağlayıcıyı oluşturan kişi sahibi olur. Erişim düzeyini yönetmek için bağlı AWS hesabının **Erişim Düzeyi** sayfasını kullanabilirsiniz. Bağlı AWS hesapları birleştirilmiş AWS hesabının izinlerini devralmaz.

Bağlı AWS hesapları her zaman ait oldukları yönetim grubundaki izinleri devralır.

## <a name="next-steps"></a>Sonraki adımlar

- AWS Maliyet ve Kullanım raporu tümleştirmesini ayarladığınıza göre [AWS maliyetlerini ve kullanımını yönetme](aws-integration-manage.md) bölümüne geçebilirsiniz.
- Maliyet analizi konusunda bilginiz yoksa [Maliyet analiziyle maliyetleri keşfetme ve analiz etme](quick-acm-cost-analysis.md) hızlı başlangıcına bakın.
- Azure'daki bütçeler konusunda bilginiz yoksa bkz. [Azure bütçesi oluşturma ve yönetme](tutorial-acm-create-budgets.md).
