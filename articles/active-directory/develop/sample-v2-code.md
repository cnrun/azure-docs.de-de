---
title: Codebeispiele für die Microsoft Identity Platform
description: Dieser Artikel enthält einen Index der verfügbaren Microsoft Identity Platform-Codebeispiele (v2.0-Endpunkt), geordnet nach Szenario.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: sample
ms.workload: identity
ms.date: 06/01/2020
ms.author: marsma
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 948eea168b071813cf20e03d3a0744a824fd4c8c
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91773891"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft Identity Platform – Codebesipiele (V2.0-Endpunkt)

Sie können mit der Microsoft Identity Platform folgende Aktionen ausführen:

- Hinzufügen von Authentifizierungs- und Autorisierungsfeatures zu Ihren Webanwendungen und Web-APIs
- Anfordern eines Zugriffstokens für den Zugriff auf eine geschützte Web-API

In diesem Artikel wird der Microsoft Identity Platform-Endpunkt kurz erläutert, und es werden Links zu Beispielen für den Endpunkt angegeben. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und es werden Codeausschnitte bereitgestellt, die Sie in Ihren Anwendungen verwenden können. Auf der Seite mit den Codebeispielen finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem enthält der Code Kommentare, die die wichtigsten Abschnitte besser verständlich machen.

> [!NOTE]
> Wenn Sie an Beispielen zu v1.0 interessiert sind, sehen Sie unter [Azure AD-Codebeispiele (v1.0-Endpunkt)](../azuread-dev/sample-v1-code.md) nach.

Informationen zum grundlegenden Szenario für die einzelnen Beispieltypen finden Sie unter [App-Typen für den Microsoft Identity Platform-Endpunkt](v2-app-types.md).

Sie können auch Beiträge zu den Beispielen auf GitHub schreiben. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Single-Page-Webanwendungen

Diese Beispiele zeigen, wie Sie eine Single-Page-Webanwendung schreiben, die mit Microsoft Identity Platform geschützt ist. Diese Beispiele verwenden eine der Arten von MSAL.js.

