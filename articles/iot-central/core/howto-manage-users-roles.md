---
title: Azure IoT Merkezi uygulamasında kullanıcıları ve rolleri yönetme | Microsoft Dokümanlar
description: Yönetici olarak, Azure IoT Merkezi uygulamanızdaki kullanıcıları ve rolleri nasıl yönetebilirsiniz?
author: lmasieri
ms.author: lmasieri
ms.date: 12/05/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: c00f9d8baa55ef0d0cf6322ee71f22e739e6acdc
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365499"
---
# <a name="manage-users-and-roles-in-your-iot-central-application"></a>IoT Central uygulamanızdaki kullanıcıları ve rolleri yönetme

Bu makalede, bir yönetici olarak Azure IoT Merkezi uygulamanızdaki kullanıcıları nasıl ekleyebileceğiniz, düzenleyebileceğiniz ve silebileceğiniz açıklanmaktadır. Makalede, Azure IoT Merkezi uygulamanızdaki rollerin nasıl yönetilenolduğu da açıklanmaktadır.

Yönetim bölümüne erişmek ve **bunları** kullanmak için, Bir Azure IoT Merkezi uygulaması için **Yönetici** rolünde olmalısınız. Bir Azure IoT Merkezi uygulaması oluşturursanız, bu uygulama için **Yönetici** rolüne otomatik olarak eklenirsiniz.

## <a name="add-users"></a>Kullanıcı ekle

Oturum açmadan ve bir Azure IoT Merkezi uygulamasına erişebilmeleri için her kullanıcının bir kullanıcı hesabı olması gerekir. Azure IoT Central'da Microsoft Hesapları ve Azure Etkin Dizin hesapları desteklenir. Azure Etkin Dizin grupları şu anda Azure IoT Central'da desteklenmemektedir.

