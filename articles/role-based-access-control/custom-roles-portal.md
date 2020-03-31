---
title: Azure portalı (Önizleme) - Azure RBAC'ı kullanarak Azure özel rolleri oluşturma veya güncelleme
description: Azure portalını kullanarak Azure rol tabanlı erişim denetimi (Azure RBAC) için Azure özel rolleri nasıl oluşturabilirsiniz öğrenin. Buna, özel rollerin nasıl listelenebildiğini, oluşturulup güncelleştirilen ve silinir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/26/2020
ms.author: rolyon
ms.openlocfilehash: 3204cdf51f3f37588f684f801a811f569b337d13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77674876"
---
# <a name="create-or-update-azure-custom-roles-using-the-azure-portal-preview"></a>Azure portalını kullanarak Azure özel rolleri oluşturma veya güncelleme (Önizleme)

> [!IMPORTANT]
> Azure portalını kullanan Azure özel rolleri şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure [yerleşik rolleri](built-in-roles.md) kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi Azure özel rollerinizi oluşturabilirsiniz. Yerleşik roller gibi, abonelik ve kaynak grubu kapsamlarında kullanıcılara, gruplara ve hizmet ilkelerine özel roller atayabilirsiniz. Özel roller bir Azure Etkin Dizin (Azure AD) dizininde depolanır ve abonelikler arasında paylaşılabilir. Her dizin en fazla 5000 özel rol olabilir. Azure portalı, Azure PowerShell, Azure CLI veya REST API kullanılarak özel roller oluşturulabilir. Bu makalede, Azure portalı (şu anda önizlemede) kullanılarak özel rollerin nasıl oluşturulacak olduğu açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Özel roller oluşturmak için şunları yapmanız gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni

## <a name="step-1-determine-the-permissions-you-need"></a>Adım 1: İhtiyacınız olan izinleri belirleme

Azure'da özel rolünüze ekleyebilecek binlerce izin vardır. Özel rolünüze eklemek isteyeceğiniz izinleri belirlemenin dört yolu aşağıda veda edebilirsiniz:

