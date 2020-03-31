---
title: Azure Veri Gezgini için Azure DevOps görevi
description: Bu konuda, bir sürüm ardışık oluşturmak ve dağıtmak öğrenmek
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/05/2019
ms.openlocfilehash: 1e44a7e71858f028b798720c5505eacbfe8c2332
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472052"
---
# <a name="azure-devops-task-for-azure-data-explorer"></a>Azure Veri Gezgini için Azure DevOps Görevi

[Azure DevOps Hizmetleri,](https://azure.microsoft.com/services/devops/) yüksek performanslı boru hatları, ücretsiz özel Git depoları, yapılandırılabilir Kanban panoları ve kapsamlı otomatik ve sürekli test özellikleri gibi geliştirme işbirliği araçları sağlar. [Azure Pipelines,](https://azure.microsoft.com/services/devops/pipelines/) herhangi bir dil, platform ve bulutla çalışan yüksek performanslı ardışık hatlar ile kodunuzu dağıtmak için CI/CD'yi yönetmenize olanak tanıyan bir Azure DevOps özelliğidir.
[Azure Veri Gezgini - Yönetici Komutları,](https://marketplace.visualstudio.com/items?itemName=Azure-Kusto.PublishToADX) sürüm ardışık hatları oluşturmanıza ve veritabanı değişikliklerinizi Azure Veri Gezgini veritabanlarınıza dağıtmanıza olanak tanıyan Azure Ardışık Hatları görevidir. [Visual Studio Marketplace'te](https://marketplace.visualstudio.com/)ücretsiz olarak mevcuttur.

Bu belge, şema değişikliklerinizi veritabanınıza dağıtmak için **Azure Veri Gezgini – Yönetici Komutları** görevinin kullanımına ilişkin basit bir örneği açıklar. Tam CI/CD ardışık lıklar için [Azure DevOps belgelerine](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops#vsts)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Veri Gezgini Küme kurulumu:
    * [Azure Veri Gezgini kümesi ve veritabanı](/azure/data-explorer/create-cluster-database-portal)
    * [Azure AD uygulamasını sağlayarak](/azure/kusto/management/access-control/how-to-provision-aad-app)Azure Etkin Dizin (Azure AD) uygulamasını oluşturun.
    * [Azure Veri Gezgini veritabanı izinlerini yöneterek](/azure/data-explorer/manage-database-permissions)Azure Veri Gezgini veritabanınızda Azure AD Uygulamanıza erişim izni verin.
* Azure DevOps kurulumu:
    * [Ücretsiz bir organizasyon için kaydolun](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)
    * [Kuruluş oluşturma](/azure/devops/organizations/accounts/create-organization?view=azure-devops)
    * [Azure DevOps'lerde proje oluşturma](/azure/devops/organizations/projects/create-project?view=azure-devops)
    * [Git ile Kod](/azure/devops/user-guide/code-with-git?view=azure-devops)

## <a name="create-folders"></a>Klasör oluşturma

Git deponuzda aşağıdaki örnek klasörleri *(İşlevler,* *İlkeler*, *Tablolar)* oluşturun. Dosyaları [buradan](https://github.com/Azure/azure-kusto-docs-samples/tree/master/DevOps_release_pipeline) aşağıda görüldüğü gibi ilgili klasörlere kopyalayın ve değişiklikleri gerçekleştirin. Örnek dosyalar aşağıdaki iş akışını yürütmek için sağlanır.

![Klasör oluşturma](media/devops/create-folders.png)

> [!TIP]
> Kendi iş akışınızı oluştururken, kod unuzu iktidara getirmenizi öneririz. Örneğin, [.create tablosu](/azure/kusto/management/create-table-command)yerine [.create-merge tablosunu](/azure/kusto/management/create-table-command#create-merge-table) kullanın ve [.create](/azure/kusto/management/create-function) işlevi yerine [.create-or-alter](/azure/kusto/management/create-alter-function) işlevini kullanın.

## <a name="create-a-release-pipeline"></a>Yayın işlem hattı oluşturma

1. [Azure DevOps kuruluşunuzda](https://dev.azure.com/)oturum açın.
1. Sol menüden **Boru Hatları** > **Bültenleri'ni** seçin ve **Yeni ardışık hatlar'ı**seçin.

    ![Yeni işlem hattı](media/devops/new-pipeline.png)

1. **Yeni sürüm ardışık pencere** açılır. **Ardışık Hatlar** sekmesinde, şablon bölmesini **seç'te** **Boş iş'i**seçin.

     ![Şablon seçin](media/devops/select-template.png)

1. **Aşama** düğmesini seçin. **Aşama** bölmesine, **Sahne adı**ekleyin. Boru hattınızı kaydetmek için **Kaydet'i** seçin.

    ![Sahneyi adlandırın](media/devops/stage-name.png)

1. **Yapı tonu ekle düğmesini** seçin. Yapı **ekle** bölmesinde, kodunuzun bulunduğu depoyu seçin, ilgili bilgileri doldurun ve **Ekle'yi**tıklatın. Boru hattınızı kaydetmek için **Kaydet'i** seçin.

    ![Yapıt ekleme](media/devops/add-artifact.png)

1. **Değişkenler** sekmesinde, görevde kullanılacak **Bitiş Noktası URL'si** için bir değişken oluşturmak için + **Ekle'yi** seçin. Bitiş noktasının **Adını** ve **Değerini** yazın. Boru hattınızı kaydetmek için **Kaydet'i** seçin. 

    ![Değişken oluşturma](media/devops/create-variable.png)

    Endpoint_URL bulmak için Azure portalındaki **Azure Veri Gezgini Kümenizin** genel bakış sayfası, Azure Veri Gezgini kümesi URI'yi içerir. URI'yi aşağıdaki biçimde oluştur. `https://<Azure Data Explorer cluster URI>?DatabaseName=<DBName>`  Örneğin, https:\//kustodocs.westus.kusto.windows.net?DatabaseName=SampleDB

    ![Azure Veri Gezgini kümesi URI](media/devops/adx-cluster-uri.png)

## <a name="create-tasks-to-deploy"></a>Dağıtacak görevler oluşturma

1. **Pipeline** sekmesinde, görev eklemek için **1 iş, 0 görev'i** tıklatın. 

    ![Görev ekleme](media/devops/add-task.png)

1. **Tabloları,** **İşlevlerini**ve **İlkeleri**bu sırada dağıtmak için üç görev oluşturun. 

1. **Görevler** sekmesinde, **+** **Aracı işi**tarafından seçin. **Azure Veri Gezgini** için arama yapın. **Market'te** **Azure Veri Gezgini – Yönetici Komutları** uzantısını yükleyin. Ardından, Azure **Veri Gezgini Komutunu Çalıştır'da** **Ekle'yi** seçin.

     ![Yönetici komutları ekleme](media/devops/add-admin-commands.png)

1. Soldaki **Kusto Komutu'na** tıklayın ve görevi aşağıdaki bilgilerle güncelleyin:
    * **Görüntü adı**: Görevin adı
    * **Dosya yolu**: **Tablolar** görevinde tablo oluşturma dosyaları *Tablo* klasöründe olduğundan */Tables/*.csl belirtiniz.
    * **Endpoint URL**: `EndPoint URL`önceki adımda oluşturulan değişkeni girin.
    * **Hizmet Bitiş Noktasını Kullan'ı** seçin ve + **Yeni'yi**seçin.

    ![Kusto komut görevini güncelleştirme](media/devops/kusto-command-task.png)

1. **Azure Veri Gezgini Ekle hizmet bağlantı** penceresinde aşağıdaki bilgileri tamamlayın:

    |Ayar  |Önerilen değer  |
    |---------|---------|
    |**Bağlantı adı**     |    Bu hizmet bitiş noktasını tanımlamak için bir ad girin     |
    |**Küme Url**    |    Değer, Azure portalındaki Azure Veri Gezgini Kümenizin genel bakış bölümünde bulunabilir | 
    |**Hizmet Müdürü Kimliği**    |    AAD Uygulama Kimliğini girin (ön koşul olarak oluşturulmuştur)     |
    |**Servis Müdürü Uygulama Anahtarı**     |    AAD Uygulama Anahtarını girin (ön koşul olarak oluşturulmuştur)    |
    |**AAD kiracı Kimliği**    |      AAD kiracınızı girin (microsoft.com, contoso.com gibi...)    |

    Bu bağlantı onay kutusunu **kullanmak için tüm ardışık lıklara izin ver'i** seçin. **Tamam'ı**seçin.

    ![Hizmet bağlantısı ekleme](media/devops/add-service-connection.png)

1. *İşlevler* ve *İlkeler* klasörlerinden dosyaları dağıtmak için adımları 1-5'i iki kez daha yineleyin. **Kaydet'i**seçin. **Görevler** sekmesinde, oluşturulan üç göreve bakın: **Tabloları Dağıt,** **İşlevleri Dağıt**ve **İlkeleri Dağıt.**

    ![Tüm klasörleri dağıtma](media/devops/deploy-all-folders.png)

1. Sürüm oluşturmak için **+ Sürüm** > **Oluştur'u** seçin.

    ![Sürüm oluşturma](media/devops/create-release.png)

1. **Günlükler** sekmesinde, dağıtım durumunun başarılı olup olmadığını denetleyin.

    ![Dağıtım başarılı](media/devops/deployment-successful.png)

Üretim öncesi üç görevintikal için bir sürüm ardışık alanı oluşturmayı tamamladınız.