---
title: Kaynak adlandırma kısıtlamaları
description: Azure kaynaklarını adlandırma kurallarını ve kısıtlamalarını gösterir.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: f6203f987654b33b32da72bfec030a9d0ab69df8
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80981537"
---
# <a name="naming-rules-and-restrictions-for-azure-resources"></a>Azure kaynaklarına yönelik adlandırma kuralları ve kısıtlamaları

Bu makalede, Azure kaynakları için adlandırma kuralları ve kısıtlamaları özetlenmiştir. Kaynakların nasıl adlandırılmasıyla ilgili öneriler için önerilen [adlandırma ve etiketleme kurallarına](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)bakın.

Geçerli karakterler sütununda özel olarak belirtilmediği sürece kaynak adları büyük/küçük harf duyarsızdır.

Aşağıdaki tablolarda, alfasayısal terim şu anlamlara gelir:

* **a** ile **z** (küçük harfler)
* **A'dan** **Z'ye** (büyük harf)
* **0 ile** **9** (sayılar)

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Sunucu | kaynak grubu | 3-63 | Küçük harfler ve sayılar.<br><br>Küçük harfle başla. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | hizmet | global | 1-50 | Alfanümerik.<br><br>Mektupla başla. |
> | hizmet / apis | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / sorunlar | API | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / sorunlar / ekler | Sorunu | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / konular / yorumlar | Sorunu | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / işlemler | API | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / işlemler / etiketler | Işlem | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / bültenleri | API | 1-80 | Alfanümerikler, alt çizgi ve tireler.<br><br>Alfanümerik veya alt çizgi ile başlatın ve bitirin. |
> | hizmet / apis / şema | API | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / tagDescriptions | API | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / apis / etiketler | API | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / api-sürüm setleri | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / yetkilendirmeServers | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / arka uçlar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / sertifikalar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / tanılama | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / gruplar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / gruplar / kullanıcılar | group | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / kimlikSağlayıcılar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / loggers | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / bildirimler | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / bildirimler / recipientEmails | bildirim | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / openidConnectProviders | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / politikalar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / ürünler | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / ürünler / apis | ürün | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / ürün / gruplar | ürün | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | servis / ürünler / etiketler | ürün | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / özellikleri | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / abonelikler | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / etiketler | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / şablonlar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |
> | hizmet / kullanıcılar | hizmet | 1-256 | Kullanamıyorum:<br> `*#&+:<>?` |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | configurationMağazalar | kaynak grubu | 5-50 | Alfanümerikler, alt çizgi ve tireler. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Kilit | atama kapsamı | 1-90 | Alfanümerikler, dönemler, alt çizgi, tireler ve parantez.<br><br>Dönem sonu olmaz. |
> | ilke atamaları | atama kapsamı | 1-128 ekran adı<br><br>1-260 kaynak adı | Görüntü adı herhangi bir karakter içerebilir.<br><br>Kaynak adı dönem `%` veya boşlukla birlikte uzaya uzay yapamaz ve son veremez. |
> | politika tanımları | tanımının kapsamı | 1-128 ekran adı<br><br>1-260 kaynak adı | Görüntü adı herhangi bir karakter içerebilir.<br><br>Kaynak adı dönem `%` veya boşlukla birlikte uzaya uzay yapamaz ve son veremez. |
> | politikaSetDefinitions | tanımının kapsamı | 1-128 ekran adı<br><br>1-260 kaynak adı | Görüntü adı herhangi bir karakter içerebilir.<br><br>Kaynak adı dönem `%` veya boşlukla birlikte uzaya uzay yapamaz ve son veremez.  |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | otomasyonHesaplar | kaynak grubu | 6-50 | Alfanümerik ve tireler.<br><br>Harfle başla ve alfanümerik le bitir. |
> | otomasyonHesaplar / sertifikalar | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez.  |
> | otomasyonHesaplar / bağlantılar | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez. |
> | automationAccounts / kimlik bilgileri | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez. |
> | automationAccounts / runbooks | otomasyon hesabı | 1-63 | Alfanümerikler, alt çizgi ve tireler.<br><br>Mektupla başla.  |
> | otomasyonHesaplar / tarifeler | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez. |
> | otomasyonHesaplar / değişkenler | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez. |
> | automationAccounts / watchers | otomasyon hesabı | 1-63 |  Alfanümerikler, alt çizgi ve tireler.<br><br>Mektupla başla. |
> | automationAccounts / webhooks | otomasyon hesabı | 1-128 | Kullanamıyorum:<br> `<>*%&:\?.+/` <br><br>Uzayla bitemez. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | toplu Hesaplar | Bölge | 3-24 | Küçük harfler ve sayılar. |
> | batchAccounts / uygulamalar | toplu hesap | 1-64 | Alfanümerikler, alt çizgi ve tireler. |
> | batchAccounts / sertifikalar | toplu hesap | 5-45 | Alfanümerikler, alt çizgi ve tireler. |
> | batchAccounts / havuzlar | toplu hesap | 1-64 | Alfanümerikler, alt çizgi ve tireler. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | blockchainÜyeler | global | 2-20 | Küçük harfler ve sayılar.<br><br>Küçük harfle başla. |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | botServices | global | 2-64 |  Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. |
> | botServices / kanallar | bot servisi | 2-64 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. |
> | botServices / Bağlantılar | bot servisi | 2-64 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. |
> | kurumsalKanallar | kaynak grubu | 2-64 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. |

