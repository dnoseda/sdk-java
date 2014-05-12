# MercadoPago SDK module for Payments integration

* [Install](#install)
* [Usage](#usage)
* [Using MercadoPago Checkout](#checkout)
* [Using MercadoPago Payment collection](#payments)

<a name="install"></a>
## Install:

Install it using maven:

Just add to your pom.xml the following repository

```XML
<repositories>
    ...
    <repository>
        <id>mercadopago</id>
        <url>https://github.com/mercadopago/sdk-java/raw/master/releases</url>
    </repository>
    ...
</repositories>  
```

Then add the dependency

```XML
 <dependencies>
    ...
    <dependency>
        <groupId>com.mercadopago</groupId>
        <artifactId>sdk</artifactId>
        <version>0.2.1</version>
    </dependency>
    ...
</dependencies>
```
And that's it!

<a name="usage"></a>
## Usage:

* Get your **CLIENT_ID** and **CLIENT_SECRET** in the following address:
    * Argentina: [https://www.mercadopago.com/mla/herramientas/aplicaciones](https://www.mercadopago.com/mla/herramientas/aplicaciones)
    * Brazil: [https://www.mercadopago.com/mlb/ferramentas/aplicacoes](https://www.mercadopago.com/mlb/ferramentas/aplicacoes)
    * México: [https://www.mercadopago.com/mlm/herramientas/aplicaciones](https://www.mercadopago.com/mlm/herramientas/aplicaciones)
    * Venezuela: [https://www.mercadopago.com/mlv/herramientas/aplicaciones](https://www.mercadopago.com/mlv/herramientas/aplicaciones)
    * Colombia: [https://www.mercadopago.com/mco/herramientas/aplicaciones](https://www.mercadopago.com/mco/herramientas/aplicaciones)

```JAVA
import com.mercadopago.MP;

import org.codehaus.jettison.json.JSONException;
import org.codehaus.jettison.json.JSONObject;

MP mp = new MP ("CLIENT_ID", "CLIENT_SECRET");

```

### Get your Access Token:

```JAVA
String accessToken = mp.getAccessToken();

System.out.println(accessToken);
```

<a name="checkout"></a>
## Using MercadoPago Checkout

### Get an existent Checkout preference:

```JAVA
JSONObject preference = mp.getPreference("PREFERENCE_ID");

System.out.println(preference.toString());
```

### Create a Checkout preference:

```JAVA
JSONObject createPreferenceResult = mp.createPreference("{'items':[{'title':'Prueba','quantity':1,'currency_id':'ARS','unit_price':10.5}]}");
System.out.println(createPreferenceResult.toString());
```
<a href="http://developers.mercadopago.com/documentacion/recibir-pagos#glossary">Others items to use</a>

### Update an existent Checkout preference:

```JAVA
JSONObject updatePreferenceResult = mp.updatePreference("PREFERENCE_ID", "{'items':[{'title':'Prueba','quantity':1,'currency_id':'USD','unit_price':2}]}");
System.out.println(updatePreferenceResult.toString());
```

<a name="payments"></a>
## Using MercadoPago Payment

### Searching:

```JAVA
// Sets the filters you want
Map<String, Object> filters = new HashMap<String, Object> ();
   filters.put("site_id", "MLA"); // Argentina: MLA; Brasil: MLB; Mexico: MLM; Venezuela: MLV; Colombia: MCO
   filters.put("external_reference", "Bill001");
        
// Search payment data according to filters
JSONObject searchResult = mp.searchPayment (filters);
JSONArray results = searchResult.getJSONObject("response").getJSONArray("results");

for (int i = 0; i < results.length(); i++) {
    System.out.println(results.getJSONObject(i).getJSONObject("collection").getString("id"));
    System.out.println(results.getJSONObject(i).getJSONObject("collection").getString("external_reference"));
    System.out.println(results.getJSONObject(i).getJSONObject("collection").getString("status"));
}
```

<a href="http://developers.mercadopago.com/documentacion/busqueda-de-pagos-recibidos">More search examples</a>

### Receiving IPN notification:

* Go to **Mercadopago IPN configuration**:
    * Argentina: [https://www.mercadopago.com/mla/herramientas/notificaciones](https://www.mercadopago.com/mla/herramientas/notificaciones)
    * Brasil: [https://www.mercadopago.com/mlb/ferramentas/notificacoes](https://www.mercadopago.com/mlb/ferramentas/notificacoes)
    * México: [https://www.mercadopago.com/mlm/herramientas/notificaciones](https://www.mercadopago.com/mlm/herramientas/notificaciones)
    * Venezuela: [https://www.mercadopago.com/mlv/herramientas/notificaciones](https://www.mercadopago.com/mlv/herramientas/notificaciones)
    * Colombia: [https://www.mercadopago.com/mco/herramientas/notificaciones](https://www.mercadopago.com/mco/herramientas/notificaciones)<br />

```JAVA
// Get the payment reported by the IPN. Glossary of attributes response in https://developers.mercadopago.com
JSONObject payment_info = mp.getPaymentInfo(request.getParameter("ID"));

// Show payment information
if (Integer.parseInt (payment_info.get("status").toString()) == 200) {
    out.print(payment_info.get("response"));
}
```

### Cancel (only for pending payments):

```JAVA
JSONObject result = mp.cancelPayment(request.getParameter("ID"));

// Show result
out.print(result);
```

### Refund (only for accredited payments):

```JAVA
JSONObject result = mp.refundPayment(request.getParameter("ID"));

// Show result
out.print(result);
```
<a href=http://developers.mercadopago.com/documentacion/devolucion-y-cancelacion> About Cancel & Refund </a>