| Yöntem | Açıklama |
| --- | --- |
| Varolan rollere bakın | Hangi izinlerin kullanıldığını görmek için varolan rollere bakabilirsiniz. Daha fazla bilgi için [Azure yerleşik rolleri'ne](built-in-roles.md)bakın. |
| Anahtar kelimeye göre izin arama | Azure portalını kullanarak özel bir rol oluşturduğunuzda, anahtar kelimeye göre izinler arayabilirsiniz. Örneğin, *sanal makine* veya *faturalama* izinleri arayabilirsiniz. Bu arama işlevi daha sonra [Adım 4: İzinler](#step-4-permissions)açıklanır. |
| Tüm izinleri indirin | Azure portalını kullanarak özel bir rol oluşturduğunuzda, tüm izinleri CSV dosyası olarak karşıdan yükleyebilir ve bu dosyada arama yapabilirsiniz. İzin **Ekle** bölmesine, tüm **izinleri** indirmek için tüm izinleri indir düğmesini tıklatın. İzin Ekle bölmesi hakkında daha fazla bilgi için [bkz.](#step-4-permissions) |
| Dokümanlarda izinleri görüntüleme | Kullanılabilir izinleri [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemlerinde](resource-provider-operations.md)görüntüleyebilirsiniz. |

## <a name="step-2-choose-how-to-start"></a>Adım 2: Nasıl başlayacağınızı seçin

Özel bir rol oluşturmaya başlamanın üç yolu vardır. Varolan bir rolü klonlayabilir, sıfırdan başlayabilir veya bir JSON dosyasıyla başlayabilirsiniz. En kolay yolu, ihtiyacınız olan izinlerin çoğuna sahip varolan bir rolü bulmak ve ardından senaryonuz için onu klonlamak ve değiştirmektir. 

### <a name="clone-a-role"></a>Bir rolü klonlama

Varolan bir rol tam olarak ihtiyacınız olan izinlere sahip değilse, onu klonlayabilir ve izinleri değiştirebilirsiniz. Bir rolü klonlamaya başlamak için aşağıdaki adımları izleyin.

1. Azure portalında, özel rolün atanmasını istediğiniz bir abonelik veya kaynak grubu açın ve ardından **Access denetimini (IAM)** açın.

    Aşağıdaki ekran görüntüsü, abonelik için açılan Access denetimi (IAM) sayfasını gösterir.

    ![Abonelik için erişim denetimi (IAM) sayfası](./media/custom-roles-portal/access-control-subscription.png)

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesini tıklatın.

1. Fatura Okuyucu rolü gibi klonlamak istediğiniz bir rolü arayın.

1. Satırın sonunda, elips (**...**) 'yi tıklatın ve ardından **Clone'u**tıklatın.

    ![Bağlamyı klonlama menüsü](./media/custom-roles-portal/clone-menu.png)

    Bu, **clone bir rol** seçeneği seçilen özel roller düzenleyiciaçılır.

1. Adım [3'e geçin: Temel bilgiler](#step-3-basics).

### <a name="start-from-scratch"></a>Sıfırdan başlama

İsterseniz, sıfırdan özel bir rol başlatmak için aşağıdaki adımları izleyebilirsiniz.

1. Azure portalında, özel rolün atanmasını istediğiniz bir abonelik veya kaynak grubu açın ve ardından **Access denetimini (IAM)** açın.

1. **Ekle'yi** tıklatın ve ardından **özel rol ekle'yi (önizleme)** tıklatın.

    ![Özel rol menüsü ekleme](./media/custom-roles-portal/add-custom-role-menu.png)

    Bu, seçilen **sıfırdan Başlat** seçeneğiyle özel roller düzenleyicisini açar.

1. Adım [3'e geçin: Temel bilgiler](#step-3-basics).

### <a name="start-from-json"></a>JSON'dan başla

İsterseniz, bir JSON dosyasında özel rol değerlerinizin çoğunu belirtebilirsiniz. Dosyayı özel roller düzenleyicisinde açabilir, ek değişiklikler yapabilir ve ardından özel rolü oluşturabilirsiniz. JSON dosyasıyla başlamak için aşağıdaki adımları izleyin.

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

1. JSON dosyasında, çeşitli özellikler için değerleri belirtin. Aşağıda, bazı değerlerin eklenmiştir. Farklı özellikler hakkında bilgi için [bkz.](role-definitions.md)

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
    
1. Azure portalında Access **denetimi (IAM)** sayfasını açın.

1. **Ekle'yi** tıklatın ve ardından **özel rol ekle'yi (önizleme)** tıklatın.

    ![Özel rol menüsü ekleme](./media/custom-roles-portal/add-custom-role-menu.png)

    Bu, özel roller düzenleyicisini açar.

1. Temel bilgiler sekmesinde, **Temel izinlerde** **JSON'dan Başlat'ı**seçin.

1. Dosya kutusunu Seç'in yanında, Aç iletişim kutusunu açmak için klasör düğmesini tıklatın.

1. JSON dosyanızı seçin ve sonra **Aç'ı**tıklatın.

1. Adım [3'e geçin: Temel bilgiler](#step-3-basics).

## <a name="step-3-basics"></a>Adım 3: Temel Bilgiler

Temel **Bilgiler** sekmesinde, özel rolünüz için ad, açıklama ve temel izinleri belirtirsiniz.

1. Özel **rol adı** kutusunda, özel rol için bir ad belirtin. Bu ad, Azure AD dizininde benzersiz olmalıdır. Ad harfleri, sayıları, boşlukları ve özel karakterleri içerebilir.

1. **Açıklama** kutusunda, özel rol için isteğe bağlı bir açıklama belirtin. Bu, özel rolün araç ucu olacaktır.

    **Temel izinler** seçeneği zaten önceki adıma göre ayarlanmalıdır, ancak değiştirebilirsiniz.

    ![Belirtilen değerlerle temeller sekmesi](./media/custom-roles-portal/basics-values.png)

## <a name="step-4-permissions"></a>Adım 4: İzinler

**İzinler** sekmesinde, özel rolünüz için izinleri belirtirsiniz. Bir rolü klonlayıp klonlamadığınıza veya JSON ile başlatıp başlamadığınıza bağlı olarak, İzinler sekmesi bazı izinleri zaten listeleyebilir.

![Özel rol oluşturma izinleri sekmesi](./media/custom-roles-portal/permissions.png)

### <a name="add-or-remove-permissions"></a>İzin ekleme veya kaldırma

Özel rolünüz için izin eklemek veya kaldırmak için aşağıdaki adımları izleyin.

1. İzin eklemek için İzin Ekle bölmesini açmak için **İzin Ekle'yi** tıklatın.

    Bu bölme, kart biçiminde farklı kategorilere gruplanmış tüm kullanılabilir izinleri listeler. Her kategori, Azure kaynakları sağlayan bir hizmet olan bir *kaynak sağlayıcısını*temsil eder.

1. İzin **kutusu** arama'da, izinleri aramak için bir dize yazın. Örneğin, faturayla ilgili izinleri bulmak için *fatura* yı arayın.

    Kaynak sağlayıcı kartlarının listesi, arama dizenize göre görüntülenir. Kaynak sağlayıcıların Azure hizmetlerini nasıl eşlenebildiğini öğrenmek için Azure [hizmetleri için Kaynak sağlayıcılarına](../azure-resource-manager/management/azure-services-resource-providers.md)bakın.

    ![Kaynak sağlayıcısıyla izin bölmesi ekleme](./media/custom-roles-portal/add-permissions-provider.png)

1. **Microsoft Faturalandırma**gibi özel rolünüze eklemek istediğiniz izinlere sahip olabilecek bir kaynak sağlayıcı kartını tıklatın.

    Bu kaynak sağlayıcısının yönetim izinlerinin listesi, arama dizenize göre görüntülenir.

    ![İzinler listesi ekleme](./media/custom-roles-portal/add-permissions-list.png)

1. Veri düzlemi için geçerli izinler arıyorsanız, **Veri Eylemleri'ni**tıklatın. Aksi takdirde, yönetim düzlemi için geçerli izinleri listelemek için eylemler kümesini **Eylemler'e** bırakın. Daha fazla bilgi için, yönetim düzlemi ve veri düzlemi arasındaki farklar hakkında, [Bkz. Yönetim ve veri işlemleri.](role-definitions.md#management-and-data-operations)

1. Gerekirse, aramanızı daha da hassaslaştırmak için arama dizesini güncelleştirin.

1. Özel rolünüze eklemek istediğiniz bir veya daha fazla izin bulduğunuzda, izinlerin yanına onay işareti ekleyin. Örneğin, Diğer: Faturaları indirme izni eklemek için **Faturayı İndir'in** yanına bir onay işareti ekleyin.

1. İzin listenize izin eklemek için **Ekle'yi** tıklatın.

    İzin bir veya `Actions` bir `DataActions`.

    ![İzin eklendi](./media/custom-roles-portal/permissions-list-add.png)

1. İzinleri kaldırmak için satırın sonundaki sil simgesini tıklatın. Bu örnekte, bir kullanıcı destek biletleri oluşturma yeteneğine `Microsoft.Support/*` gerek olmadığından, izin silinebilir.

### <a name="add-wildcard-permissions"></a>Joker karakter izinleri ekleme

Nasıl başlamayı seçtiğinize bağlı olarak, izinler listenizde\*joker karakterli izinler () olabilir. Joker karakter\*( ) sağladığınız dizeyle eşleşen her şeye izin verir. Örneğin, Azure Maliyet Yönetimi ve dışa aktarmayla ilgili tüm izinleri eklemek istediğinizi varsayalım. Bu izinlerin tümlerini ekleyebilirsiniz:

```
Microsoft.CostManagement/exports/action
Microsoft.CostManagement/exports/read
Microsoft.CostManagement/exports/write
Microsoft.CostManagement/exports/delete
Microsoft.CostManagement/exports/run/action
```

Bu izinlerin tümünün eklenmesi yerine, joker karakter izni ekleyebilirsiniz. Örneğin, aşağıdaki joker karakter izni önceki beş izine eşdeğerdir. Bu, gelecekte eklenebilen tüm dışa aktarma izinlerini de içerir.

```
Microsoft.CostManagement/exports/*
```

Yeni bir joker karakter izni eklemek istiyorsanız, **İzin Ekle** bölmesini kullanarak ekemezsiniz. Joker karakter izni eklemek için **JSON** sekmesini kullanarak el ile eklemeniz gerekir. Daha fazla bilgi için [Bkz. Adım 6: JSON](#step-6-json).

### <a name="exclude-permissions"></a>İzinleri hariç tutma

Rolünüz bir joker karakter\*( ) iznine sahipse ve belirli izinleri bu joker karakter izninden hariç tutmak veya çıkarmak istiyorsanız, bunları hariç tutabilirsiniz. Örneğin, aşağıdaki joker karakter iznine sahip olduğunuzu varsayalım:

```
Microsoft.CostManagement/exports/*
```

Bir dışa aktarmanın silinmesine izin vermek istemiyorsanız, aşağıdaki silme iznini hariç tutabilirsiniz:

```
Microsoft.CostManagement/exports/delete
```

Bir izni hariç tutsanız, bir `NotActions` `NotDataActions`veya . Etkili yönetim izinleri tüm `Actions` ekleyerek ve daha sonra tüm çıkarılarak `NotActions`hesaplanır. Etkili veri izinleri tüm `DataActions` ekleyerek ve daha sonra tüm çıkarılarak `NotDataActions`hesaplanır.

> [!NOTE]
> İzin hariç olmak, inkar etmekle aynı şey değildir. İzinleri hariç almak, izinleri joker karakter izninden çıkarmanın yalnızca kullanışlı bir yoludur.

1. İzin verilen joker karakter iznini hariç tutmak veya çıkarmak için, İzinleri Hariç Le'yi açmak için **İzinleri Hariç** Le'yi tıklatın.

    Bu bölmede, dışlanan veya çıkarılan yönetim veya veri izinlerini belirtirsiniz.

1. Hariç tutmak istediğiniz bir veya daha fazla izin bulduktan sonra, izinlerin yanına bir onay işareti ekleyin ve sonra **Ekle** düğmesini tıklatın.

    ![İzinbölmeyi hariç tut - izin seçili](./media/custom-roles-portal/exclude-permissions-select.png)

    İzin a `NotActions` veya . `NotDataActions`

    ![İzin hariç](./media/custom-roles-portal/exclude-permissions-list-add.png)

## <a name="step-5-assignable-scopes"></a>Adım 5: Atanabilir kapsamlar

**Atanabilir kapsamlar** sekmesinde, abonelik veya kaynak grubu gibi özel rolünüzün atama için nerede kullanılabildiği belirtilir. Nasıl başlamayı seçtiğinize bağlı olarak, bu sekme Erişim denetimi (IAM) sayfasını açtığınız kapsamı listeleyebilir. Kök kapsamına atanabilir kapsamı ("/") ayarlamak desteklenmez. Bu önizleme için, bir yönetim grubunu atanabilir kapsam olarak ekleyemezsiniz.

1. Atanabilir Kapsam ekle bölmesini açmak için **atanabilir kapsamları Ekle'yi** tıklatın.

    ![Atanabilir kapsamlar sekmesi](./media/custom-roles-portal/assignable-scopes.png)

1. Genellikle aboneliğinizi kullanmak istediğiniz bir veya daha fazla kapsamı tıklatın.

    ![Atayabilen kapsamlar ekleme](./media/custom-roles-portal/add-assignable-scopes.png)

1. Atanabilir kapsamınızı eklemek için **Ekle** düğmesini tıklatın.

## <a name="step-6-json"></a>Adım 6: JSON

**JSON** sekmesinde, Özel rolünüzün JSON'da biçimlendirilmiş olduğunu görürsünüz. İsterseniz, doğrudan JSON'u edinebilirsiniz. Joker karakter (\*) izni eklemek istiyorsanız, bu sekmeyi kullanmanız gerekir.

1. JSON'u yeniden delemek için **Edit'i**tıklatın.

    ![Özel rolü gösteren JSON sekmesi](./media/custom-roles-portal/json.png)

1. JSON'da değişiklik yapın.

    JSON doğru biçimlendirilmemişse, dikey olukta kırmızı pürüzlü bir çizgi ve bir gösterge görürsünüz.

1. Düzenleme tamamlandığında **Kaydet'i**tıklatın.

## <a name="step-7-review--create"></a>Adım 7: Gözden geçirme + oluşturma

Gözden **Geçir + oluştur** sekmesinde, özel rol ayarlarınızı gözden geçirebilirsiniz.

1. Özel rol ayarlarınızı gözden geçirin.

    ![İnceleme + oluşturma sekmesi](./media/custom-roles-portal/review-create.png)

1. Özel rolünüzü oluşturmak için **Oluştur'u** tıklatın.

    Birkaç dakika sonra, özel rolünüzün başarıyla oluşturulduğunu gösteren bir ileti kutusu görüntülenir.

    ![Özel rol iletisi oluşturma](./media/custom-roles-portal/custom-role-success.png)

    Herhangi bir hata algılanırsa, bir ileti görüntülenir.

    ![Gözden geçirme + hata oluşturma](./media/custom-roles-portal/review-create-error.png)

1. **Roller** listesindeki yeni özel rolünüzü görüntüleyin. Özel rolünüzü **görmüyorsanız, Yenile'yi**tıklatın.

     Özel rolünüzün her yerde görünmesi birkaç dakika sürebilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Özel rollerinizi görüntülemek için aşağıdaki adımları izleyin.

1. Bir abonelik veya kaynak grubu açın ve ardından **Access denetimini (IAM)** açın.

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesini tıklatın.

1. **Tür** listesinde, özel rollerinizi görmek için **CustomRole'yi** seçin.

    Özel rolünüzü yeni oluşturduysanız ve listede **göremiyorsanız, Yenile'yi**tıklatın.

    ![Özel rol listesi](./media/custom-roles-portal/custom-role-list.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

1. Bu makalede daha önce açıklandığı gibi, özel roller listenizi açın.

1. Güncelleştirmek istediğiniz özel rol için elipsis (**...**) 'yi tıklatın ve ardından **Edit'i**tıklatın. Yerleşik rolleri güncelleştiremezsiniz.

    Özel rol editörde açılır.

    ![Özel rol menüsü](./media/custom-roles-portal/edit-menu.png)

1. Özel rolü güncelleştirmek için farklı sekmeleri kullanın.

1. Değişikliklerinizi bitirdikten sonra, değişikliklerinizi gözden geçirmek için **Gözden Geçir + oluştur** sekmesini tıklatın.

1. Özel rolünüzü güncellemek için **Güncelleştir** düğmesini tıklatın.

## <a name="delete-a-custom-role"></a>Özel rolü silme

1. Bu makalede daha önce açıklandığı gibi, özel roller listenizi açın.

1. Özel rolü kullanarak tüm rol atamaları kaldırın.

1. Silmek istediğiniz özel rol için elipsis (**...**) tuşuna basın ve sonra **Sil'i**tıklatın.

    ![Özel rol menüsü](./media/custom-roles-portal/delete-menu.png)

    Özel rolünüzün tamamen silinmiş olması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure PowerShell'i kullanarak özel bir rol oluşturma](tutorial-custom-role-powershell.md)
- [Azure'da özel roller](custom-roles.md)
- [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