## <a name="microsoftcache"></a>Microsoft.Önbellek

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Redis | global | 1-63 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. Ardışık tirelere izin verilmiyor. |
> | Redis / güvenlik duvarıKuralları | Redis | 1-256 | Alfanümerik |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Profil | kaynak grubu | 1-260 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | profiller / uç noktalar | global | 1-50 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | sertifikaSiparişler | kaynak grubu | 3-30 | Alfanümerik. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | accounts | kaynak grubu | 2-64 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | availabilitySets | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfanümerik veya alt çizgi ile sona erer. |
> | diskŞifreleme Setleri | kaynak grubu | 1-80 | Alfanümerik ve alt çizgi. |
> | Disk | kaynak grubu | 1-80 | Alfanümerik ve alt çizgi. |
> | Galerileri | kaynak grubu | 1-80 | Alfanümerik ve periyotlar.<br><br>Alfanümerik ile başlayıp bitirin. |
> | galeriler / uygulamalar | galeri | 1-80 | Alfanümerikler, tireler ve periyotlar.<br><br>Alfanümerik ile başlayıp bitirin. |
> | galeriler / uygulamalar / sürümleri | uygulama | 32 bit'lik bir sayı | Sayılar ve dönemler. |
> | galeriler / resimler | galeri | 1-80 | Alfanümerikler, tireler ve periyotlar.<br><br>Alfanümerik ile başlayıp bitirin. |
> | galeriler / resimler / sürümler | image | 32 bit'lik bir sayı | Sayılar ve dönemler. |
> | images | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfanümerik veya alt çizgi ile sona erer. |
> | anlık görüntüler | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfanümerik veya alt çizgi ile sona erer. |
> | virtualMachines | kaynak grubu | 1-15 (Windows)<br>1-64 (Linux)<br><br>Aşağıdaki nota bakın. | Kullanamıyorum:<br> `\/""[]:|<>+=;,?*@&`<br><br>Alt la başlayamayız. Dönem ya da tire ile bitle bitemez. |
> | virtualMachineScaleSets | kaynak grubu | 1-15 (Windows)<br>1-64 (Linux)<br><br>Aşağıdaki nota bakın. | Kullanamıyorum:<br> `\/""[]:|<>+=;,?*@&`<br><br>Alt la başlayamayız. Dönem ya da tire ile bitle bitemez. |

