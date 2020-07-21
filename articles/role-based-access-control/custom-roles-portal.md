---
title: Azure portal-Azure RBAC kullanarak Azure özel rolleri oluşturma veya güncelleştirme
description: Azure portal ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak Azure özel rolleri oluşturmayı öğrenin. Buna özel rolleri listeleme, oluşturma, güncelleştirme ve silme işlemlerinin nasıl yapılacağı de dahildir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2020
ms.author: rolyon
ms.openlocfilehash: 91d2605dddd6107d09e635969f5e5d98c2a02d60
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511730"
---
# <a name="create-or-update-azure-custom-roles-using-the-azure-portal"></a>Azure portalı kullanarak özel Azure rolleri oluşturma veya güncelleştirme

[Azure yerleşik rolleri](built-in-roles.md) kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi Azure özel rollerinizi de oluşturabilirsiniz. Yerleşik rollerde olduğu gibi, abonelik ve kaynak grubu kapsamları 'nda kullanıcılara, gruplara ve hizmet sorumlularına özel roller atayabilirsiniz. Özel Roller bir Azure Active Directory (Azure AD) dizininde depolanır ve abonelikler arasında paylaşılabilir. Her bir dizin en fazla 5000 özel role sahip olabilir. Özel roller Azure portal, Azure PowerShell, Azure CLı veya REST API kullanılarak oluşturulabilir. Bu makalede, Azure portal kullanarak nasıl özel rol oluşturulacağı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Özel Roller oluşturmak için şunlar gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni

## <a name="step-1-determine-the-permissions-you-need"></a>1. Adım: ihtiyacınız olan izinleri belirleme

Azure 'da, özel rolünüzün potansiyel olarak içerebileceği binlerce izin vardır. Özel rolünüze eklemek istediğiniz izinleri belirleyebilmeniz için kullanabileceğiniz dört yol aşağıda verilmiştir:

