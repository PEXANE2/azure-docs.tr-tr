---
title: Azure Veri Gezgini için Azure DevOps görevi
description: Bu konu başlığında, bir yayın işlem hattı oluşturup dağıtmayı öğreneceksiniz
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/05/2019
ms.openlocfilehash: 6394d7149bd4e80f0a17a59a6259eedf4c806fd4
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77188170"
---
# <a name="azure-devops-task-for-azure-data-explorer"></a>Azure Veri Gezgini için Azure DevOps görevi

[Azure DevOps Services](https://azure.microsoft.com/services/devops/) yüksek performanslı işlem hatları, ücretsiz özel Git depoları, yapılandırılabilir Kanban panoları ve kapsamlı otomatikleştirilmiş ve sürekli test özellikleri gibi geliştirme işbirliği araçları sağlar. [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) , herhangi bir dil, platform ve bulutla çalışan yüksek performanslı işlem hatları ile kodunuzu DAĞıTMAK üzere CI/CD 'yi yönetmenizi sağlayan bir Azure DevOps özelliğidir.
[Azure Veri Gezgini-yönetici komutları](https://marketplace.visualstudio.com/items?itemName=Azure-Kusto.PublishToADX) , yayın işlem hatları oluşturmanızı ve veritabanı değişikliklerinizi Azure Veri Gezgini veritabanlarına dağıtmanızı sağlayan Azure Pipelines görevidir. [Visual Studio Market](https://marketplace.visualstudio.com/)ücretsiz olarak kullanılabilir.

Bu belgede, şema değişikliklerinizi veritabanınıza dağıtmak için **Azure Veri Gezgini – yönetici komutları** görevinin kullanımıyla ilgili basit bir örnek açıklanmaktadır. Tüm CI/CD işlem hatları için [Azure DevOps belgelerine](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops#vsts)bakın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Veri Gezgini kümesi kurulumu:
    * [Azure Veri Gezgini kümesi ve veritabanı](/azure/data-explorer/create-cluster-database-portal)
    * [Bir Azure AD uygulaması](/azure/kusto/management/access-control/how-to-provision-aad-app)sağlayarak Azure Active Directory (Azure AD) uygulaması oluşturun.
    * Azure [Veri Gezgini veritabanı izinlerini yöneterek](/azure/data-explorer/manage-database-permissions)Azure Veri Gezgini veritabanınıza Azure AD uygulaması erişim izni verin.
* Azure DevOps kurulumu:
    * [Ücretsiz bir kuruluşa kaydolun](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)
    * [Kuruluş oluşturma](/azure/devops/organizations/accounts/create-organization?view=azure-devops)
    * [Azure DevOps 'da proje oluşturma](/azure/devops/organizations/projects/create-project?view=azure-devops)
    * [Git ile kod](/azure/devops/user-guide/code-with-git?view=azure-devops)

## <a name="create-folders"></a>Klasör oluşturma

Git deponuzda aşağıdaki örnek klasörleri (*işlevler*, *ilkeler*, *Tablolar*) oluşturun. Dosyaları [buradan](https://github.com/Azure/azure-kusto-docs-samples/tree/master/DevOps_release_pipeline) aşağıda görüldüğü gibi ilgili klasörlere kopyalayın ve değişiklikleri kaydedin. Örnek dosyalar aşağıdaki iş akışını yürütmek için verilmiştir.

![Klasör oluşturma](media/devops/create-folders.png)

> [!TIP]
> Kendi iş akışınızı oluştururken, kodunuzu ıdempotent yapmanızı öneririz. Örneğin. Create [-merge tablosunu](/azure/kusto/management/create-table-command#create-merge-table) kullanın. Create [Table](/azure/kusto/management/create-table-command)yerine [. Create](/azure/kusto/management/functions#create-function) [-or-alter](/azure/kusto/management/functions#create-or-alter-function) işlevini kullanın.

## <a name="create-a-release-pipeline"></a>Yayın işlem hattı oluşturma

1. [Azure DevOps kuruluşunuzda](https://dev.azure.com/)oturum açın.
1. Sol taraftaki menüden ** > Işlem** **hatları** ' nı seçin ve **Yeni işlem hattı**' nı seçin.

    ![Yeni işlem hattı](media/devops/new-pipeline.png)

1. **Yeni yayın ardışık düzeni** penceresi açılır. İşlem **hatları** sekmesinde, **şablon seçin** bölmesinde **boş iş**' ı seçin.

     ![Şablon seçin](media/devops/select-template.png)

1. **Aşama** düğmesini seçin. **Aşama** bölmesinde, **aşama adını**ekleyin. İşlem hattınızı kaydetmek için **Kaydet** ' i seçin.

    ![Aşamayı adlandırın](media/devops/stage-name.png)

1. **Yapıt Ekle** düğmesini seçin. **Yapıt Ekle** bölmesinde, kodunuzun bulunduğu depoyu seçin, ilgili bilgileri doldurun ve **Ekle**' ye tıklayın. İşlem hattınızı kaydetmek için **Kaydet** ' i seçin.

    ![Yapıt Ekle](media/devops/add-artifact.png)

1. **Değişkenler** sekmesinde **+ Ekle** ' yi seçerek, görevde kullanılacak **uç nokta URL 'si** için bir değişken oluşturun. Uç noktanın **adını** ve **değerini** yazın. İşlem hattınızı kaydetmek için **Kaydet** ' i seçin. 

    ![Değişken Oluştur](media/devops/create-variable.png)

    Endpoint_URL bulmak için, Azure portal **azure Veri Gezgini kümenizin** genel bakış sayfası Azure VERI GEZGINI kümesi URI 'sini içerir. URI 'yi aşağıdaki biçimde oluşturun `https://<Azure Data Explorer cluster URI>?DatabaseName=<DBName>`.  Örneğin, https:\//kustodocs.westus.kusto.Windows.net? DatabaseName = SampleDB

    ![Azure Veri Gezgini küme URI 'SI](media/devops/adx-cluster-uri.png)

## <a name="create-tasks-to-deploy"></a>Dağıtılacak görevleri oluştur

1. İşlem **hattı** sekmesinde, görev eklemek için **1 iş, 0 görev** ' e tıklayın. 

    ![Görev Ekle](media/devops/add-task.png)

1. **Tabloları**, **işlevleri**ve **ilkeleri**bu sırayla dağıtmak için üç görev oluşturun. 

1. **Görevler** sekmesinde, **Aracı işiyle** **+** ' yi seçin. **Azure Veri Gezgini** için arama yapın. **Market**'te **Azure Veri Gezgini-yönetici komutları** uzantısını yükler. Ardından, **Azure Veri Gezgini Çalıştır komutuyla** **Ekle** ' yi seçin.

     ![Yönetici komutları Ekle](media/devops/add-admin-commands.png)

1. Soldaki **kusto komutuna** tıklayın ve aşağıdaki bilgilerle görevi güncelleştirin:
    * **Görünen ad**: görevin adı
    * **Dosya yolu**: **Tablolar** görevinde tablo oluşturma dosyaları *tablo* klasöründe olduğundan */Tables/* . CSL değerini belirtin.
    * **Uç nokta URL 'si**: önceki adımda oluşturulan `EndPoint URL`değişkenini girin.
    * **Hizmet uç noktası kullan** öğesini seçin ve **+ Yeni**' yi seçin.

    ![Kusto komut görevini Güncelleştir](media/devops/kusto-command-task.png)

1. **Azure Veri Gezgini hizmet bağlantısı ekleme** penceresinde aşağıdaki bilgileri doldurun:

    |Ayar  |Önerilen değer  |
    |---------|---------|
    |**Bağlantı adı**     |    Bu hizmet uç noktasını tanımlamak için bir ad girin     |
    |**Küme URL 'Si**    |    Değer, Azure portal Azure Veri Gezgini kümenizin genel bakış bölümünde bulunabilir | 
    |**Hizmet sorumlusu kimliği**    |    AAD uygulama KIMLIĞINI girin (önkoşul olarak oluşturulur)     |
    |**Hizmet sorumlusu uygulama anahtarı**     |    AAD uygulama anahtarını girin (önkoşul olarak oluşturulur)    |
    |**AAD Kiracı kimliği**    |      AAD kiracınızı girin (örneğin, microsoft.com, contoso.com...)    |

    **Tüm işlem hatlarının bu bağlantıyı kullanmasına Izin ver** onay kutusunu seçin. **Tamam**’ı seçin.

    ![Hizmet bağlantısı ekle](media/devops/add-service-connection.png)

1. *İşlevler* ve *ilkeler* klasörlerinden dosyaları dağıtmak için iki kez 1-5 adımı yineleyin. **Kaydet**’i seçin. **Görevler** sekmesinde, oluşturulan üç görev: **tabloları dağıtma**, **işlevleri dağıtma**ve **ilkeleri dağıtma**.

    ![Tüm klasörleri dağıt](media/devops/deploy-all-folders.png)

1. Yayın oluşturmak için **+ yayın** > **Sürüm Oluştur** ' u seçin.

    ![Yayın oluştur](media/devops/create-release.png)

1. **Günlükler** sekmesinde, dağıtım durumunun başarılı olduğunu denetleyin.

    ![Dağıtım başarılı](media/devops/deployment-successful.png)

Önceden üretime üç görevin dağıtılması için bir yayın işlem hattının oluşturulmasını tamamladınız.