> [!NOTE]
> Azure sanal makinelerinin iki farklı adı vardır: kaynak adı ve ana bilgisayar adı. Portalda sanal bir makine oluşturduğunuzda, her iki ad için de aynı değer kullanılır. Önceki tablodaki kısıtlamalar ana bilgisayar adı içindir. Gerçek kaynak adı en fazla 64 karakter uzunluğunda olabilir.

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | konteynerGruplar | kaynak grubu | 1-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. Ardışık tireler kullanılamaz. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Kayıt | global | 5-50 | Alfanümerik. |
> | kayıt defterleri / buildTasks | registry | 5-50 | Alfanümerik. |
> | kayıt defterleri / buildGörevler/adımlar | görev oluşturma | 5-50 | Alfanümerik. |
> | kayıt defterleri / çoğaltmalar | registry | 5-50 | Alfanümerik. |
> | kayıt defteri / scopeMaps | registry | 5-50 | Alfanümerikler, tireler ve alt çizgi. |
> | kayıt defterleri / görevleri | registry | 5-50 | Alfanümerikler, tireler ve alt çizgi. |
> | kayıt defterleri / belirteçleri | registry | 5-50 | Alfanümerikler, tireler ve alt çizgi. |
> | kayıt defterleri / webhooks | registry | 5-50 | Alfanümerik. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | yönetilen Kümeler | kaynak grubu | 1-63 | Alfanümerikler, alt çizgi ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | openShiftManagedClusters | kaynak grubu | 1-30 | Alfanümerik. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | hub'lar | kaynak grubu | 1-64 | Alfanümerik.<br><br>Mektupla başla.  |
> | hub'lar / yetkilendirmePolitikalar | Hub | 1-50 | Alfanümerikler, alt çizgi ve periyotlar.<br><br>Alfanümerik ile başlayıp bitirin. |
> | hub'lar / konektörler | Hub | 1-128 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub'lar / konektörler/eşlemeler | bağlayıcı | 1-128 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub'lar / etkileşimler | Hub | 1-128 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub / kpi | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub 'lar / bağlantılar | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub ' lar / tahminler | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub 'lar / profiller | Hub | 1-128 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hubs / ilişkiLinkler | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub 'lar / ilişkiler | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub'lar / roleAtamaları | Hub | 1-128 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |
> | hub 'lar / görünümler | Hub | 1-512 | Alfanümerik ve alt çizgi.<br><br>Mektupla başla. |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Dernek | kaynak grubu | 1-180 | Kullanamıyorum:<br>`%&\\?/`<br><br>Dönem ya da boşlukla bitemez. |
> | kaynakSağlayıcılar | kaynak grubu | 3-64 | Kullanamıyorum:<br>`%&\\?/`<br><br>Dönem ya da boşlukla bitemez. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Işleri | kaynak grubu | 3-24 | Alfanümerikler, tireler, alt çizgi ve periyotlar. |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | çalışma alanı | kaynak grubu | 3-30 | Alfanümerikler, alt çizgi ve tireler |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Fabrika | global | 3-63 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | fabrikalar / veri akışları | Fabrika | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |
> | fabrikalar / veri setleri | Fabrika | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |
> | fabrikalar / integrationRuntimes | Fabrika | 3-63 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | fabrikalar / bağlantılı hizmetler | Fabrika | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |
> | fabrikalar / boru hatları | Fabrika | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |
> | fabrikalar / tetikleyiciler | Fabrika | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |
> | fabrikalar / tetikleyiciler / rerunTriggers | Tetikleyici | 1-260 | Kullanamıyorum:<br>`<>*#.%&:\\+?/`<br><br>Alfanümerik ile başla. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Küçük harfler ve sayılar. |
> | hesaplar / computePolitikalar | account | 3-60 | Alfanümerikler, tireler ve alt çizgi. |
> | hesaplar / dataLakeStoreAccounts | account | 3-24 | Küçük harfler ve sayılar. |
> | hesaplar / firewallRules | account | 3-50 | Alfanümerikler, tireler ve alt çizgi. |
> | hesaplar / depolamaHesapları | account | 3-60 | Alfanümerikler, tireler ve alt çizgi. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Küçük harfler ve sayılar. |
> | hesaplar / firewallRules | account | 3-50 | Alfanümerikler, tireler ve alt çizgi. |
> | hesaplar / virtualNetworkRules | account | 3-50 | Alfanümerikler, tireler ve alt çizgi. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | services | kaynak grubu | 2-62 | Alfanümerikler, tireler, dönemler ve alt çizgi.<br><br>Alfanümerik ile başla. |
> | hizmetler / projeler | hizmet | 2-57 | Alfanümerikler, tireler, dönemler ve alt çizgi.<br><br>Alfanümerik ile başla. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Sunucu | global | 3-63 | Küçük harfler, tireler ve sayılar.<br><br>Tireile başlayıp bitiremezsin. |
> | sunucular / veritabanları | Sunucu | 1-63 | Alfanümerik ve tireler. |
> | sunucular / firewallRules | Sunucu | 1-128 | Alfanümerikler, tireler ve alt çizgi. |
> | sunucular / virtualNetworkRules | Sunucu | 1-128 | Alfanümerik ve tireler. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Sunucu | global | 3-63 | Küçük harfler, tireler ve sayılar.<br><br>Tireile başlayıp bitiremezsin. |
> | sunucular / veritabanları | Sunucu | 1-63 | Alfanümerik ve tireler. |
> | sunucular / firewallRules | Sunucu | 1-128 | Alfanümerikler, tireler ve alt çizgi. |
> | sunucular / virtualNetworkRules | Sunucu | 1-128 | Alfanümerik ve tireler. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Sunucu | global | 3-63 | Küçük harfler, tireler ve sayılar.<br><br>Tireile başlayıp bitiremezsin. |
> | sunucular / veritabanları | Sunucu | 1-63 | Alfanümerik ve tireler. |
> | sunucular / firewallRules | Sunucu | 1-128 | Alfanümerikler, tireler ve alt çizgi. |
> | sunucular / virtualNetworkRules | Sunucu | 1-128 | Alfanümerik ve tireler. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | IotHub'lar | global | 3-50 | Alfanümerik ve tireler.<br><br>Tire ile bitle bitemez. |
> | IotHubs / sertifikalar | IoT hub | 1-64 | Alfanümerikler, tireler, dönemler ve alt çizgi. |
> | IotHubs / eventHubEndpoints / ConsumerGroups | olayHubEndpoints | 1-50 | Alfanümerikler, tireler, dönemler ve alt çizgi. |
> | teminHizmetleri | kaynak grubu | 3-64 | Alfanümerik ve tireler.<br><br>Alfanümerik ile bitirin. |
> | sağlamaHizmetleri / sertifikaları | teminHizmetleri | 1-64 | Alfanümerikler, tireler, dönemler ve alt çizgi. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Labs | kaynak grubu | 1-50 | Alfanümerikler, alt çizgi ve tireler. |
> | laboratuvarlar / customimages | Lab | 1-80 | Alfanümerikler, alt çizgi, tire ve parantez. |
> | laboratuvarlar / formüller | Lab | 1-80 | Alfanümerikler, alt çizgi, tire ve parantez. |
> | laboratuvarlar / sanal makineler | Lab | 1-15 (Windows)<br>1-64 (Linux) | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. Hepsi sayı olamaz. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | veritabanıHesapları | global | 3-31 | Küçük harfler, sayılar ve tireler.<br><br>Küçük harf veya numara ile başlayın. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Etki alanları | kaynak grubu | 3-50 | Alfanümerik ve tireler. |
> | etki alanları / konular | etki alanı | 3-50 | Alfanümerik ve tireler. |
> | eventAbonelikler | kaynak grubu | 3-64 | Alfanümerik ve tireler. |
> | konuları | kaynak grubu | 3-50 | Alfanümerik ve tireler. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Küme | kaynak grubu | 6-50 | Alfanümerik ve tireler.<br><br>Mektupla başla. Harf veya sayı ile bitirin. |
> | Ad alanları | global | 6-50 | Alfanümerik ve tireler.<br><br>Mektupla başla. Harf veya sayı ile bitirin. |
> | ad alanları / Yetkilendirme Kuralları | ad alanı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Harf veya sayı ile başlayıp bitirin. |
> | namespaces / felaketRecoveryConfigs | ad alanı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Harf veya sayı ile başlayıp bitirin. |
> | ad alanları / eventhubs | ad alanı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Harf veya sayı ile başlayıp bitirin. |
> | namespaces / eventhubs / authorizationRules | olay merkezi | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Harf veya sayı ile başlayıp bitirin. |
> | ad alanları / eventhubs / tüketici grupları | olay merkezi | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Harf veya sayı ile başlayıp bitirin. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Küme | global | 3-59 | Alfanümerik ve tireler<br><br>Harf veya sayı ile başlayıp bitirin. |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Işleri | kaynak grubu | 2-64 | Alfanümerik ve tireler.<br><br>Mektupla başla. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Bileşen | kaynak grubu | 1-260 | Kullanamıyorum:<br>`%&\?/` <br><br>Uzay la ya da dönemle bitemez.  |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | IoTApps | global | 2-63 | Küçük harfler, sayılar ve tireler.<br><br>Küçük harf veya numara ile başlayın. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Tonoz | global | 3-24 | Alfanümerik ve tireler.<br><br>Mektupla başla. Harf veya rakamla bitirin. Ardışık tireleri içeremez. |
> | kasalar / sırlar | Kasa | 1-127 | Alfanümerik ve tireler. |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Küme | global | 4-22 | Küçük harfler ve sayılar.<br><br>Mektupla başla. |
> | /kümeler / veritabanları | cluster | 1-260 | Alfanümerikler, tireler, boşluklar ve dönemler. |
> | /kümeler / veritabanları / dataConnections | database | 1-40 | Alfanümerikler, tireler, boşluklar ve dönemler. |
> | /kümeler / veritabanları / eventhubconnections | database | 1-40 | Alfanümerikler, tireler, boşluklar ve dönemler. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | entegrasyonHesapları | kaynak grubu | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / derlemeleri | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / toplu Yapılandırmalar | tümleştirme hesabı | 1-20 | Alfanümerik. |
> | entegrasyonHesapları / sertifikaları | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / haritalar | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / ortakları | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / rosettanetprocessconfigurations | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesapları / şemaları | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonHesaplar / oturumlar | tümleştirme hesabı | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |
> | entegrasyonServiceOrtamlar | kaynak grubu | 1-80 | Alfanümerikler, tireler, dönemler ve alt çizgi. |
> | entegrasyonServiceEnvironments / managedApis | entegrasyon hizmet ortamı | 1-80 | Alfanümerikler, tireler, dönemler ve alt çizgi. |
> | Iş akışı | kaynak grubu | 1-80 | Alfanümerikler, tireler, alt çizgi, dönemler ve parantez. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | taahhütPlanlar | kaynak grubu | 1-260 | Kullanamıyorum:<br>`<>*%&:?+/\\`<br><br>Boşlukla bitemez. |
> | Webservices | kaynak grubu | 1-260 | Kullanamıyorum:<br>`<>*%&:?+/\\`<br><br>Boşlukla bitemez. |
> | çalışma alanı | kaynak grubu | 1-260 | Kullanamıyorum:<br>`<>*%&:?+/\\`<br><br>Boşlukla bitemez. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | çalışma alanı | kaynak grubu | 3-33 | Alfanümerik ve tireler. |
> | çalışma alanları / hesaplamalar | çalışma alanı | 2-16 | Alfanümerik ve tireler. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | kullanıcıAssignedIdentities | kaynak grubu | 3-128 | Alfanümerikler, tireler ve alt çizgi<br><br>Harf veya sayı ile başlayın. |