| Yöntem | Açıklama |
| --- | --- |
| Mevcut rollere bakın | Hangi izinlerin kullanıldığını görmek için mevcut rollere bakabilirsiniz. Daha fazla bilgi için bkz. [Azure yerleşik rolleri](built-in-roles.md). |
| Anahtar sözcüğe göre izinleri ara | Azure portal kullanarak özel bir rol oluşturduğunuzda, anahtar sözcüğe göre izinler araması yapabilirsiniz. Örneğin, *sanal makine* veya *faturalandırma* izinleri için arama yapabilirsiniz. Bu arama işlevi, [4. Adım: izinler](#step-4-permissions)bölümünde daha sonra açıklanmaktadır. |
| Tüm izinleri indir | Azure portal kullanarak özel bir rol oluşturduğunuzda tüm izinleri bir CSV dosyası olarak indirebilir ve ardından bu dosyada arama yapabilirsiniz. İzinleri **Ekle** bölmesinde tüm izinleri indirmek için **tüm izinleri indir** düğmesine tıklayın. İzin Ekle bölmesi hakkında daha fazla bilgi için bkz. [4. Adım: izinler](#step-4-permissions). |
| Docs 'taki izinleri görüntüleme | Kullanılabilir izinleri [Azure Resource Manager kaynak sağlayıcısı işlemlerinde](resource-provider-operations.md)görüntüleyebilirsiniz. |

## <a name="step-2-choose-how-to-start"></a>2. Adım: nasıl başlayakullanacağınızı seçin

Özel bir rol oluşturmaya başlayabilmeniz için kullanabileceğiniz üç yol vardır. Var olan bir rolü klonlayabilir, sıfırdan başlayabilir veya bir JSON dosyasıyla başlayabilirsiniz. En kolay yol, ihtiyacınız olan izinlere sahip olan mevcut bir rolü bulmanız ve sonra senaryonuz için kopyalayıp değiştirmektir. 

### <a name="clone-a-role"></a>Rolü kopyalama

Mevcut bir rol gerekli izinlere sahip değilse, onu kopyalayabilir ve sonra izinleri değiştirebilirsiniz. Bir rolü kopyalamaya başlamak için bu adımları izleyin.

1. Azure portal, özel rolün atanabilir olmasını istediğiniz bir abonelik veya kaynak grubu açın ve ardından **erişim denetimi (IAM)** öğesini açın.

    Aşağıdaki ekran görüntüsünde, abonelik için açılan erişim denetimi (ıAM) sayfası gösterilmektedir.

    ![Abonelik için erişim denetimi (ıAM) sayfası](./media/custom-roles-portal/access-control-subscription.png)

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesine tıklayın.

1. Fatura okuyucu rolü gibi kopyalamak istediğiniz bir rolü arayın.

1. Satırın sonunda üç nokta (**...**) simgesine tıklayın ve ardından **Kopyala**' ya tıklayın.

    ![Kopya bağlam menüsü](./media/custom-roles-portal/clone-menu.png)

    Bu, özel roller düzenleyicisini **bir rol Kopyala** seçeneği belirlenmiş olarak açar.

1. 3. [Adım: temel bilgiler](#step-3-basics)' e geçin.

### <a name="start-from-scratch"></a>Sıfırdan başlama

İsterseniz, sıfırdan özel bir rol başlatmak için aşağıdaki adımları izleyebilirsiniz.

1. Azure portal, özel rolün atanabilir olmasını istediğiniz bir abonelik veya kaynak grubu açın ve ardından **erişim denetimi (IAM)** öğesini açın.

1. **Ekle** ' ye tıklayın ve ardından **özel rol Ekle**' ye tıklayın.

    ![Özel rol Ekle menüsü](./media/custom-roles-portal/add-custom-role-menu.png)

    Bu, özel roller düzenleyicisini **sıfırdan başla** seçeneği seçili olarak açar.

1. 3. [Adım: temel bilgiler](#step-3-basics)' e geçin.

### <a name="start-from-json"></a>JSON 'dan Başlat

İsterseniz, özel rol değerlerinizin çoğunu bir JSON dosyasında belirtebilirsiniz. Dosyayı özel roller düzenleyicisinde açabilir, ek değişiklikler yapabilir ve sonra özel rolü oluşturabilirsiniz. Bir JSON dosyasıyla başlamak için bu adımları izleyin.

1. Aşağıdaki biçime sahip bir JSON dosyası oluşturun:

    ```json
    {
        "properties": {
            "roleName": "",
            "description": "",
            "assignableScopes": [],
            "permissions": [
                {
                    "actions": [],
                    "notActions": [],
                    "dataActions": [],
                    "notDataActions": []
                }
            ]
        }
    }
    ```

1. JSON dosyasında çeşitli özellikler için değerleri belirtin. Aşağıda bazı değerler eklenmiş bir örnek verilmiştir. Farklı özellikler hakkında daha fazla bilgi için bkz. [Azure rol tanımlarını anlama](role-definitions.md).

    ```json
    {
        "properties": {
            "roleName": "Billing Reader Plus",
            "description": "Read billing data and download invoices",
            "assignableScopes": [
                "/subscriptions/11111111-1111-1111-1111-111111111111"
            ],
            "permissions": [
                {
                    "actions": [
                        "Microsoft.Authorization/*/read",
                        "Microsoft.Billing/*/read",
                        "Microsoft.Commerce/*/read",
                        "Microsoft.Consumption/*/read",
                        "Microsoft.Management/managementGroups/read",
                        "Microsoft.CostManagement/*/read",
                        "Microsoft.Support/*"
                    ],
                    "notActions": [],
                    "dataActions": [],
                    "notDataActions": []
                }
            ]
        }
    }
    ```
    
1. Azure portal, **erişim denetimi (IAM)** sayfasını açın.

1. **Ekle** ' ye tıklayın ve ardından **özel rol Ekle**' ye tıklayın.

    ![Özel rol Ekle menüsü](./media/custom-roles-portal/add-custom-role-menu.png)

    Bu, özel roller düzenleyicisini açar.

1. Temel bilgiler sekmesinde, **temel izinler**' de **JSON 'dan Başlat**' ı seçin.

1. Dosya Seç kutusunun yanındaki klasör düğmesine tıklayarak aç iletişim kutusunu açın.

1. JSON dosyanızı seçin ve ardından **Aç**' a tıklayın.

1. 3. [Adım: temel bilgiler](#step-3-basics)' e geçin.

## <a name="step-3-basics"></a>3. Adım: temel bilgiler

**Temel bilgiler** sekmesinde, özel rolünüzün ad, açıklama ve taban çizgisi izinlerini belirtirsiniz.

1. **Özel rol adı** kutusunda, özel rol için bir ad belirtin. Ad, Azure AD dizini için benzersiz olmalıdır. Ad harf, sayı, boşluk ve özel karakterler içerebilir.

1. **Açıklama** kutusunda, özel rol için isteğe bağlı bir açıklama belirtin. Bu, özel rol için araç ipucu olur.

    **Taban çizgisi izinleri** seçeneği, önceki adıma göre zaten ayarlanmalıdır, ancak bunu değiştirebilirsiniz.

    ![Belirtilen değerleri içeren temel bilgiler sekmesi](./media/custom-roles-portal/basics-values.png)

## <a name="step-4-permissions"></a>4. Adım: Izinler

**İzinler** sekmesinde, özel rolünüzün izinlerini belirtirsiniz. Bir rolü Klonladığınız veya JSON ile başladığınıza bağlı olarak, Izinler sekmesi bazı izinleri zaten listelemeyebilir.

![Özel rol oluştur 'un İzinler sekmesi](./media/custom-roles-portal/permissions.png)

### <a name="add-or-remove-permissions"></a>İzinleri ekleme veya kaldırma

Özel rolünüzün izinlerini eklemek veya kaldırmak için bu adımları izleyin.

1. İzinler eklemek için **izin Ekle ' ye tıklayarak izin** Ekle bölmesini açın.

    Bu bölme, kullanılabilir tüm izinleri bir kart biçiminde farklı kategoriler halinde gruplanmış olarak listeler. Her kategori, Azure kaynaklarını sağlayan bir hizmet olan bir *kaynak sağlayıcısını*temsil eder.

1. **Izin ara** kutusuna, izinleri aramak için bir dize yazın. Örneğin, faturayla ilgili izinleri bulmak için *Fatura* araması yapın.

    Arama dizeniz temelinde, kaynak sağlayıcısı kartlarının bir listesi görüntülenir. Kaynak sağlayıcılarının Azure hizmetleriyle nasıl eşlenme listesi için bkz. [Azure hizmetleri Için kaynak sağlayıcıları](../azure-resource-manager/management/azure-services-resource-providers.md).

    ![Kaynak sağlayıcısı ile izin bölmesi ekleme](./media/custom-roles-portal/add-permissions-provider.png)

1. Özel rolünüze eklemek istediğiniz izinlere sahip olabilecek **Microsoft faturalandırma**gibi bir kaynak sağlayıcısı kartına tıklayın.

    Bu kaynak sağlayıcının yönetim izinlerinin listesi, arama dizeniz temelinde görüntülenir.

    ![İzin listesi ekle](./media/custom-roles-portal/add-permissions-list.png)

1. Veri düzlemi için uygulanan izinleri arıyorsanız, **veri eylemleri**' ne tıklayın. Aksi takdirde, yönetim düzlemine uygulanan izinleri listelemek için Eylemler seçeneğini **Eylemler** olarak ayarlayın. Yönetim düzlemi ve veri düzlemi arasındaki farklılıklar hakkında daha fazla bilgi için bkz. [Yönetim ve veri işlemleri](role-definitions.md#management-and-data-operations).

1. Gerekirse, aramanızı daha ayrıntılı hale getirmek için arama dizesini güncelleştirin.

1. Özel rolünüze eklemek istediğiniz bir veya daha fazla izin bulduktan sonra, izinlerin yanına bir onay işareti ekleyin. Örneğin, diğer ' in yanına bir onay işareti ekleyin: faturaları indirme iznini eklemek için **faturayı indirin** .

1. İzin listenize izin eklemek için **Ekle** ' ye tıklayın.

    İzin bir veya olarak eklenir `Actions` `DataActions` .

    ![İzin eklendi](./media/custom-roles-portal/permissions-list-add.png)

1. İzinleri kaldırmak için, satırın sonundaki Sil simgesine tıklayın. Bu örnekte, bir kullanıcının destek bileti oluşturmasına gerek duymayacak, `Microsoft.Support/*` izin silinebilir.

### <a name="add-wildcard-permissions"></a>Joker karakter izinleri ekleme

Başlatma isteğinize bağlı olarak, izin listenizde joker karakterler () ile izinleriniz olabilir `*` . Bir joker karakter ( `*` ) sağladığınız eylem dizesiyle eşleşen her şeye bir izni genişletir. Örneğin, aşağıdaki joker karakter dizesi Azure maliyet yönetimi ve dışarı aktarma ile ilgili tüm izinleri ekler. Bu Ayrıca, eklenebilir olabilecek, gelecekteki tüm dışarı aktarma izinlerini de içerir.

```
Microsoft.CostManagement/exports/*
```

Yeni bir joker karakter izni eklemek istiyorsanız, **Izinleri Ekle** bölmesini kullanarak ekleyemezsiniz. Joker karakter iznini eklemek için **JSON** sekmesini kullanarak el ile eklemeniz gerekir. Daha fazla bilgi için bkz. [6. Adım: JSON](#step-6-json).

### <a name="exclude-permissions"></a>Dışlama izinleri

Rolünüzün bir joker karakter ( `*` ) izni varsa ve bu joker karakter izninden belirli izinleri dışlamak ya da çıkarmak istiyorsanız, bunları dışarıda bırakabilirsiniz. Örneğin, aşağıdaki joker karakter iznine sahip olduğunu varsayalım:

```
Microsoft.CostManagement/exports/*
```

Bir dışarı aktarmanın silinmesine izin vermek istemiyorsanız, aşağıdaki Silme iznini dışlayabilirsiniz:

```
Microsoft.CostManagement/exports/delete
```

Bir izni dışladığınızda, veya olarak eklenir `NotActions` `NotDataActions` . Etkin yönetim izinleri, tüm `Actions` ' ı eklenerek ve ardından tüm öğesinin çıkarılmasıyla hesaplanır `NotActions` . Etkin veri izinleri, tüm `DataActions` ' ı eklenerek ve ardından tüm öğesinin çıkarılmasıyla hesaplanır `NotDataActions` .

> [!NOTE]
> İznin dışlanması bir reddetme ile aynı değildir. İzinlerin dışlanması, izinleri bir joker karakter izninden çıkarmak için kullanışlı bir yoldur.

1. İzin verilen joker karakter izninden bir izni dışlamak veya çıkarmak için izinleri hariç tut bölmesini açmak için **Izinleri hariç tut** ' a tıklayın.

    Bu bölmede, dışlanan veya çıkarılan yönetim veya veri izinlerini belirtirsiniz.

1. Dışlamak istediğiniz bir veya daha fazla izin bulduktan sonra, izinlerin yanına bir onay işareti ekleyin ve ardından **Ekle** düğmesine tıklayın.

    ![İzinleri dışlama bölmesi-izin seçildi](./media/custom-roles-portal/exclude-permissions-select.png)

    İzin veya olarak eklenir `NotActions` `NotDataActions` .

    ![İzin dışlandı](./media/custom-roles-portal/exclude-permissions-list-add.png)

## <a name="step-5-assignable-scopes"></a>5. Adım: atanabilir kapsamlar

**Atanabilir kapsamlar** sekmesinde, özel rolünüzün atama için kullanılabildiği, abonelik veya kaynak grubu gibi bir konum belirtirsiniz. Bu sekmede, başlatmayı nasıl seçtiğinize bağlı olarak, erişim denetimi (ıAM) sayfasını açtığınız kapsam listeleyebilir. Atanabilir kapsamın kök kapsama ("/") ayarlanması desteklenmiyor. Şu anda bir yönetim grubunu atanabilir kapsam olarak ekleyemezsiniz.

1. Atanabilir kapsamlar Ekle bölmesini açmak için **atanabilir kapsamlar Ekle** ' ye tıklayın.

    ![Atanabilir kapsamlar sekmesi](./media/custom-roles-portal/assignable-scopes.png)

1. Kullanmak istediğiniz bir veya daha fazla kapsamı (genellikle aboneliğiniz) tıklatın.

    ![Atanabilir kapsamlar ekleme](./media/custom-roles-portal/add-assignable-scopes.png)

1. Atanabilir kapsamınızı eklemek için **Ekle** düğmesine tıklayın.

## <a name="step-6-json"></a>6. Adım: JSON

**JSON** sekmesinde, özel rolünüzün JSON içinde biçimlendirildiğini görürsünüz. İsterseniz JSON 'u doğrudan düzenleyebilirsiniz. Joker karakter ( `*` ) izni eklemek istiyorsanız, bu sekmeyi kullanmanız gerekir.

1. JSON 'u düzenlemek için **Düzenle**' ye tıklayın.

    ![Özel rolü gösteren JSON sekmesi](./media/custom-roles-portal/json.png)

1. JSON üzerinde değişiklik yapın.

    JSON doğru biçimlendirilmediyse dikey cilt alanında kırmızı bir pürüzlü çizgi ve bir gösterge görürsünüz.

1. Düzenlemeden sonra **Kaydet**' e tıklayın.

## <a name="step-7-review--create"></a>7. Adım: Inceleme + oluştur

**Gözden geçir + oluştur** sekmesinde, özel rol ayarlarınızı gözden geçirebilirsiniz.

1. Özel rol ayarlarınızı gözden geçirin.

    ![Gözden geçir + Oluştur sekmesi](./media/custom-roles-portal/review-create.png)

1. Özel rolünüzü oluşturmak için **Oluştur** ' a tıklayın.

    Birkaç dakika sonra, özel rolünüzün başarıyla oluşturulduğunu belirten bir ileti kutusu görünür.

    ![Özel rol iletisi oluştur](./media/custom-roles-portal/custom-role-success.png)

    Herhangi bir hata algılanırsa, bir ileti görüntülenir.

    ![İnceleme + oluşturma hatası](./media/custom-roles-portal/review-create-error.png)

1. Yeni özel rolünüzü **Roller** listesinde görüntüleyin. Özel rolünüzü görmüyorsanız **Yenile**' ye tıklayın.

     Özel rolünüzün her yerde gösterilmesi birkaç dakika sürebilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Özel rollerinizi görüntülemek için aşağıdaki adımları izleyin.

1. Bir abonelik veya kaynak grubu açın ve ardından **erişim denetimi (IAM)** öğesini açın.

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesine tıklayın.

1. **Tür** listesinde, **customrole** öğesini seçerek özel rollerinizi görüntüleyin.

    Özel rolünüzü oluşturduysanız ve listede görmüyorsanız, **Yenile**' ye tıklayın.

    ![Özel rol listesi](./media/custom-roles-portal/custom-role-list.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

1. Bu makalenin önceki kısımlarında açıklandığı gibi, özel roller listenizi açın.

1. Güncelleştirmek istediğiniz özel rolün üç nokta (**...**) simgesine tıklayın ve ardından **Düzenle**' ye tıklayın. Yerleşik rolleri güncelleştiremayacağınızı unutmayın.

    Özel rol düzenleyicide açılır.

    ![Özel rol menüsü](./media/custom-roles-portal/edit-menu.png)

1. Özel rolü güncelleştirmek için farklı sekmeleri kullanın.

1. Değişikliklerinizi tamamladıktan sonra değişikliklerinizi gözden geçirmek için **gözden geçir + oluştur** sekmesine tıklayın.

1. Özel rolünüzü güncelleştirmek için **Güncelleştir** düğmesine tıklayın.

## <a name="delete-a-custom-role"></a>Özel rolü silme

1. Bu makalenin önceki kısımlarında açıklandığı gibi, özel roller listenizi açın.

1. Özel rolü kullanan tüm rol atamalarını kaldırın.

1. Silmek istediğiniz özel rolün üç nokta (**...**) simgesine tıklayın ve ardından **Sil**' e tıklayın.

    ![Özel rol menüsü](./media/custom-roles-portal/delete-menu.png)

    Özel rolünüzün tamamen silinmesi birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure PowerShell kullanarak bir Azure özel rolü oluşturma](tutorial-custom-role-powershell.md)
- [Özel Azure rolleri](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