| Plattform | BESCHREIBUNG | Link |
| -------- | --------------------- | -------- |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | SPA ruft Microsoft Graph auf |[javascript-graphapi-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2) |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser) | SPA ruft Microsoft Graph mithilfe des Authentifizierungscodeflows mit PKCE auf |[javascript-v2](https://github.com/Azure-Samples/ms-identity-javascript-v2) |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | SPA ruft B2C auf |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL-Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| SPA ruft Microsoft Graph auf  | [active-directory-javascript-singlepageapp-angular](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [Angular (MSAL-Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| SPA ruft benutzerdefinierte Web-API auf | [ms-identity-javascript-angular-spa-aspnetcore-webapi](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi) |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [Angular (MSAL-Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) | SPA ruft B2C auf |[active-directory-b2c-javascript-angular-spa](https://github.com/Azure-Samples/active-directory-b2c-javascript-angular-spa) |
| ![Diese Abbildung zeigt das React-Logo.](media/sample-v2-code/logo_react.png) [React (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core)| SPA ruft benutzerdefinierte Web-API auf, die wiederum Microsoft Graph aufruft  | [ms-identity-javascript-react-spa-dotnetcore-webapi-obo](https://github.com/Azure-Samples/ms-identity-javascript-react-spa-dotnetcore-webapi-obo) |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [Angular (MSAL-Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) | SPA ruft benutzerdefinierte Web-API mit App-Rollen und -Sicherheitsgruppen auf |[ms-identity-javascript-angular-spa-dotnetcore-webapi-roles-groups](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-dotnetcore-webapi-roles-groups) |

## <a name="web-applications"></a>Webanwendungen

Die folgenden Beispiele zeigen Webanwendungen, die Benutzer anmelden. Einige Beispiele zeigen außerdem, wie die Anwendung Microsoft Graph oder Ihre eigene Web-API mit der Identität des Benutzers aufruft.

| Plattform | Nur Benutzeranmeldung | Benutzeranmeldung und Aufruf von Microsoft Graph |
| -------- | ------------------- | --------------------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | [Benutzertutorial für die ASP.NET Core-Web-App-Anmeldung](https://aka.ms/aspnetcore-webapp-sign-in) | Das gleiche Beispiel im Schritt [ASP.NET Core-Web-App-Aufrufe an Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph) |
| ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [Schnellstart für ASP.NET](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  |                   | [ms-identity-java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp) |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | [ms-identity-python-flask-webapp-authentication](https://github.com/Azure-Samples/ms-identity-python-flask-webapp-authentication) | [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp) |
| ![Diese Abbildung zeigt das Ruby-Logo.](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Öffentliche Desktop- und mobile Client-Apps

In den folgenden Beispielen werden öffentliche Clientanwendungen (Desktop-/mobile Anwendungen) veranschaulicht, die im Namen eines Benutzers auf die Microsoft Graph-API oder Ihre eigene Web-API zugreifen. Mit Ausnahme des Beispiels *Desktop (Konsole) mit WAM* verwenden alle diese Clientanwendungen die Microsoft Authentication Library (MSAL).

| Clientanwendung | Plattform | Flow/Zuweisung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET Core-Web-API |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop (WPF)      | ![Diese Abbildung zeigt das .NET/C#-Logo.](media/sample-v2-code/logo_NET.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NET.png) | [Integrierte Windows-Authentifizierung](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Integrierte Windows-Authentifizierung](msal-authentication-flows.md#integrated-windows-authentication) |[ms-identity-java-desktop](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Desktop (Konsole) mit WAM  | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | Interaktiv mit [Web Account Manager](/windows/uwp/security/web-account-manager) (WAM) |[dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[ms-identity-java-desktop](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[ms-identity-python-desktop](https://github.com/Azure-Samples/ms-identity-python-desktop) |  |
| Mobil (Android, iOS, UWP)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Xamarin).](media/sample-v2-code/logo_xamarin.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobil (iOS)       | ![Diese Abbildung zeigt das iOS-/Objective-C- oder Swift-Logo.](media/sample-v2-code/logo_iOS.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[ios-swift-objc-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Desktop (macOS)       | macOS | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[macOS-swift-objc-native-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Mobil (Android-Java)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |  [android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Mobil (Android-Kotlin)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |  [android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Daemonanwendungen

Die folgenden Beispiele zeigen eine Anwendung, die auf die Microsoft Graph-API mit ihrer eigenen Identität (ohne Benutzer) zugreift.

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Konsole | ![Diese Abbildung zeigt das .NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web-App | ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |
| Konsole | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [ms-identity-java-daemon](https://github.com/Azure-Samples/ms-identity-java-daemon) |
| Konsole | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [ms-identity-python-daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) |

## <a name="headless-applications"></a>Monitorlose Anwendungen

In den folgenden Beispielen wird eine öffentliche Clientanwendung veranschaulicht, die auf einem Gerät ohne Webbrowser ausgeführt wird. Diese App kann ein Befehlszeilentool sein oder unter Linux oder Mac ausgeführt werden, oder es kann sich um eine IoT-Anwendung handeln. In den Beispielen wird eine App verwendet, die im Namen eines Benutzers, der sich interaktiv an einem anderen Gerät anmeldet (z. B. einem Mobiltelefon), auf die Microsoft Graph-API zugreift. Diese Clientanwendung verwendet die Microsoft Authentication Library (MSAL).

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[ms-identity-java-devicecodeflow](https://github.com/Azure-Samples/ms-identity-java-devicecodeflow) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[ms-identity-python-devicecodeflow](https://github.com/Azure-Samples/ms-identity-python-devicecodeflow) |

## <a name="multi-tenant-saas-applications"></a>SaaS-Anwendungen mit mehreren Mandanten

In den folgenden Beispielen wird gezeigt, wie Sie Ihre Anwendung so konfigurieren, dass Anmeldungen von einem beliebigen Azure AD-Mandanten (Azure Active Directory) akzeptiert werden. Wenn Sie Ihre Anwendung als *mehrinstanzenfähig* konfigurieren, bedeutet das, dass Sie eine **SaaS-Anwendung (Software-as-a-Service)** für viele Organisationen anbieten können. Die Benutzer können sich dann bei Ihrer Anwendung anmelden, nachdem sie der Verwendung ihres Kontos zugestimmt haben.

| Plattform | Beschreibung | Link |
| -------- | --------------------- | -------- |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser) | Mehrinstanzenfähige SPA ruft Graph-API auf. |[ms-identity-javascript-angular-spa-aspnet-webapi-multitenant](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter1) |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [Angular (MSAL-Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) | Mehrinstanzenfähige SPA ruft mehrinstanzenfähige benutzerdefinierte Web-API auf |[ms-identity-javascript-angular-spa-aspnet-webapi-multitenant](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter2) |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png) [.NET Core (MSAL.NET)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | ASP.NET Core-MVC-Webanwendung ruft Graph-API auf. |[active-directory-aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-3-Multi-Tenant) |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png) [.NET Core (MSAL.NET)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | ASP.NET Core-MVC-Webanwendung ruft ASP.NET Core-Web-API auf. |[active-directory-aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-3-AnyOrg) |

## <a name="web-apis"></a>Web-APIs

Die folgenden Beispiele veranschaulichen, wie Sie eine Web-API mit dem Microsoft Identity-Plattform-Endpunkt schützen und wie Sie eine nachgeschaltete API von der Web-API aus aufrufen.

| Plattform | Beispiel |
| -------- | ------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | ASP.NET Core-Web-API (Dienst) von [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web-API (Dienst) von [ms-identity-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | Web-API (Dienst) von [ms-identity-java-webapi](https://github.com/Azure-Samples/ms-identity-java-webapi) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png) | Web-API (Dienst) von [active-directory-javascript-nodejs-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png) | B2C-Web-API (Dienst) von [active-directory-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) |

## <a name="azure-functions-as-web-apis"></a>Azure Functions als Web-APIs

Die folgenden Beispiele veranschaulichen, wie Sie mit „HttpTrigger“ eine Azure-Funktion schützen, mit dem Microsoft Identity-Plattform-Endpunkt eine Web-API verfügbar machen und über die Web-API eine nachgeschaltete API aufrufen.

| Plattform | Beispiel |
| -------- | ------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | ASP.NET Core-Web-API (Dienst)-Azure-Funktion von [dotnet-native-aspnetcore-v2](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)  |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | Web-API (Dienst) von [NodeJS und Passport-Azure-AD](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)</p>Python | Web-API (Dienst) von [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | Web-API (Dienst) von [NodeJS und Passport-Azure-AD mithilfe von OnBehalfOf](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions) |

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

[Beispiele](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) und Tutorials, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Beispiele und Tutorials der Microsoft Graph-Community](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

- [Entwicklerhandbuch zu Azure Active Directory (v1.0)](../azuread-dev/v1-overview.md)
- [Microsoft Graph-API – Konzepte und Referenz](/graph/use-the-api?context=graph%2fapi%2fbeta&view=graph-rest-beta)