## <a name="microsoftmaps"></a>Microsoft.Haritalar

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | accounts | kaynak grubu | 1-98 (kaynak grubu adı ve hesap adı için) | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | medya hizmetleri | kaynak grubu | 3-24 | Küçük harfler ve sayılar. |
> | mediaservices / liveEvents | Medya hizmeti | 1-32 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başla. |
> | mediaservices / liveEvents / liveOutputs | Canlı etkinlik | 1-256 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başla. |
> | mediaservices / streamingEndpoints | Medya hizmeti | 1-24 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başla. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | uygulamaAğ Ağ Geçitleri | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | uygulamaGüvenlikGrupları | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | azureFirewalls | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfanümerik veya alt çizgi ile sona erer. |
> | burcuHosts | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | Bağlantı | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | dnsZones | kaynak grubu | 1-63 karakter<br><br>2 ile 34 arasında etiket<br><br>Her etiket bir döneme göre ayrılmış bir karakter kümesidir. Örneğin, **contoso.com** 2 etiketi vardır. | Her etiket alfasayısal, alt çizgi ve tire içerebilir.<br><br>Her etiket bir döneme ayrılır. |
> | expressRouteCircuits | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | güvenlik duvarıPolitikalar | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | güvenlik duvarıPolitikalar / ruleGroups | güvenlik duvarı ilkesi | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | frontDoors | global | 5-64 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | yükDengeleyiciler | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | loadBalancers / gelenNatRules | yük dengeleyici | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | yerelAğ Ağ Geçitleri | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | ağArayüzler | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | ağSecurityGroups | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | ağSecurityGroups / securityRules | ağ güvenlik grubu | 1-80 |  Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | ağ Watchers | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | privateDnsZones | kaynak grubu | 1-63 karakter<br><br>2 ile 34 arasında etiket<br><br>Her etiket bir döneme göre ayrılmış bir karakter kümesidir. Örneğin, **contoso.com** 2 etiketi vardır. | Her etiket alfasayısal, alt çizgi ve tire içerebilir.<br><br>Her etiket bir döneme ayrılır. |
> | privateDnsZones / virtualNetworkLinks | özel DNS bölgesi | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | publicIPAdresleri | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | publicIPPrefixes | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | rotaFiltreler | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | rotaFiltreler / rotaFilterRules | rota filtresi | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | routeTablolar | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | routeTables / rotalar | yol tablosu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | serviceEndpointPolitikalar | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | trafik yöneticisi profilleri | global | 1-63 | Alfanümerikler, tireler ve periyotlar.<br><br>Alfanümerik ile başlayıp bitirin. |
> | virtualNetworkGateways | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | sanalAğlar | kaynak grubu | 2-64 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | sanal ağlar / alt ağlar | sanal ağ | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | virtualNetworks / virtualNetworkPeerings | sanal ağ | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | sanalWans | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | vpnAğ Geçitleri | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | vpnGateways / vpnConnections | VPN ağ geçidi | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |
> | vpnSiteler | kaynak grubu | 1-80 | Alfanümerikler, alt çizgi, periyotlar ve tireler.<br><br>Alfanümerik ile başla. Alfasayısal veya alt çizgiyi bitirin. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHub'lar

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Ad alanları | global | 6-50 | Alfanümerik ve tireler<br><br>Alfanümerik ile başlayıp bitirin. |
> | ad alanları / Yetkilendirme Kuralları | ad alanı | 1-256 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik başlatın. |
> | ad alanları / notificationHubs | ad alanı | 1-260 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik başlatın. |
> | namespaces / notificationHubs / AuthorizationRules | bildirim merkezi | 1-256 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik başlatın. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Küme | kaynak grubu | 4-63 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | çalışma alanı | kaynak grubu | 4-63 | Alfanümerik ve tireler.<br><br>Alfanümerik ile başlayıp bitirin. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | panolar | kaynak grubu | 3-160 | Alfanümerik ve tireler.<br><br>Kısıtlanmış karakterleri kullanmak için, kullanmak istediğiniz pano adı ile **gizli başlık** adlı bir etiket ekleyin. Portal, panoyu gösterirken bu adı görüntüler. |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | çalışma alanıKoleksiyonlar | region | 3-63 | Alfanümerik ve tireler.<br><br>Tireile başlamıyorum. Ardışık tireler kullanamaz. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Kapasite | region | 3-63 | Küçük harfler veya sayılar<br><br>Küçük harfle başla. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Tonoz | kaynak grubu | 2-50 | Alfanümerik ve tireler.<br><br>Mektupla başla. |
> | tonozlar / yedeklemePolitikalar | kasa | 3-150 | Alfanümerik ve tireler.<br><br>Mektupla başla. Tire ile bitle bitemez. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Ad alanları | global | 6-50 | Alfanümerik ve tireler.<br><br>En başta bir harf kullanın. Bir harf veya sayı ile bitirin. |
> | ad alanları / Yetkilendirme Kuralları | ad alanı | 1-50 |  Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik ile başlayıp bitirin. |
> | ad boşlukları / HybridConnections | ad alanı | 1-260 | Alfanümerikler, periyotlar, tireler, alt çizgiler ve kesikler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | ad alanları / HybridConnections/authorizationRules | hibrid bağlantı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik ile başlayıp bitirin. |
> | isim boşlukları / WcfRelays | ad alanı | 1-260 | Alfanümerikler, periyotlar, tireler, alt çizgiler ve kesikler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | namespaces / WcfRelays / authorizationRules | Wcf röle | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfanümerik ile başlayıp bitirin. |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Dağıtım | kaynak grubu | 1-64 | Alfanümerikler, alt çizgi, parantezler, tireler ve dönemler. |
> | kaynak grupları | aboneliği | 1-90 | Alfanümerikler, alt çizerler, parantezler, tireler, dönemler ve [regex belgeleriyle](/rest/api/resources/resourcegroups/createorupdate)eşleşen unicode karakterleri.<br><br>Dönemle bitemez. |
> | tagNames | kaynak | 1-512 | Kullanamıyorum:<br>`<>%&\?/` |
> | tagNames / tagValues | etiket adı | 1-256 | Tüm karakterler. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Ad alanları | global | 6-50 | Alfanümerik ve tireler.<br><br>En başta bir harf kullanın. Bir harf veya sayı ile bitirin.<br><br>Daha fazla bilgi için [bkz.](/rest/api/servicebus/create-namespace) |
> | ad alanları / Yetkilendirme Kuralları | ad alanı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfenumerik ile başlayıp bitir. |
> | namespaces / felaketRecoveryConfigs | global | 6-50 | Alfanümerik ve tireler.<br><br>Mektupla başla. Alfanümerik ile bitirin. |
> | namespaces / migrationConfigurations | ad alanı |  | Her zaman **$default.** |
> | ad alanları / kuyruklar | ad alanı | 1-260 | Alfanümerikler, periyotlar, tireler, alt çizgiler ve kesikler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | ad alanları / kuyruklar / yetkilendirmeKuralları | kuyruk | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfenumerik ile başlayıp bitir. |
> | namespaces / konular | ad alanı | 1-260 | Alfanümerikler, periyotlar, tireler, alt çizgiler ve kesikler.<br><br>Alfanümerik ile başlayıp bitirin. |
> | namespaces / konular / yetkilendirmeKurallar | konu başlığı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfenumerik ile başlayıp bitir. |
> | ad alanları / konular / abonelikler | konu başlığı | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfenumerik ile başlayıp bitir. |
> | namespaces / konular / abonelikler / kurallar | aboneliği | 1-50 | Alfanümerikler, periyotlar, tireler ve alt çizgi.<br><br>Alfenumerik ile başlayıp bitir. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Küme | region | 4-23 | Küçük harfler, sayılar ve tireler.<br><br>Küçük harfle başla. Küçük harf veya sayı ile bitirin. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | sinyalR | global | 3-63 | Alfanümerik ve tireler.<br><br>Mektupla başla. Harf veya sayı ile bitirin.  |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | yönetilen Örnekler | global | 1-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. |
> | Sunucu | global | 1-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. |
> | sunucular / yöneticiler | sunucu |  | Olmalı. `ActiveDirectory` |
> | sunucular / veritabanları | sunucu | 1-128 | Kullanamıyorum:<br>`<>*%&:\/?`<br><br>Dönem ya da boşlukla bitemez. |
> | sunucular / veritabanları / syncGroups | database | 1-150 | Alfanümerikler, tireler ve alt çizgi. |
> | sunucular / elasticPools | sunucu | 1-128 | Kullanamıyorum:<br>`<>*%&:\/?`<br><br>Dönem ya da boşlukla bitemez. |
> | sunucular / failoverGroups | global | 1-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. |
> | sunucular / firewallRules | sunucu | 1-128 | Kullanamıyorum:<br>`<>*%&:;\/?`<br><br>Dönemle bitemez. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | depolamaHesapları | global | 3-24 | Küçük harfler ve sayılar. |
> | storageAccounts / blobServices | depolama hesabı |  | Olmalı. `default` |
> | storageAccounts / blobServices / konteynerler | depolama hesabı | 3-63 | Küçük harfler, sayılar ve tireler.<br><br>Küçük harf veya numara ile başlayın. Ardışık tireler kullanamaz. |
> | storageAccounts / fileServices | depolama hesabı |  | Olmalı. `default` |
> | storageAccounts / fileServices / hisse | depolama hesabı | 3-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. Ardışık tireler kullanamaz. |
> | storageAccounts / managementPolitikalar | depolama hesabı |  | Olmalı. `default` |
> | blob | kapsayıcı | 1-1024 | Herhangi bir URL karakteri, büyük/küçük harf duyarlı |
> | kuyruk | depolama hesabı | 3-63 | Küçük harfler, sayılar ve tireler.<br><br>Tireile başlayıp bitiremezsin. Ardışık tireler kullanamaz. |
> | tablo | depolama hesabı | 3-63 | Alfanümerik.<br><br>Mektupla başla. |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | depolamaSyncServices | kaynak grubu | 1-260 | Alfanümerikler, boşluklar, dönemler, tireler ve alt çizgi.<br><br>Dönem ya da boşlukla bitemez. |
> | storageSyncServices / syncGroups | depolama eşitleme hizmeti | 1-260 | Alfanümerikler, boşluklar, dönemler, tireler ve alt çizgi.<br><br>Dönem ya da boşlukla bitemez. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Yöneticileri | kaynak grubu | 2-50 | Alfanümerik ve tireler.<br><br>Mektupla başla. Alfanümerik ile bitirin. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | akış işleri | kaynak grubu | 3-63 | Alfanümerikler, tireler ve alt çizgi. |
> | streamingjobs / fonksiyonlar | akış işi | 3-63 | Alfanümerikler, tireler ve alt çizgi. |
> | akış işleri / girişleri | akış işi | 3-63 | Alfanümerikler, tireler ve alt çizgi. |
> | akış işleri / çıkışları | akış işi | 3-63 | Alfanümerikler, tireler ve alt çizgi. |
> | streamingjobs / dönüşümler | akış işi | 3-63 | Alfanümerikler, tireler ve alt çizgi. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | Ortam | kaynak grubu | 1-90 | Kullanamıyorum:<br>`'<>%&:\?/#` |
> | ortamlar / accessPolitikalar | environment | 1-90 | Kullanamıyorum:<br> `'<>%&:\?/#` |
> | ortamlar / olayKaynaklar | environment | 1-90 | Kullanamıyorum:<br>`'<>%&:\?/#` |
> | ortamlar / referenceDataSets | environment | 3-63 | Alfanümerik |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Varlık | Kapsam | Uzunluk | Geçersiz Karakterler |
> | --- | --- | --- | --- |
> | serverfarms | kaynak grubu | 1-40 | Alfanümerik ve tireler. |
> | Siteler | global | 2-60 | Alfanümerik ve tire içerir.<br><br>Tireile başlayıp bitiremezsin. |
> | siteler / yuvalar | Site | 2-59 | Alfanümerik ve tireler. |

## <a name="next-steps"></a>Sonraki adımlar

Kaynakların nasıl adlandırılmasıyla ilgili öneriler için bkz: [Hazır: Önerilen adlandırma ve etiketleme kuralları.](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)
