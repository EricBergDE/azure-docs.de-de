---
title: Azure Container Registry-Authentifizierung mit Dienstprinzipalen
description: "Erfahren Sie, wie Sie Zugriff auf Images in Ihrer privaten Containerregistrierung gewähren, indem Sie einen Azure Active Directory-Dienstprinzipal verwenden."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 97036ecabceb12b87b76c6ecb7e521157cbef827
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container Registry-Authentifizierung mit Dienstprinzipalen

Sie können einen Dienstprinzipal von Azure Active Directory (Azure AD) verwenden, um den Containerimages `docker push` und `pull` den Zugriff auf Ihre Containerregistrierung zu gewähren. Mithilfe eines Dienstprinzipals können Sie den Zugriff für „monitorlose“ Dienste und Anwendungen ermöglichen.

## <a name="what-is-a-service-principal"></a>Was ist ein Dienstprinzipal?

*Dienstprinzipale* von Azure AD ermöglichen den Zugriff auf Azure-Ressourcen innerhalb Ihres Abonnements. Sie können sich einen Dienstprinzipal als Benutzeridentität für einen Dienst vorstellen, wobei „Dienst“ jede Anwendung, jeder Dienst oder jede Plattform sein kann, die Zugriff auf die Ressourcen benötigt. Sie haben die Möglichkeit, einen Dienstprinzipal mit Zugriffsrechten so zu konfigurieren, dass diese nur für die von Ihnen angegebenen Ressourcen gelten. Anschließend können Sie für Ihre Anwendung bzw. Ihren Dienst festlegen, dass diese bzw. dieser die Anmeldeinformationen des Dienstprinzipals verwendet, um auf diese Ressourcen zuzugreifen.

Im Rahmen der Azure Container Registry können Sie einen Azure AD-Dienstprinzipal mit Pull-, Push- und Pull- oder Besitzerberechtigungen für Ihre private Docker-Registrierung in Azure erstellen.

## <a name="why-use-a-service-principal"></a>Gründe für die Verwendung eines Dienstprinzipals

Mithilfe eines Azure AD-Dienstprinzipals können Sie bereichsbezogenen Zugriff auf Ihre private Containerregistrierung gewähren. Für Ihre einzelnen Anwendungen oder Dienste können Sie verschiedene Dienstprinzipale erstellen, die jeweils über abgestimmte Zugriffsrechte für die Registrierung verfügen. Und da Sie die Anmeldeinformationen nicht an Dienste und Anwendungen weitergeben müssen, haben Sie die Möglichkeit, ausschließlich für den gewählten Dienstprinzipal (und somit für die Anwendung) Anmeldeinformationen weiterzugeben oder Zugriffsrechte zu widerrufen.

Beispielsweise kann Ihre Webanwendung einen Dienstprinzipal verwenden, der ihr nur Zugriff auf Image `pull` ermöglicht, während Ihr Buildsystem einen Dienstprinzipal verwenden kann, der ihm sowohl Zugriff auf `push` als auch `pull` bietet. Wenn die Entwicklung Ihrer Anwendung von einer anderen Person übernommen wird, können Sie die Anmeldeinformationen für den Dienstprinzipal weitergeben, ohne das Buildsystem zu beeinflussen.

## <a name="when-to-use-a-service-principal"></a>Einsatzbereiche eines Dienstprinzipals

Sie sollten einen Dienstprinzipal verwenden, um in **monitorlosen Szenarien** Zugriff auf die Registrierung bieten. Das heißt, dies betrifft jede Anwendung, jeden Dienst oder jedes Skript, das Containerimages in automatisierter oder anderweitig unbeaufsichtigter Weise pushen oder pullen muss.

Für den individuellen Zugriff auf eine Registrierung, z.B. wenn Sie manuell ein Containerimage auf Ihre Entwicklungsarbeitsstation pullen, sollten Sie stattdessen Ihre eigene [Azure AD-Identität](container-registry-authentication.md#individual-login-with-azure-ad) für den Registrierungszugriff verwenden (z.B. mit [az acr login][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="next-steps"></a>Nächste Schritte

Sobald ein Dienstprinzipal erstellt wurde, dem Sie Zugriff auf Ihre Containerregistrierung gewährt haben, können Sie seine Anmeldeinformationen in Ihren Anwendungen und Diensten für die Interaktion mit der Registrierung verwenden.

Das Thema der Konfiguration einzelner Anwendungen zur Verwendung von Anmeldeinformationen des Dienstprinzipals ist für diesen Artikel zu umfangreich. Daher finden Sie hier Anweisungen für einige spezifische Dienste und Plattformen:

* [Authentifizieren per Azure Container Registry über den Azure Container Service](container-registry-auth-aks.md)
* [Authentifizieren per Azure Container Registry über Azure Container Instances (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az_acr_login