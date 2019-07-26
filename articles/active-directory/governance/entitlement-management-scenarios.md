---
title: Azure AD Yetkilendirme Yönetimi (Önizleme) ile yaygın senaryolar-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) yaygın senaryolar için izlemeniz gereken üst düzey adımları öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8e7709abcc97baac9bf657b9fff2110cb2e72c1
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489026"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'ndeki genel senaryolar (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşunuz için yetkilendirme yönetimini yapılandırabileceğiniz çeşitli yollar vardır. Ancak, yeni başladıysanız Yöneticiler, onaylayanlar ve istek sahipleri için yaygın senaryoları anlamanız yararlı olur.

## <a name="administrators"></a>Yöneticiler

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Yetkilendirme Yönetimi için yeni başladım ve Başlarken yardım istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | [İlk erişim paketinizi oluşturmak için öğreticiyi izleyin](entitlement-management-access-package-first.md) | [![Azure portal simgesi](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Dizinimde bulunan kullanıcıların gruplara, uygulamalara veya SharePoint sitelerine erişim istemesine izin vermek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Bir katalogda yeni bir erişim paketi oluşturma](entitlement-management-access-package-create.md#start-new-access-package) | ![Erişim paketi oluşturma](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Erişim paketine kaynak rolleri ekleme](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Gruplar</li><li>Uygulamalar</li><li>SharePoint siteleri</li></ul> | ![Kaynak rolleri ekleme](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [İlke Ekle](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Dizininizdeki kullanıcılar için</li><li>Onay iste</li><li>Süre sonu ayarları</li></ul> | ![İlke ekleme](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Kullanıcılar, iş ortakları dizinimin (henüz dizinimde olmayan kullanıcılar dahil) gruplara, uygulamalara veya SharePoint sitelerine erişim istemesine izin vermek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Bir katalogda yeni bir erişim paketi oluşturma](entitlement-management-access-package-create.md#start-new-access-package) | ![Erişim paketi oluşturma](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Erişim paketine kaynak rolleri ekleme](entitlement-management-access-package-edit.md#add-resource-roles) | ![Kaynak rolleri ekleme](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Dış kullanıcılar için ilke ekleme](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Dizininizde bulunmayan kullanıcılar için</li><li>Onay iste</li><li>Süre sonu ayarları</li></ul> | ![Dış kullanıcılar için ilke ekleme](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [Erişim paketini iş ortağınıza istemek için erişim portalı bağlantısını gönderin](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>İş ortağı, bağlantısını kullanıcılarla paylaşabilir</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Erişim paketindeki grupları, uygulamaları veya SharePoint sitelerini değiştirmek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** Erişim paketini açın | ![Kaynak rolleri ekleme](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Kaynak rolleri ekleme veya kaldırma](entitlement-management-access-package-edit.md#add-resource-roles) | ![Kaynak rolleri ekleme](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Gruplar, uygulamalar veya SharePoint siteleri için kimin atanma olduğunu görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** Bir erişim paketi açın | ![Kaynak rolleri ekleme](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Atamaları görüntüle](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Hangi kullanıcıların erişim paketine erişimi olduğunu görüntüleme</li><li>Hangi kullanıcının erişiminin dolduğunu görüntüleme</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Bir kullanıcının erişimi olan grupları, uygulamaları veya SharePoint sitelerini görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | [Kullanıcı atamaları raporunu görüntüle](entitlement-management-reports.md)<ul><li>Ne zaman istendiğini ve kim tarafından onaylandığını görüntüle</li></ul> |  |

## <a name="approvers"></a>Onaylayanlar

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Gruplara, uygulamalara veya SharePoint sitelerine erişim isteklerini onaylamak istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Erişim portalından isteği aç](entitlement-management-request-approve.md#open-request) | [![Erişim Portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Erişim isteğini onayla](entitlement-management-request-approve.md#approve-or-deny-request) | ![Erişimi onayla](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>İstek Sahipleri

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Bana sunulan grupları, uygulamaları veya SharePoint sitelerini görüntülemek ve erişim istemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Erişim Portalı 'nda oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Erişim Portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Erişim paketi bul |  |
> | **3.** [Erişim isteği](entitlement-management-request-access.md#request-an-access-package) | ![Erişim izni iste](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Bir dış Kullanıcı kullanıyorum ve doğrudan bağlantı ile gruplara, uygulamalara veya SharePoint sitelerine erişim istemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Aldığınız erişim portalı bağlantısını bulun](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [Erişim Portalı 'nda oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Erişim Portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Erişim isteği](entitlement-management-request-access.md#request-an-access-package) | ![Dış Kullanıcı erişimi iste](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Zaten erişim izni olan grupları, uygulamaları veya SharePoint sitelerini görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Erişim Portalı 'nda oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Erişim Portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Etkin erişim paketlerini görüntüle |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: İlk erişim paketinizi oluşturma](entitlement-management-access-package-first.md)
- [Temsilci görevleri](entitlement-management-delegate.md)