Daha fazla bilgi için [Microsoft hesap yardımı](https://support.microsoft.com/products/microsoft-account?category=manage-account) ve [Quickstart: Azure Etkin Dizin'e yeni kullanıcılar ekleyin.](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)

1. Bir IoT Merkezi uygulamasına kullanıcı eklemek için **Yönetim** bölümündeki **Kullanıcılar** sayfasına gidin.
    
    > [!div class="mx-imgBorder"]
    >![Kullanıcıları yönetme](media/howto-manage-users-roles/manage-users-pnp.png)

1. Kullanıcı eklemek için **Kullanıcılar** sayfasında **+ Kullanıcı Ekle'yi**seçin.

1. **Role** açılır menüsünden kullanıcı için bir rol seçin. Bu makalenin [Rolleri Yönet](#manage-roles) bölümündeki roller hakkında daha fazla bilgi edinin.

    > [!div class="mx-imgBorder"]
    >![Kullanıcı ekleme ve bir rol seçme](media/howto-manage-users-roles/add-user-pnp.png)

    > [!NOTE]
    > Diğer kullanıcıları ekleme izni veren özel bir rolde olan bir kullanıcı, kullanıcıları yalnızca kendi rollerinden daha aynı veya daha az izinle bir role ekleyebilir.

Bir IoT Merkezi kullanıcı kimliği Azure Active Directory'den silinir ve sonra yeniden eklenirse, kullanıcı IoT Central uygulamasında oturum açamaz. Erişimi yeniden etkinleştirmek için, IoT Central yöneticisinin uygulamayı silmesi ve kullanıcıyı okuması gerekir.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Kullanıcılara atanan rolleri yeniden

Roller atandıktan sonra değiştirilemez. Bir kullanıcıya atanan rolü değiştirmek için, kullanıcıyı silin ve ardından kullanıcıyı farklı bir rolle yeniden ekleyin.

> [!NOTE]
> Atanan roller IoT Central uygulamasına özgüdür ve Azure Portalı'ndan yönetilemez.

## <a name="delete-users"></a>Kullanıcıları silme

Kullanıcıları silmek için **Kullanıcılar** sayfasında bir veya daha fazla onay kutusu seçin. Ardından **Sil**’i seçin.

## <a name="manage-roles"></a>Rolleri yönetme

Roller, kuruluşunuz içinde kimlerin IoT Central'da çeşitli görevleri yapmasına izin verildiğini denetlemenize olanak tanır. Uygulamanızın kullanıcılarına atayabileceğiniz üç yerleşik rol vardır. Daha ince taneli denetime ihtiyaç duyarsanız özel roller de [oluşturabilirsiniz.](#create-a-custom-role)

> [!div class="mx-imgBorder"]
> ![Rol seçimini yönetme](media/howto-manage-users-roles/manage-roles-pnp.png)

### <a name="administrator"></a>Yönetici

**Yönetici** rolündeki kullanıcılar, faturalandırma da dahil olmak üzere uygulamanın her bölümünü yönetebilir ve denetleyebilir.

Bir uygulama oluşturan kullanıcı otomatik olarak **Yönetici** rolüne atanır. **Yönetici** rolünde her zaman en az bir kullanıcı olmalıdır.

### <a name="builder"></a>Oluşturucu

**Oluşturucu** rolündeki kullanıcılar uygulamanın her bölümünü yönetebilir, ancak Yönetim veya Sürekli Veri Dışa aktarMa sekmelerinde değişiklik yapamaz.

### <a name="operator"></a>İşleç

**Operatör** rolündeki kullanıcılar aygıt durumunu ve durumunu izleyebilir. Aygıt şablonlarında değişiklik yapmalarına veya uygulamayı yönetmelerine izin verilmez. Operatörler aygıtek ve silme, aygıt kümelerini yönetme ve analiz ve işleri çalıştırabilir. 

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Çözümünüz daha ince taneli erişim denetimleri gerektiriyorsa, özel izin kümeleriyle özel roller oluşturabilirsiniz. Özel bir rol oluşturmak için, uygulamanızın **Yönetim** bölümündeki **Roller** sayfasına gidin. Ardından **+ Yeni rolü**seçin ve rolünüz için bir ad ve açıklama ekleyin. Rolünüzün gerektirdiği izinleri seçin ve ardından **Kaydet'i**seçin.

Kullanıcıları yerleşik bir role eklediğiniz gibi özel rolünüze de ekleyebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Özel bir rol oluşturma](media/howto-manage-users-roles/create-custom-role-pnp.png)

### <a name="custom-role-options"></a>Özel rol seçenekleri

Özel bir rol tanımladığınızda, bir kullanıcıya rolün bir üyesiyse verilen izinkümesini seçersiniz. Bazı izinler diğerlerine bağlıdır. Örneğin, bir role **Güncelleştirme uygulaması panolarına** izin eklerseniz, **Uygulama panolarını görüntüle** izni otomatik olarak eklenir. Aşağıdaki tablolar, özel roller oluştururken kullanabileceğiniz kullanılabilir izinleri ve bunların bağımlılıklarını özetler.

#### <a name="managing-devices"></a>Cihazları yönetme

**Aygıt şablonu izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Yönetme | Görünüm <br/> Diğer bağımlılıklar: Aygıt örneklerini görüntüleme  |
| Tam Denetim | Görüntüle, Yönet <br/> Diğer bağımlılıklar: Aygıt örneklerini görüntüleme |

**Aygıt örneği izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme |
| Güncelleştirme | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme  |
| Oluşturma | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme  |
| Sil | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme  |
| Komutları Yürüt | Güncelleme, Görüntüleme <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme  |
| Tam Denetim | Komutları Görüntüle, Güncelleştir, Oluştur, Sil, Komutları Çalıştır <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt gruplarını görüntüleme  |

**Aygıt grupları izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt örneklerini görüntüleme |
| Güncelleştirme | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt örneklerini görüntüleme   |
| Oluşturma | Görünüm, Güncelleme <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt örneklerini görüntüleme   |
| Sil | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt örneklerini görüntüleme   |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme <br/> Diğer bağımlılıklar: Aygıt şablonlarını ve aygıt örneklerini görüntüleme |

**Aygıt bağlantısı yönetimi izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Örneği okuma | None <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt gruplarını, aygıt örneklerini görüntüleyin |
| Örneği yönetme | None |
| Genel okuma | None   |
| Genel yönetimi yönetin | Genel Oku |
| Tam Denetim | Örneği oku, örneği yönet, genel ilerle, genel ilerle. <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt gruplarını, aygıt örneklerini görüntüleyin |

**İş izni**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleme |
| Güncelleştirme | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleme |
| Oluşturma | Görünüm, Güncelleme <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleme |
| Sil | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleme |
| Yürütme | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleyin; Aygıt örneklerini güncelleştirin; Aygıt örneklerinde komutları yürütme |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme, Yürütme <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini ve aygıt gruplarını görüntüleyin; Aygıt örneklerini güncelleştirin; Aygıt örneklerinde komutları yürütme |

**Kurallar izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None <br/> Diğer bağımlılıklar: Aygıt şablonlarını görüntüleme |
| Güncelleştirme | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını görüntüleme |
| Oluşturma | Görünüm, Güncelleme <br/> Diğer bağımlılıklar: Aygıt şablonlarını görüntüleme |
| Sil | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını görüntüleme |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme <br/> Diğer bağımlılıklar: Aygıt şablonlarını görüntüleme |

#### <a name="managing-the-app"></a>Uygulamayı yönetme

**Uygulama ayarları izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Kopyala | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini, aygıt gruplarını, panoları, veri dışa aktarma, markaoluşturma, yardım bağlantılarını, özel rolleri, kuralları görüntüleyin |
| Sil | Görünüm   |
| Tam Denetim | Görüntüle, Güncelle, Kopyala, Sil <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt gruplarını, uygulama panolarını, veri dışa aktarmayı, markalama, yardım bağlantılarını, özel rolleri, kuralları görüntüleyin |

**Uygulama şablonu dışa aktarma izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Dışarı Aktarma | Görünüm <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt örneklerini, aygıt gruplarını, panoları, veri dışa aktarma, markaoluşturma, yardım bağlantılarını, özel rolleri, kuralları görüntüleyin |
| Tam Denetim | Görünüm, Dışa Aktarma <br/> Diğer bağımlılıklar: Aygıt şablonlarını, aygıt gruplarını, uygulama panolarını, veri dışa aktarmayı, markalama, yardım bağlantılarını, özel rolleri, kuralları görüntüleyin |

**Faturalama izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Yönetme | None     |
| Tam Denetim | Yönetme |

#### <a name="managing-users-and-roles"></a>Kullanıcıları ve rolleri yönetme

**Özel roller izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None |
| Güncelleştirme | Görünüm |
| Oluşturma | Görünüm, Güncelleme |
| Sil | Görünüm |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme |

**Kullanıcı yönetimi izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None <br/> Diğer bağımlılıklar: Özel rolleri görüntüleyin |
| Ekle | Görünüm <br/> Diğer bağımlılıklar: Özel rolleri görüntüleyin |
| Sil | Görünüm <br/> Diğer bağımlılıklar: Özel rolleri görüntüleyin |
| Tam Denetim | Görüntüle, Ekle, Sil <br/> Diğer bağımlılıklar: Özel rolleri görüntüleyin |

> [!NOTE]
> Diğer kullanıcıları ekleme izni veren özel bir rolde olan bir kullanıcı, kullanıcıları yalnızca kendi rollerinden daha aynı veya daha az izinle bir role ekleyebilir.

#### <a name="customizing-the-app"></a>Uygulamayı özelleştirme

**Uygulama panosu izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Oluşturma | Görünüm, Güncelleme |
| Sil | Görünüm   |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme |

**Kişisel panoizinler**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Oluşturma | Görünüm, Güncelleme   |
| Sil | Görünüm   |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme |

**Markalama, favicon ve renk izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Tam Denetim | Görünüm, Güncelleme |

**Yardım bağlantıları izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Tam Denetim | Görünüm, Güncelleme |

#### <a name="extending-the-app"></a>Uygulamayı genişletme

**Veri verme izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Güncelleştirme | Görünüm   |
| Oluşturma | Görünüm, Güncelleme  |
| Sil | Görünüm   |
| Tam Denetim | Görüntüleme, Güncelleme, Oluşturma, Silme |

**API belirteç izinleri**

| Adı | Bağımlılıklar |
| ---- | -------- |
| Görünüm | None     |
| Oluşturma | Görünüm   |
| Sil | Görünüm   |
| Tam Denetim | Görüntüleme, Oluşturma, Silme |

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi uygulamanızda kullanıcıları ve rolleri nasıl yöneteceğimize ilişkin bilgi edindiğinize göre, önerilen bir sonraki adım faturanızı nasıl [yöneteceğimize](howto-view-bill.md)ilişkin bilgi edinmektir.
