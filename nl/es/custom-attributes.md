---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-03"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}

# Acceso a los atributos personalizados de usuario
{: #custom}

Cuando obtenga una señal de acceso, es posible obtener acceso al punto final de los atributos protegidos del usuario.
{: shortdesc}

{{site.data.keyword.appid_short_notm}} proporciona una <a href="https://appid-profiles.ng.bluemix.net/swagger-ui/index.html#/Attributes" target="_blank">API REST <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> que permite el inicio de sesión, de forma anónima o mediante autenticación, con un [proveedor de identidad](/docs/services/appid/identity-providers.html) admitido. El punto final de API es un recurso protegido por una señal de acceso, que genera {{site.data.keyword.appid_short_notm}} durante el proceso de inicio de sesión y autorización.

</br>

## Acceso con el SDK de iOS
{: #ios}

 Puede acceder a los atributos personalizados pasando una señal de acceso mediante los siguientes métodos de API.

  ```swift
  func setAttribute(key: String, value: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func setAttribute(key: String, value: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func getAttribute(key: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func getAttribute(key: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func getAttributes(completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func getAttributes(accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func deleteAttribute(key: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func deleteAttribute(key: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  ```
  {: pre}

Cuando no se haya aprobado explícitamente una señal de acceso, {{site.data.keyword.appid_short_notm}} utilizará la última señal recibida.

Por ejemplo, puede llamar al código siguiente para establecer un atributo nuevo, o para alterar temporalmente uno existente.

  ```swift
	AppID.sharedInstance.userProfileManager?.setAttribute("key", "value") { (error, result) in
		guard let result = result, error == nil else {
	  		return //se ha producido un error
		}
		//atributos recibidos como diccionario
	})
  ```
  {: pre}

  Para obtener más información sobre cómo trabajar en iOS Swift, consulte el <a href="https://github.com/ibm-cloud-security/appid-clientsdk-swift" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
  {: tip}

</br>


## Acceso con el SDK de Android
{: #android}

Puede acceder a los atributos personalizados pasando una señal de acceso mediante los siguientes métodos de API.

```java
void setAttribute(@NonNull String name, @NonNull String value, UserAttributeResponseListener listener);
  void setAttribute(@NonNull String name, @NonNull String value, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void getAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void getAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void deleteAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void deleteAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void getAllAttributes(@NonNull UserAttributeResponseListener listener);
  void getAllAttributes(@NonNull AccessToken accessToken, @NonNull UserAttributeResponseListener listener);
```
{: pre}

Cuando no se haya aprobado explícitamente una señal de acceso, {{site.data.keyword.appid_short_notm}} utilizará la última señal recibida.

Por ejemplo, puede llamar al código siguiente para establecer un atributo nuevo, o para alterar temporalmente uno existente.

```java
appId.getUserProfileManager().setAttribute(name, value, useThisToken, new UserProfileResponseListener() {
	@Override
		public void onSuccess(JSONObject attributes) {
		//atributos recibidos en formato JSON con una respuesta satisfactoria
		}

	@Override
		public void onFailure(UserAttributesException e) {
		//se ha producido una excepción
	}
});
```
{: pre}

Para obtener más información sobre cómo trabajar en Android, consulte el <a href="https://github.com/ibm-cloud-security/appid-clientsdk-android" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
{: tip}

</br>

## Acceso con el SDK del servidor de Swift
{: #swift}

Puede acceder a los atributos personalizados pasando una señal de acceso mediante los siguientes métodos de API.

  ```swift
  func getAllAttributes(accessToken: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func getAttribute(accessToken: String, attributeName: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func setAttribute(accessToken: String, attributeName: String, attributeValue : "abc", completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func deleteAllAttributes(accessToken: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  ```
  {: pre}

  Ejemplo de uso:

  ```swift

	let userProfileManager = UserProfileManager(options: options)
	let accesstoken = "access token"

	userProfileManager.setAttribute(accessToken: accessToken, attributeName: "name", attributeValue : "abc") { (error, response) in
		guard let response = response, error == error else {
			return //se ha producido un error
		}
		//atributos recibidos como diccionario
	}
  ```

  {: pre}

  Para obtener más información sobre cómo trabajar en el SDK del servidor de Swift, consulte el <a href="https://github.com/ibm-cloud-security/appid-serversdk-swift" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
  {: tip}

</br>

## Acceso con el SDK del servidor de Node.js
{: #node}

Puede acceder a los atributos personalizados pasando una señal de acceso mediante los siguientes métodos de API.

  ```javascript
	function getAllAttributes(accessTokenString) {}
	function getAttribute(accessTokenString, key) {}
	function setAttribute(accessTokenString, key, value) {}
	function deleteAttribute(accessTokenString, name) {}
  ```
  {: pre}

  Ejemplo de uso:

  ```javascript

	const userProfileManager = require("ibmcloud-appid").UserProfileManager;
	userProfileManager.init();

	var accessToken = req.session[WebAppStrategy.AUTH_CONTEXT].accessToken;

	userProfileManager.setAttribute(accessToken, name, value).then(function (attributes) {
		//atributos devueltos como diccionario
	});
  ```
  {: pre}

  Para obtener más información sobre cómo trabajar en el SDK del servidor de Node.js, consulte el <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
  {: tip}



