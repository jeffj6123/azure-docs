---
title: "Quickstart: Call your Bing Custom Search endpoint using Java | Microsoft Docs"
titleSuffix: Azure AI services
description: Use this quickstart to begin requesting search results from your Bing Custom Search instance in Java.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.devlang: java
ms.custom: devx-track-java, mode-api, devx-track-extended-java
ms.author: aahi
---

# Quickstart: Call your Bing Custom Search endpoint using Java

[!INCLUDE [Bing move notice](../bing-web-search/includes/bing-move-notice.md)]

Use this quickstart to learn how to request search results from your Bing Custom Search instance. Although this application is written in Java, the Bing Custom Search API is a RESTful web service compatible with most programming languages. The source code for this sample is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingCustomSearchv7.java).

## Prerequisites

- A Bing Custom Search instance. For more information, see [Quickstart: Create your first Bing Custom Search instance](quick-start.md).

- The latest [Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/index.html).

- The [Gson library](https://github.com/google/gson).

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## Create and initialize the application

1. Create a new Java project in your favorite IDE or editor, and import the following libraries:

    ```java
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLEncoder;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Scanner;
    import javax.net.ssl.HttpsURLConnection;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    ```

2. Create a class named `CustomSrchJava`, and then create variables for your subscription key, custom search endpoint, and search instance's custom configuration ID. You can use the global endpoint in the following code, or use the [custom subdomain](../../ai-services/cognitive-services-custom-subdomains.md) endpoint displayed in the Azure portal for your resource.
    ```java
    public class CustomSrchJava {
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";
        static String searchTerm = "Microsoft";  
    ...
    ```

3. Create another class named `SearchResults` to contain the response from your Bing Custom Search instance.

    ```java
    class SearchResults {
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }
    ```

4. Create a function named `prettify()` to format the JSON response from the Bing Custom Search API.

    ```java
        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## Send and receive a search request 

1. Create a function named `SearchWeb()` that sends a request and returns a `SearchResults` object. Create the request url by combining your custom configuration ID, query, and  endpoint information. Add your subscription key to the `Ocp-Apim-Subscription-Key` header.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            // construct the URL for your search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchTerm, "UTF-8") + "&CustomConfig=" + customConfigId);
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    ...
    ```

2. Create a stream and store the JSON response in a `SearchResults` object.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            ...
            // receive the JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();
        
            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);
            
            stream.close();
            return results;
        }
    ```

3. Print the JSON response.

    ```java
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    ```

4. Run the program.
    
## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](./tutorials/custom-search-web-page.md)
